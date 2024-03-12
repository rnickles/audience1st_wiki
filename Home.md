# Preparing to develop

In addition to the app, you will need a Stripe account, though you can 
use just the test-mode keys during development. (Not needed to get the app initially stood up.)

You will need `chromedriver` and `google-chrome` to run the tests. To install chromedriver run
```
wget https://edgedl.me.gvt1.com/edgedl/chrome/chrome-for-testing/121.0.6167.85/linux64/chromedriver-linux64.zip

unzip chromedriver_linux64.zip

sudo mv chromedriver /usr/local/bin

sudo chmod +x /usr/local/bin/chromedriver

chromedriver --version
```
To install google-chrome run (based on https://tecadmin.net/how-to-install-google-chrome-on-ubuntu-22-04/)
```
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb

sudo dpkg -i google-chrome-stable_current_amd64.deb

google-chrome --version
```

Once you have the above, run `bundle install --without production` and ensure all gems get installed.

Before running the app or its tests though, you must setup multi-tenancy and the secrets file.

## Multi-tenant setup and application secrets

**This is important.** By default Audience1st is designed to be setup
as [multi-tenant using the `apartment`
gem](https://github.com/influitive/apartment), where each theater is a
tenant.  Audience1st determines the tenant name for a given request
fomr the first subdomain in the URI, e.g. if your deployment domain is
`somewhere.com`, then `my-theater.somewhere.com` selects `my-theater`
as the tenant for that request.

For development or staging, the recommended approach is to setup a
single tenant.  In this example we will call it `my-tenant-name`; you can
call it whatever you want, but if you deploy to Heroku for staging,
the app name `my-tenant-name.herokuapp.com` must exist, so choose
the name carefully.

In addition, Audience1st uses the [`figaro` gem](https://github.com/laserlemon/figaro) to manage
configuration variables and secrets, many of which are *necessary* to run the app 
in production or development and even to run the tests.

Therefore you must create a file containing the values of these secrets, some of which values 
may differ between production and development/testing.

1.  Create a file `config/application.yml` containing the following:

```yaml
tenant_names: my-tenant-name
session_secret: "exactly 128 random ASCII characters"
attr_encrypted_key: "exactly 32 random characters"
STRIPE_KEY: "Publishable key from a Stripe account in test mode"
STRIPE_SECRET: "Secret key from a Stripe account in test mode"
MAILGUN_SMTP_LOGIN: postmaster@mail.yourdomain.com
MAILGUN_SMTP_PASSWORD: "your long SMTP password here"
```

Note 1: If the Mailgun-related keys are omitted, you won't be email to send transactional
emails from the app, which may be fine.

Note 2: In a production setting, you'd have several tenant names separated by
commas.

Note 3: If you get the error `ArgumentError: key must be 32 bytes or longer` that means you should place an actual 32-bit string in `attr_encrypted_key`. That is the only one needed to get going. 

**Please don't version the `config/application.yml` file or include it in pull requests, nor
modify the existing `config/application.yml.asc`.  The `.gitignore` is
deliberately set to ignore `config/application.yml` when versioning.** 

2. Create a `config/database.yml` file (and don't version it; it is
also git-ignored) containing `development:` and
`test:` targets:

```yaml
development:
  adapter: sqlite3
  database: db/my-tenant-name.sqlite3
test:
  adapter: sqlite3
  database: db/test.sqlite3
```

(The `production` configuration, if any, depends on your deployment
environment.  Heroku ignores any `production` configuration because it
sets its own using PostgreSQL.)

3.  After running `bundle` as usual, you can run `bundle exec rake
db:schema:load` to load the database schema into each tenant.

4.  Run `rake db:seed` on the development database,
which creates a few special users, including the administrative user
`admin@audience1st.com` with password `admin`. If you get this error, ignore it and proceed.  
```
rake aborted!
ActiveRecord::RecordInvalid: Validation failed: Full name has already been taken, Short name has already been taken
```
  

5.  To start the app, say `rails server webrick`  (assuming you
want to use the simpler Webrick server locally; the `Procfile` uses 
a 2-process Puma server for the production environment currently), but in your
browser, **do not** try to visit `localhost:3000`; instead visit
`http://my-tenant-name.lvh.me:3000` since the multi-tenant
selection relies on the first component of the URI being the tenant
name.  This uses the [free lvh.me
service](https://nickjanetakis.com/blog/ngrok-lvhme-nipio-a-trilogy-for-local-development-and-testing#lvh-me)
that always resolves to `localhost`.

6.  **WARNING.** When you run `rails console` to get a REPL, the first thing you should type
in the console is `Apartment::Tenant.switch! 'my-tenant-name'` to switch to the (only) tenant's
database schema.

7.  The app should now be able to run and you should be able to login
with the administrator password.  Later you can designate other users as administrators.

8.  If you want fake-but-realistic data, also run the task
`TENANT=my-tenant-name bundle
exec rake staging:initialize`.  This creates a bunch of fake users,
shows, etc., courtesy of the `faker` gem.

At this point you should be able to run tests locally (`rake cucumber` and `rake rspec`).
Since the tests also rely on the value of the application secrets, you need to 
set those values in the repo's environment variable settings in Travis CI. See
the Testing page in this wiki for how to do that and for more details on the test suite.

Note 1: If you're getting a bunch of `sqlite3_adapter.rb:26: warning: rb_check_safe_obj will be removed in Ruby 3.0` warnings, placing `RUBYOPT="-W0"` at the beginning of the testing command will suppress the warning and provide cleaner output.   

Note 2: If you're getting the error `unknown error: cannot find Chrome binary`, make sure `chromedriver` and `google-chrome` are installed.

Note 3: If you're getting the error `Real HTTP connections are disabled. Unregistered request: GET https://storage.googleapis.com/chrome-for-testing-public/121.0.6167.85/linux64/chromedriver-linux64.zip with headers {'Accept'=>'/', 'Accept-Encoding'=>'gzip;q=1.0,deflate;q=0.6,identity;q=0.3', 'Host'=>'storage.googleapis.com', 'User-Agent'=>'Ruby'}` it's possibe you need to modify `features/support/webmock.rb` to explicitly allow `storage.googleapis.com`:
```
# The fix, specifically the allow entry
require 'webmock/cucumber'
WebMock.enable!
WebMock.disable_net_connect!(
  net_http_connect_on_start: true, # @see https://github.com/bblimke/webmock/blob/master/README.md#connecting-on-nethttpstart
  allow_localhost: true,
  #  186585553: When upgrade to Ruby3.0, stop requiring webdrivers gem, upgrade to Selenium 4.11+,
  #   and hopefully remove the whitelist URIs below
  allow: ["googlechromelabs.github.io", 'edgedl.me.gvt1.com', "storage.googleapis.com"]
)
```

# Deploying to production or staging

These instructions are for Heroku and assume that you have created a
Heroku app container and provisioned it with the basic (free) level of
Heroku Postgres.  You can adapt these instructions for other
deployment environments.

1. Get the code pushed to the deployment environment (`git push heroku
master` usually). If using Heroku, `figaro heroku:set -e production` to make
`application.yml`'s environment variables available to Heroku,
including the value of `tenant_names`.  For staging-type deployments to Heroku, the correct
value is the Heroku appname, so if your app is
`luminous-coconut.herokuapp.com`, the `tenant_names` environment
variable should be set to `luminous-coconut`.

Note 1: If you get this error 
```
remote:  !     The Ruby version you are trying to install does not exist on this stack.
remote:  !     
remote:  !     You are trying to install ruby-2.7.7 on heroku-22.
```
that means `heroku` is using the wrong stack. Fix it with `heroku stack:set heroku-20`. 

Note 2: If you get this error
```
remote:        rake aborted!
remote:        PG::ConnectionBad: connection to server at "127.0.0.1", port 5432 failed: Connection refused

```
that means the `heroku` app container hasn't been provisioned. Fix it with `heroku addons:create heroku-postgresql`.

Note 3: If you get this error 
```
remote:        rake aborted!
remote:        Figaro::MissingKey: Missing required configuration key: "tenant_names"

```
that means figaro needs to be configured for heroku. Do `figaro heroku:set -e production`.

2. Ensure that the `config/application.yml` on your development
computer contains the correct configuration data.

3.  If this is the first deployment, you have to create the tenant(s) schema(ta).  
To do this, first do `heroku run rake db:schema:load` to create and seed
Postgres' `public` schema.  The `public` schema is not actually used by any
Audience1st tenant but it is cloned whenever a new tenant is created and must be present
or migrations will fail.  Next say 
`heroku run TENANT=my-tenant-name rake a1client:create`, which will create the
schema for `my-tenant-name`.  (The code for the Rake task in in `lib/tasks/client.rb`.)
Finally, `heroku run rake db:migrate` to ensure the schema is up-to-date.
From now on `rake db:migrate` and other database-related tasks will automatically
be applied to all tenants.  Repeat the `a1client:create` task any time you need to
add a tenant.  There is also an `a1client:drop` task to delete a tenant's schema and 
all of its data.  Whenever the list of tenants changes, don't forget to also adjust
the value of the `tenant_names` variable.

4. If the environment variable `EDGE_URL` is set on Heroku,
`config.action_controller.asset_host` will be set to that value to
serve static assets from a CDN, which you must configure (the
current deployment uses the Edge CDN add-on for Heroku, which uses
Amazon CloudFront as a CDN).  If not set, assets will be served the
usual way without CDN.  (If you're just deploying a staging server,
you should not set this variable.)

5. The task `Customer.notify_upcoming_birthdays` emails an administrator or boxoffice manager with information about customers whose birthdays are coming up soon.  The threshold for "soon" can be set in Admin > Options.

## Integration: Sending transactional email in production

In production, email confirmations are sent for various things.
Audience1st is configured to use Mailgun.  If you do nothing,
transactional emails will be suppressed in your staging/production
environment.  If you want to use
Mailgun for real email sending in your staging/production app, do the following:

1. Provision the Mailgun add-on for Heroku and obtain the necessary credentials.

2. `config/application.yml` file should contain the Mailgun-related keys above.
You may need to `figaro heroku:set -e production` to get the key values into Heroku's production environment.

3. Login to Audience1st as
an administrator, go to Options, and enter the Mailgun domain
(i.e. the domain from which transactional emails will appear to come,
usually something like `your-app.herokuapp.com` for a staging
environment).

4.  Be sure that same domain name appears among the "allowed domains"
in the Mailgun settings.  You'll have to set up various DNS entries to support DKIM as well.

## Production cron jobs

The main production deployment of Audience1st runs two periodic jobs using Heroku Scheduler:

1. Daily around 1AM Pacific: `./bin/backup_postgres_to_s3`, a shell script that encrypts the entire Postgres database with `BACKUP_GPG_KEY` and backs it up to S3 bucket `BACKUP_S3_BUCKET` with a filename based on the current date.

2. Every 10 minutes: `NEW_RELIC_AGENT_ENABLED=false rake a1:restart_if_memory_exceeded`, a Rake task that uses the Heroku API to check if any dynos have exceeded their memory quota (i.e. are swapping) and does a restart-all-dynos if so.  This relies on enabling [Heroku runtime metrics sampling](https://devcenter.heroku.com/articles/log-runtime-metrics) on the app.  Empirically, under heavy usage, the 512MB dynos start to swap and I need to figure out what allocation pattern causes this.

# To disable or change multi-tenancy

This requires removing a few files.  **Do not make any PRs that delete those files** since we need
them in the main/production version.  

1. Remove `gem 'apartment'` from the `Gemfile` before running `bundle
install`

2. Remove the file `config/initializers/apartment.rb`

3. Make sure your `config/application.yml` does **not**
contain any mention of `tenant_names`

# To change the tenant selection scheme

If you decide to use multi-tenancy but change the
tenant-selection scheme in `config/initializers/apartment.rb` 
(see the `apartment` gem's documentation for
what this means), you'll also need to edit the before-suite logic in
`features/support/env.rb` and `spec/support/rails_helper.rb`.  Those
bits of code ensure that testing works properly with multi-tenancy
enabled, but they rely on the tenant name being the DNS subdomain.  If
you don't know what this means, you should probably ask for assistance
deploying this software. :-)
