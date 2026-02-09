---
layout: single
categories: ["Password Pusher"]
title: "OSS Password Pusher: Dropping Official MySQL/MariaDB Support & Future Defaults"
header:
  teaser: /assets/images/naked-logo.png
---

Open source Password Pusher has supported SQLite3, PostgreSQL, and MySQL databases pretty much since the project was created. For some reason, MySQL has always been the troublemaker in terms of problems and bugs, but we still did our best to happily support it throughout the years.

Recently, MySQL/MariaDB has caused yet [another issue](https://github.com/pglombardo/PasswordPusher/issues/3774) that's been blocking the v1.63.0 release. This most recent MySQL/MariaDB issue has forced us to make a change in strategy.

## Current Database Support

Historically, Password Pusher has supported three database options:
- **Ephemeral SQLite3**: Data is lost on container restart (default for quick sharing, testing)
- **PostgreSQL**: Full-featured database for production deployments
- **MySQL/MariaDB**: Alternative production database option

## Why We're Dropping MySQL Support

Bug [#3774](https://github.com/pglombardo/PasswordPusher/issues/3774) has been blocking v1.63.0 for over a month. I've spent several days diagnosing and attempting to fix it, but MySQL continues to cause issues that slow down development and release cycles.

I was originally planning to drop MySQL support sometime next year, but given the time already invested in this bug and the ongoing maintenance burden, we're making this change now in v1.63.0.

## What This Means for Users

**If you're using MySQL/MariaDB:**
- You can stay on v1.62.x and everything will continue working
- From v1.63.0 forward, MySQL will most likely not work
- The MySQL base libraries won't be ripped out—if you want to hack a local solution, the code is still there, but official support is being dropped

**If you're using PostgreSQL:**
- No changes. PostgreSQL support continues as before.

**If you're using SQLite3:**
- No changes. The SQLite3 option continues to work.

## Future Direction

Longer term, we'll be changing the default database to persistent SQLite3. SQLite3 has matured nicely and is now being used in many production applications. This database is embedded with the application on a mounted volume, so no separate or external database resources are needed—just the mounted volume for persistence.

This will allow users to run Password Pusher with a single container and a mounted volume, simplifying deployments significantly.

## Summary

- MySQL/MariaDB support is being dropped in v1.63.0 going forward so we can move faster
- Existing MySQL installations will continue to work with v1.62.x
- Base MySQL/MariaDB libraries remain in v1.63.0 (for now) but there are known bugs
- SQLite3 will eventually become the default database, allowing single-container deployments with a mounted volume
- No change to PostgreSQL support

Thanks all for understanding.  Over the 15 years of the project, I've never had to pull support for a database but at this point the problems with MySQL/MariaDB are affecting the entire community.  We have a backlog of security patches to release and it's been blocked by the MySQL/MariaDB issues.  The change above will greatly simplify things for our limited team and ship new features faster.  Check out the new [Administration Center](https://github.com/pglombardo/PasswordPusher/releases/tag/v1.63.0-g)!
