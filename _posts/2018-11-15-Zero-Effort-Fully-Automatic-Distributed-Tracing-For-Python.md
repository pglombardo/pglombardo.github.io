---
layout: single
categories: ["Instana", "Python"]
classes: wide
header:
  teaser: /assets/images/posts/stan_loves_python_github_social_media_card.png
---

_This post was orginally featured on the [Instana Blog](https://www.instana.com/blog/zero-effort-fully-automatic-distributed-tracing-for-python/)._

Today Instana is proud to announce an industry first - fully automatic distributed tracing and monitoring of Python applications. Seriously, it’s completely automatic and it’s never been done before.

At Instana, monitoring automation and intelligence is our lifeblood. In today’s complex microservice architectures, imposing disruptive manual steps onto an organization for performance visibility is no longer acceptable. Today, many solutions claim <em>automatic instrumentation</em> but don’t mention all of the manual steps (and on-going maintenance) required to achieve that claimed <em>automatic-ness.</em>

The purpose of this post is twofold: First, to illustrate the true cost of adding instrumentation to your application and second, to announce general availability of our new fully automatic Python instrumentation that bypasses all of the identified organizational costs.
<h3>The True Cost of “Non-Automatic” Instrumentation</h3>
The use of microservices has exploded in the last few years and as a corollary, so has overall application complexity. This isn’t limited only to application architectures. That complexity also includes the many platforms and cloud providers in use today. The last thing an organization needs is to add more complexity; <em>organizations need solutions that simplify</em>.

<a class="sp-lightbox" href="http://www.instana.com/media/Instana-Automatic-Microservices-Monitoring-Massive-Scale.png">
<img style="border: 1px solid DarkSlateGray;" src="/assets/images/posts/Instana-Automatic-Microservices-Monitoring-Massive-Scale.png" alt="Instana Automatic Microservices Monitoring Massive Scale" width="730" />
</a>

Figure 1: Screenshot of Instana Infrastructure Map showing massive monitored environment

At Instana, we have unquestionably large customers with very complex application environments built from many code bases. For organizations such as these, to add instrumentation <strong><em>from other APM vendors</em></strong> usually involves the following costly steps:
<ol>
 	<li><strong>Install a package</strong>This sounds benign but for an organization that has <em>30+ code bases</em>, this is not a trivial task. Also consider whether developer resources need to be scheduled. To install a package across an organization, it’s teams and code bases can take weeks to months or more.</li>
 	<li><strong><strong>Configure that package</strong></strong>&nbsp;

Most APM vendors require you to manually name your application (why?), set a customer key, or configure where to report performance instrumentation data.</li>
 	<li><strong>Write Code</strong>Import a package? Add tracing code? Initialize the package in your application? How long will this take to schedule developer time, do this across teams and against 30+ code bases? And further, to QA the changes and move them through the deploy cycle to production?</li>
 	<li><strong><strong>Change Execution Context</strong></strong>&nbsp;

Some vendors even require organizations to change <em>how</em> they run their applications such as adding <em>-javaagent</em> to the command line to point out a custom monitoring jar or even running their entire application inside of a wrapper script (!). Such changes then require investment and ongoing support from your Operations and/or Site Reliability Engineers. Then multiply this effort by the number of code bases and platforms that this applies to…</li>
 	<li><strong><strong>Upgrades</strong></strong>&nbsp;

Bugs do exist and software evolves to offer new features. Need to fix a bug or get a new monitoring feature? Then be prepared to handle all of the above steps again to upgrade and all the (potentially breaking) changes that come with it.</li>
</ol>
What we find here is that in the APM world, the term <em>automatic</em> is used often but when viewed with a modicum of scrutiny as above, the existing market of APM offerings are anything but automatic.

This is what we, at Instana, have been working to improve. To bypass this organizational cost completely. To make monitoring transparent, truly automatic, and continuous for organizations.

<strong><em>To that end, we’ve always offered Python instrumentation that <a href="https://docs.instana.io/ecosystem/python/#activating-without-code-changes" target="_blank" rel="noopener noreferrer">requires no application code changes </a>but today we take that effort one giant leap forward...</em></strong>
<h3>Instana’s <em>Truly</em> Automatic Python Instrumentation</h3>
Today we’re announcing the general availability of our automatic remote Python instrumentation utilizing <a href="/supported-technologies/instana-autotrace/">Instana’s <em>AutoTrace</em> </a>technology.

We don’t require any of the steps identified in the previous section - Instana will now identify your remote Python web servers, apply instrumentation, and start tracing/monitoring automatically.

To be clear, to get this new functionality <em>requires exactly zero steps</em> from your organization beyond installing the base Instana agent:
<ul>
 	<li>No installing packages</li>
 	<li>No modifying code</li>
 	<li>No setting environment variables</li>
 	<li>No deploying code</li>
 	<li>No naming applications</li>
 	<li>No configuring packages</li>
 	<li>No changing command lines</li>
 	<li>No upgrading packages</li>
 	<li>No restarting processes</li>
</ul>
What you get:
<ul>
 	<li>Instant Python gratification</li>
 	<li><a href="http://www.instana.com/blog/application-maps-for-troubleshooting-and-cloud-migration/" target="_blank" rel="noopener noreferrer">Contextual visualization of your Python services</a></li>
 	<li><a href="http://www.instana.com/blog/service-performance-analysis-application-perspectives/" target="_blank" rel="noopener noreferrer">Automatic performance monitoring of those services</a></li>
 	<li><a href="http://www.instana.com/blog/how-automatic-root-cause-analysis-works/" target="_blank" rel="noopener noreferrer">Precise root cause analysis</a></li>
</ul>
With the Instana agent installed on your host, your <em>Python web servers</em> will be continuously detected, instrumented, and simply show up in your Instana dashboard as they come and go - true continuous and automatic performance monitoring of Python.

<strong><em>This makes Instana the first APM vendor to provide fully automatic and continuous remote monitoring and tracing of Python applications.</em></strong>

<em>Note that for this first release, we’re limiting this to just Gunicorn and UWSGI web server processes but will expand this list over time. If you want to jump ahead and add more processes to this list, <a href="https://docs.instana.io/ecosystem/python/configuration/#automatic-instrumentation" target="_blank" rel="noopener noreferrer">see our documentation</a>.</em>

To better visualize the significance of this announcement, let’s compare the true cost of instrumentation of other vendors to Instana. (❎ == Automatic, no work required)
<table style="width: 100%; border: 1px solid darkslategray; border-collapse: collapse;">
<tbody>
<tr style="border: 1px solid darkslategray; border-collapse: collapse;">
<td style="border: 1px solid darkslategray; border-collapse: collapse;"><strong>The True Cost of Instrumentation</strong></td>
<td style="border: 1px solid darkslategray; border-collapse: collapse; text-align: center;"><strong>Other Vendors</strong></td>
<td style="border: 1px solid darkslategray; border-collapse: collapse; text-align: center;"><strong>Instana</strong></td>
</tr>
<tr style="border: 1px solid darkslategray; border-collapse: collapse;">
<td style="border: 1px solid darkslategray; border-collapse: collapse;">Install Software (a package, jar, gem etc.)</td>
<td style="text-align: center; border: 1px solid darkslategray; border-collapse: collapse;">✔</td>
<td style="text-align: center; border: 1px solid darkslategray; border-collapse: collapse;">❎</td>
</tr>
<tr style="border: 1px solid darkslategray; border-collapse: collapse;">
<td style="border: 1px solid darkslategray; border-collapse: collapse;">Configure Software (INI files, YAML files)</td>
<td style="text-align: center; border: 1px solid darkslategray; border-collapse: collapse;">✔</td>
<td style="text-align: center; border: 1px solid darkslategray; border-collapse: collapse;">❎</td>
</tr>
<tr style="border: 1px solid darkslategray; border-collapse: collapse;">
<td style="border: 1px solid darkslategray; border-collapse: collapse;">Set Environment Variables</td>
<td style="text-align: center; border: 1px solid darkslategray; border-collapse: collapse;">✔</td>
<td style="text-align: center; border: 1px solid darkslategray; border-collapse: collapse;">❎</td>
</tr>
<tr style="border: 1px solid darkslategray; border-collapse: collapse;">
<td style="border: 1px solid darkslategray; border-collapse: collapse;">Name the Application</td>
<td style="text-align: center; border: 1px solid darkslategray; border-collapse: collapse;">✔</td>
<td style="text-align: center; border: 1px solid darkslategray; border-collapse: collapse;">❎</td>
</tr>
<tr style="border: 1px solid darkslategray; border-collapse: collapse;">
<td style="border: 1px solid darkslategray; border-collapse: collapse;">Code Changes Required? (import package, init code)</td>
<td style="text-align: center; border: 1px solid darkslategray; border-collapse: collapse;">✔</td>
<td style="text-align: center; border: 1px solid darkslategray; border-collapse: collapse;">❎</td>
</tr>
<tr style="border: 1px solid darkslategray; border-collapse: collapse;">
<td style="border: 1px solid darkslategray; border-collapse: collapse;">Change execution context (run your app inside a wrapper? -javaagent?)</td>
<td style="text-align: center; border: 1px solid darkslategray; border-collapse: collapse;">✔</td>
<td style="text-align: center; border: 1px solid darkslategray; border-collapse: collapse;">❎</td>
</tr>
<tr style="border: 1px solid darkslategray; border-collapse: collapse;">
<td style="border: 1px solid darkslategray; border-collapse: collapse;">Periodically upgrade instrumentation and revalidate all of the above?</td>
<td style="text-align: center; border: 1px solid darkslategray; border-collapse: collapse;">✔</td>
<td style="text-align: center; border: 1px solid darkslategray; border-collapse: collapse;">❎</td>
</tr>
<tr style="border: 1px solid darkslategray; border-collapse: collapse;">
<td style="border: 1px solid darkslategray; border-collapse: collapse;">Restart applications?</td>
<td style="text-align: center; border: 1px solid darkslategray; border-collapse: collapse;">✔</td>
<td style="text-align: center; border: 1px solid darkslategray; border-collapse: collapse;">❎</td>
</tr>
<tr style="border: 1px solid darkslategray; border-collapse: collapse;">
<td style="border: 1px solid darkslategray; border-collapse: collapse;">Periodic package upgrades?</td>
<td style="text-align: center; border: 1px solid darkslategray; border-collapse: collapse;">✔</td>
<td style="text-align: center; border: 1px solid darkslategray; border-collapse: collapse;">❎</td>
</tr>
</tbody>
</table>
<strong><em>P.S. Did you know that we already offer this same Auto-Trace functionality for <a href="https://www.instana.com/supported-technologies/" target="_blank" rel="noopener noreferrer">Java and PHP</a>? Python is just the latest addition. More languages to come!</em></strong>
<h3>As Always - More Python Instrumentation</h3>
Outside of this new functionality, we’re also continually adding new instrumentation to the Python sensor. Most recently we’ve added support for <a href="https://www.sqlalchemy.org" target="_blank" rel="noopener noreferrer">SQL Alchemy</a> and <a href="https://pypi.org/project/asynqp/" target="_blank" rel="noopener noreferrer">Asynqp</a>. Stay tuned because much more is on the way.

<a class="sp-lightbox" href="http://www.instana.com/media/Instana-Python-Distributed-Trace-SQL-Alchemy.png">
<img style="border: 1px solid DarkSlateGray;" src="/assets/images/posts/Instana-Python-Distributed-Trace-SQL-Alchemy.png" alt="Instana Python Distributed Trace SQL Alchemy" width="730" />
</a>

Figure 2: Screenshot of Instana showing a distributed trace of a single request

Have a request for something not covered? <a href="https://support.instana.com/hc/en-us/requests/new" target="_blank" rel="noopener noreferrer">Let us know</a>!
<h3>A Final Word on Instana Auto-Trace for Python</h3>
I hope this post properly illustrates the true cost of non-automatic instrumentation for organizations and how Instana is rewriting the rules for APM. My hope is that this will help more than a few organizations make well informed decisions before choosing a solution that will cost too much organizationally.

We work hard at Instana on extensive, continuous, and automatic monitoring so you don’t have to. You can try Instana Auto-Trace for Python in your own applications today by signing up for a free trial using the “Try Instana” button at the top of this page.