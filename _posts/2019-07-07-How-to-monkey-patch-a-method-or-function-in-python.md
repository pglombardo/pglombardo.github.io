---
layout: single
categories: ["Python"]
classes: wide
header:
  teaser: /assets/images/posts/stan_loves_python_github_social_media_card.png
title: "How to wrap (monkey patch) methods or functions in Python"
---

You can wrap any function or method in Python using [wrapt](https://github.com/GrahamDumpleton/wrapt).  Some use the builtin [functools](https://docs.python.org/3.7/library/functools.html) but I've found wrapt to be the better choice for many reasons - most of which is the ease, simplicity and consistency of use.

Wrapping (or monkey patching) is often done for various reasons such as:

* Adding performance measurement code
* Adding debug code to investigate a problem
* To augment or override functionality

Here's how:
```sh
pip install wrapt
```

```python
import wrapt
import urllib3

@wrapt.patch_function_wrapper('urllib3', 'HTTPConnectionPool.urlopen')
def urlopen_with_instana(wrapped, instance, args, kwargs):
      # Perform any required steps beforehand

      rv =  wrapped(*args, **kwargs)
      
      # Post method steps

      # Always make sure to return the original return value or
      # we change the behavior of the method or function that we're
      # wrapping and potentially break callers
      return rv
    except Exception as e:
      # Handle, log or print the exception
      #
      # Make sure to continue to raise the exception so as to not
      # change the behavior of the method or function that
      # we are wrapping.
      raise
```

With this, every call to `urllib3.HTTPConnectionPool.urlopen()` will be intercepted by `urlopen_with_instana` which allows us to add pre, post and exception handling code execution.

This is the pattern we use to intercept Python Redis calls in the [Python sensor for Instana](https://github.com/instana/python-sensor).

_The following example uses [OpenTracing APIs](https://opentracing.io) for a distributed tracing implementation._

```python
@wrapt.patch_function_wrapper('redis.client','StrictRedis.execute_command')
def execute_command_with_instana(wrapped, instance, args, kwargs):
    parent_span = tracer.active_span

    # If we're not tracing, just return
    if parent_span is None or parent_span.operation_name == "redis":
        return wrapped(*args, **kwargs)

    with tracer.start_active_span("redis", child_of=parent_span) as scope:
        try:
            collect_tags(scope.span, instance, args, kwargs)
            if (len(args) > 0):
                scope.span.set_tag("command", args[0])

            rv = wrapped(*args, **kwargs)
        except Exception as e:
            scope.span.log_exception(e)
            raise
        else:
            return rv
```