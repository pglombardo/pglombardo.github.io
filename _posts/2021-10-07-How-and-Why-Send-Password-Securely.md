---
layout: single
categories: ["Password Pusher"]
classes: wide
header:
  teaser: /assets/images/posts/2021/send-secure-laptop.png
  image: /assets/images/posts/2021/send-secure-laptop.png
title: "How to Securely Send Passwords or Other Confidential Information"
toc: false
---

Do you have to send something confidential to a colleague, friend or associate?  Don't let that information persist indefinitely.

Be careful how and what you transmit as that information can sit around forever and be exploited by others long after you've forgotten what you've sent.

# Historical Methods

When having to send credentials, most think of email immediately and if not that, then one of the many chat systems as a mode of transmission.  These systems have inherent security drawbacks that I'll detail here as some are not immediately evident.

## Email

There are so many downsides to email that I'll just list them in short order:

1. Email is inherently insecure and can be intercepted at multiple points of delivery by malicious entitities.
2. Emailed passwords are usually sent with context to what they go to which is equivalent to a free tip to any malicious entities.
3. The email addresses and headers can give a clue to what the password may go to (such as company or corporation).
4. Emailed passwords live in perpetuity (read: forever) in email archives.
5. Passwords in email can be retrieved and used months or years later if an email account is eventually compromised.
6. After an email is sent, you have zero control on the where, when and how long that email will exist for.

## WhatsApp, Facebook Messenger, AIM, ICQ, Signal, Telegram, Slack, Microsoft Teams

Some chat systems are more secure than others but in the end they are all owned by companies and corporations who are bound by law and all types of policies.  The trustworthiness in these chat systems go only as far as you can trust those companies.

Some of the drawbacks of transmitting over chat systems:

1. A compromised account can access sensitive information posted months or years ago
2. Companies and organizations are often required by law to maintain chat logs for some period of time.
3. Chat systems leave a local cache on devices that can often be accesses outside of the chat software.

Or the less evident issue is the catch-22:  It's often that the user you want to send information to doesn't have access to chat until you send him the password.

# What Would Be The Best Case Scenario?

So given all of the complexities and drawbacks above, what would be the ideal solution?

The best case scenario for transmitting a secure password would have the following features:

1. The location of the password would be secret and known only to you and whom you communicate it to.
2. The password would be stored encrypted and if transmitted, over an encrypted line (HTTPS).
3. If left untouched, the password would automatically expire and delete itself after a number of views or days has passed.
4. The end user can delete the password as acknowledgement once they successfully retrieve it.
5. You can see and track who viewed (or attempted to view) the password.
6. You could manually delete the password entirely yourself at anytime.
7. You could maintain a detailed audit log of events for that password even though the password itself has been deleted.

# Password Pusher

To satisfy this best case scenario, I've created [Password Pusher](https://pwpush.com) as an opensource project [available on Github](https://github.com/pglombardo/PasswordPusher).

I run this software at [pwpush.com](https://pwpush.com) but since it's opensource and you can easily run your own private internal instance in which I'll explain how below.

After 10 years of running, maintaining and improving this project, it is the best solution out there by far to communicate passwords securely.

With Password Pusher, the password is stored at a secret URL which then expires after a certain number of views or days (whichever occurs first).

Here are some of the highlights.

## Passwords Expire After a Number of Views or Days

> If left untouched, the password would automatically expire and delete itself after a number of views or days has passed.

Pushed passwords are stored encrypted in the database and have an expiration of days and views.  When either of these triggers first, the password is entirely deleted from the database.

![](/assets/images/posts/2021/pwpush/pwpush-expiration-settings.png)

## Unbranded Secret URL and Simple Delivery Page

> The location of the password would be secret and known only to you and who you communicate it to.
>
> The password would be stored encrypted and if transmitted, over an encrypted line (HTTPS).

Post a password and get a simple unbranded password delivery page at a secret HTTPS URL.

No branding and no outgoing links to confuse end users.  Simple instructions indicating best practices for the end user.

![](/assets/images/posts/2021/pwpush/pwpush-payload-simple.png)

## User Deletion of Passwords

> The end user can delete the password as acknowledgement once they successfully retrieve it.

End users can optionally delete the password themselves once retrieved.  Once this is done, the password no longer exists and can never be retrieved again.

![](/assets/images/posts/2021/pwpush/pwpush-user-delete.png)

## Full Audit Log Tracking

> You can see and track who viewed (or attempted to view) the password.
>
> You could maintain a detailed audit log of events for that password even though the password itself has been deleted.

For each password posted, you get a full audit log from creation, expiration, deletion and beyond.  We even show you failed view attempts and where they came from.

![](/assets/images/posts/2021/pwpush/pwpush-audit-log.png)

## Dashboard to Track Pushed Passwords

Track what you push out with a dashboard that allows you to see what's active and what has already expired.

![](/assets/images/posts/2021/pwpush/pwpush-dashboard.png)

## Internationalization: All the Languages

Do you need to send something to an international user?  Set the target language for your push ahead of time.

![](/assets/images/posts/2021/pwpush/pwpush-target-language.png)

## Optional 1-click Preliminary Retrieval Step

Want to avoid URL scanners?  An optional 1-click preliminary step is available to avoid bots.

![](/assets/images/posts/2021/pwpush/pwpush-preliminary-step.gif)

## Dark Theme

A dark theme is also included too that follows your operating system settings.

![](/assets/images/posts/2021/pwpush/pwpush-dark-theme-flip.gif)

## Run Your Own Private Instance

Many companies and organizations have security policies which preclude them from using public services such as [pwpush.com](https://pwpush.com).  As such, Password Pusher is available to run internally at your organization with little to no effort.

`docker run -d -p "5100:5100" pglombardo/pwpush-ephemeral:1.10.0`

Password Pusher continually provides updated containers available on [Docker Hub](https://hub.docker.com/u/pglombardo).  There is also [documentation](https://github.com/pglombardo/PasswordPusher#-run-your-own-instance) on how to run it on a variety of other platforms.

Docker, Kubernetes, Digital Ocean, ECS we support it all and even have user integrations into Slack, ZenDesk, Microsoft Teams, Microsoft Active Directory and more.

There are [dozens of private Password Pusher instances running](https://duckduckgo.com/?q=%22go+ahead.+email+another+password%22&ia=web) in the wild and I welcome them all.

# Summary

Sending passwords is a critical operation that is often overlooked and should be taken seriously.  With all of the other things going on in your life, job and workplace, let Password Pusher worry about the details for you.  Use tools that were built from the ground up with security in mind.

If you have any issues using Password Pusher or running it internally, please feel free to contact me [on Github](https://github.com/pglombardo/PasswordPusher/issues/new) or [through pwpush.com](https://pwpush.com/en/feedbacks/new).

[https://pwpush.com/en/p/86186h565jvtvf5j/r](https://pwpush.com/en/p/86186h565jvtvf5j/r)