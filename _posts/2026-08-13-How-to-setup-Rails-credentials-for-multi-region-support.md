---
layout: single
categories: ["Rails", "Security"]
title: "How to Set Up Rails Credentials for Multi-Region Support"
date: 2026-08-13
header:
  teaser: /assets/images/posts/2025/coding3.png
---

[Password Pusher](https://pwpush.com) runs the same Rails app in the US and EU — [us.pwpush.com](https://us.pwpush.com) and [eu.pwpush.com](https://eu.pwpush.com) — both as `RAILS_ENV=production`.  The app config is shared.  The secrets are not: Stripe, storage, SMTP, encryption keys and the rest are per-region.

Rails credentials are organized by environment (`development`, `test`, `production`).  That doesn't help when you have two production deployments that must not share a master key.

You *could* add `production_us` and `production_eu` as real Rails environments.  Then you duplicate `production.rb`, CI, and every `Rails.env.production?` check in the app.  I didn't want that.

# The Solution

Keep `RAILS_ENV=production`.  Create extra credentials files named after the region, then tell Rails which files to load at boot.

`bin/rails credentials:edit --environment NAME` writes files named after `NAME`.  That name does not have to be a real Rails environment — there is no `config/environments/production-us.rb`.

```bash
bin/rails credentials:edit --environment production-us
bin/rails credentials:edit --environment production-eu
```

That creates:

```
config/credentials/production-us.yml.enc
config/credentials/production-us.key
config/credentials/production-eu.yml.enc
config/credentials/production-eu.key
```

Commit the `.yml.enc` files.  Gitignore the keys:

```gitignore
/config/credentials/*.key
```

Each deploy sets a region environment variable.  In Password Pusher that's `PWPX_REGION` (`us` or `eu`).

In `config/environments/production.rb`:

```ruby
Rails.application.configure do
  raise "PWPX_REGION is not set" if ENV["PWPX_REGION"].blank?

  config.credentials.content_path = "config/credentials/production-#{ENV["PWPX_REGION"].downcase}.yml.enc"
  config.credentials.key_path = "config/credentials/production-#{ENV["PWPX_REGION"].downcase}.key" unless ENV.key?("RAILS_MASTER_KEY")

  # ...the rest of production
end
```

`content_path` is the encrypted YAML.  `key_path` is the decryption key.  See the [Rails guides](https://guides.rubyonrails.org/configuring.html#config-credentials-content-path).  If you point `key_path` at the YAML file, boot will fail in a confusing way.

If `RAILS_MASTER_KEY` is already in the environment (Hatchbox and Kamal set this per app), skip `key_path`.  The US app gets the US key; the EU app gets the EU key.  Don't reuse one master key across regions — that would defeat the split.

To edit later, keep using `--environment production-us` (or `production-eu`).  `RAILS_ENV=production rails credentials:edit` still looks at `production.yml.enc`, and the boot code above will raise if `PWPX_REGION` isn't set.

# Staging

Same idea, still one `RAILS_ENV=staging`:

```bash
bin/rails credentials:edit --environment staging-us
bin/rails credentials:edit --environment staging-eu
```

```ruby
# config/environments/staging.rb
raise "PWPX_REGION is not set" if ENV["PWPX_REGION"].blank?

config.credentials.content_path = "config/credentials/staging-#{ENV["PWPX_REGION"].downcase}.yml.enc"
config.credentials.key_path = "config/credentials/staging-#{ENV["PWPX_REGION"].downcase}.key" unless ENV.key?("RAILS_MASTER_KEY")
```

# Deploy

Each region needs:

1. `RAILS_ENV=production` (or `staging`)
2. `PWPX_REGION=us` or `PWPX_REGION=eu`
3. `RAILS_MASTER_KEY` set to **that region's** key

The encrypted files ship with the git checkout.  The key does not.  If the US master key leaks, EU credentials stay encrypted, and the other way around.

Self-hosted Password Pusher doesn't use this path — operators already configure everything with environment variables.  For the hosted service, credentials files are easier to manage than a long list of per-host env vars.
