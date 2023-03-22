---
layout: single
categories: ["Ruby"]
header:
  teaser: /assets/images/posts/rails-logo.jpg
title: "How to Run a Ruby on Rails Application in a Subfolder" 
---

In the past, Rails supported `RAILS_RELATIVE_URL_ROOT` but it seems to break asset delivery with the introduction of webpacker and requires code changes in multiple places to make it work.

# The Solution

The better solution I've found is to wrap all of your routes (`config/routes.rb`) in a `scope`.

```ruby
# config/routes.rb

Rails.application.routes.draw do

  scope(:path => '/railsapp') do
    # Original Routes
    get '/pages/*id' => 'pages#show'
    root to: 'passwords#new'
  end
end
```

With this, the Rails application now serves requests in a subfolder `/railsapp`.  The application and its assets work and with only one change.

So if you are running the development webserver, the URL of the front page will now be something like `http://127.0.0.1:5100/railsapp`

# One Step Further

It's a nice solution but requires code modification.  If you are developing an application to be run by others (such as in an opensource project), then you need a simpler solution: _an environment variable_.

But when that environment variable isn't set, we still want the original paths without issue.

Here's the solution:

```ruby
# config/routes.rb

Rails.application.routes.draw do
  # Create a block that we'll execute in the conditional below
  routes_config = Proc.new {
    # Original Routes
    get '/pages/*id' => 'pages#show'
    root to: 'passwords#new'
  }

  if ENV.key?('CUSTOM_SUBFOLDER')
    scope(:path => ENV['CUSTOM_SUBFOLDER']) do
      routes_config.call
    end
  else
    routes_config.call
  end
end
```

With this, if the environment variable `CUSTOM_SUBFOLDER` is set, then all paths & routes will be pre-pended with the value of the environment variable.  If not, then you still have your original paths.

This solution is in use in [Password Pusher](https://github.com/pglombardo/PasswordPusher).  Want to help out?  Star the Github project to feed the algos.