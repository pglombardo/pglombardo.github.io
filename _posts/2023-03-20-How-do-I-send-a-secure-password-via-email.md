---
layout: single
categories: ["Password Pusher"]
classes: wide
title: "How do I send a secure password via email?"
header:
  teaser: /assets/images/posts/2023/email.png
---

Do you need to send a secure password or clip of text securely?  If you send it by email, it could live in email archives forever.

Further, email is inherently secure.  A sent email can be intercepted and copied an unlimited amount of times before it arrives at it's destination.

The simple answer is to not send the password directly.  Send an auto-expiring link to the password with a tool like [Password Pusher](https://pwpush.com).

With this, you can send a link like this over email.

```
https://pwpush.com/en/p/i3x-ya9jtymohpg
```

And when clicked, the person you sent this link to sees:

![](/assets/images/posts/2021/pwpush/pwpush-payload-simple.png)

Links to passwords automatically expire after a preset number of views or duration of time.  Further, you can maintain an audit log to see who viewed the password, when and from where.

![](/assets/images/posts/2021/pwpush/pwpush-audit-log.png)

Save yourself the headache and don't send sensitive information over email.