---
permalink: /projects
layout: splash
classes: wide
title: "Projects: Past and Present"
# Repo cards generated from: https://lab.lepture.com/github-cards/#instana/python-sensor|default
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  overlay_image: /assets/images/math.jpg
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
 
excerpt: "Various projects from recent years"
intro: 
  - excerpt: ''
python_sensor_row:
  - image_path: /assets/images/posts/stan_loves_python_github_social_media_card.png
    image_caption: "Instana Python Sensor"
    alt: "Instana Python Sensor"
    title: "Python Performance Instrumentation"
    excerpt: 'Open source distributed tracing and metrics sensor with automatic instrumentation of a large list of popular Python frameworks, libraries and packages and a qualified implementation of the [OpenTracing](https://opentracing.io) API'
    url: "https://github.com/instana/python-sensor"
    btn_label: "Github"
    btn_class: "btn--primary"
ruby_sensor_row:
  - image_path: /assets/images/posts/instana-ruby-code-view.png
    image_caption: "Instana Ruby Sensor"
    alt: "Instana Ruby Sensor"
    title: "Ruby Performance Instrumentation"
    excerpt: 'Open source distributed tracing and metrics sensor with automatic instrumentation of a large list of popular Ruby frameworks, libraries and packages, a proprietary SDK and a qualified implementation of the [OpenTracing](https://opentracing.io) API'
    url: "https://github.com/instana/ruby-sensor"
    btn_label: "Github"
    btn_class: "btn--primary"
go_sensor_row:
  - image_path: /assets/images/posts/instana-go-banner.png
    image_caption: "Instana Go Sensor"
    alt: "Instana Go Sensor"
    title: "Go Performance Instrumentation"
    excerpt: 'Open source distributed tracing and metrics sensor supporting a qualified implementation of the [OpenTracing](https://opentracing.io) API'
    url: "https://github.com/instana/go-sensor"
    btn_label: "Github"
    btn_class: "btn--primary"
crystal_sensor_row:
  - image_path: /assets/images/posts/instana-crystal-sensor.png
    image_caption: "Instana Crystal Sensor"
    alt: "Instana Crystal Sensor"
    title: "Crystal Performance Instrumentation"
    excerpt: 'Open source distributed tracing and metrics sensor supporting a qualified implementation of the [OpenTracing](https://opentracing.io) API and a proprietary SDK'
    url: "https://github.com/instana/crystal-sensor"
    btn_label: "Github"
    btn_class: "btn--primary"

pwpush_row:
  - image_path: /assets/images/posts/pwpush-logo.png
    image_caption: "PasswordPusher"
    alt: "Password Pusher"
    title: "Password Pusher"
    excerpt: 'PasswordPusher is an application to securely communicate passwords over the web. Passwords automatically expire after a certain number of views and/or time has passed'
    url: "https://pwpush.com"
    btn_label: "Go to App"
    btn_class: "btn--primary"
pwpush_alfred_row:
  - image_path: /assets/images/pages/pwpush-alfred.png
    alt: "Password Pusher Alfred Workflow"
    title: "Password Pusher Alfred Workflow"
    excerpt: 'Alfred workflow for PasswordPusher'
    url: "http://www.packal.org/workflow/passwordpusher"
    btn_label: "Packal"
    btn_class: "btn--primary"
gameface_row:
  - image_path: /assets/images/pages/Gameface%20Header%202013-11-14%20at%2017.13.08.png
    alt: "Gameface"
    title: "Gameface"
    excerpt: 'Social networking platform for gamers'
irc2hipchat_row:
  - image_path: /assets/images/pages/irc2hipchat.png
    alt: "IRC2HipChat"
    title: "IRC2HipChat"
    excerpt: 'Log all messages from an IRC room to HipChat'
    url: "/IRC2HipChat/"
    btn_label: "Docs"
    btn_class: "btn--primary"
traceview_row:
  - image_path: /assets/images/pages/traceview-ruby.png
    alt: "TraceView Ruby"
    title: "Tracelytics: Ruby Performance Instrumentation for TraceView"
    excerpt: 'Distributed tracing instrumentation for a large number of Ruby frameworks, libraries and gems'
    url: "https://github.com/tracelytics/ruby-traceview"
    btn_label: "Github"
    btn_class: "btn--primary"
---

{% include feature_row id="intro" type="center" %}

{% include feature_row id="python_sensor_row" type="left" %}

{% include feature_row id="ruby_sensor_row" type="right" %}

{% include feature_row id="go_sensor_row" type="left" %}

{% include feature_row id="crystal_sensor_row" type="right" %}

{% include feature_row id="pwpush_row" type="left" %}

{% include feature_row id="pwpush_alfred_row" type="right" %}

{% include feature_row id="gameface_row" type="left" %}

{% include feature_row id="irc2hipchat_row" type="right" %}

{% include feature_row id="traceview_row" type="left" %}

