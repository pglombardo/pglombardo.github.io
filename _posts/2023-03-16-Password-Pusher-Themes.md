---
layout: single
categories: ["Password Pusher"]
title: "Password Pusher Themes!"
header:
  teaser: https://pwpush.fra1.cdn.digitaloceanspaces.com/themes%2Fquartz-theme-pwpush.com.png
---

After many requests Password Pusher now ships with 26 built in themes!

---> Go see the [Theme Gallery](https://github.com/pglombardo/PasswordPusher/blob/master/Themes.md#theme-gallery)

And more, we also now support custom CSS.

# How Configure the Password Pusher Theme

The themes in Password Pusher come from the great [Bootswatch](https://bootswatch.com) project and are unmodified.  They are licensed under the MIT License.

To use, select your theme and set the following environment variables:

```bash
export PWP__THEME=quartz
export PWP_PRECOMPILE=true
```

With this you get:

![](https://pwpush.fra1.cdn.digitaloceanspaces.com/themes%2Fquartz-theme-pwpush.com.png)

See also:

* [Theme Configuration Documentation](https://github.com/pglombardo/PasswordPusher/blob/master/Configuration.md#themes)

# How to add Custom CSS

Password Pusher supports adding custom CSS to the application. The application hosts a `custom.css` file located at `app/assets/stylesheets/custom.css`. This file is loaded last so it take precedence over all built in themes and styling.

This file can either be modified directly or in the case of Docker containers, a new file mounted over the existing one.

When changing this file inside a Docker container, make sure to set the precompile option `PWP_PRECOMPILE=true`. This will assure that the custom CSS is incorporated correctly.

An example Docker command to override that file would be:

```bash
docker run -e PWP_PRECOMPILE=true --mount type=bind,source=/path/to/my/custom.css,target=/opt/PasswordPusher/app/assets/stylesheets/custom.css -p 5100:5100 pglombardo/pwpush-ephemeral:release
```

or the `docker-compose.yml` equivalent:

```yaml
version: '2.1'
services:

  pwpush:
    image: docker.io/pglombardo/pwpush-ephemeral:release
    ports:
      - "5100:5100"
    environment:
      PWP_PRECOMPILE: 'true'
    volumes:
      - type: bind
        source: /path/to/my/custom.css
        target: /opt/PasswordPusher/app/assets/stylesheets/custom.css
```

Remember that when doing this, this new CSS code has to be precompiled.

To do this in Docker containers, simply set the environment variable `PWP_PRECOMPILE=true`. For source code, run `bin/rails assets:precompile`. This compilation process will incorporate the custom CSS into the updated site theme.

See also:

* [How to Add Custom CSS](https://github.com/pglombardo/PasswordPusher/blob/master/Configuration.md#themes)


