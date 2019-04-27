# Notes on testing

There is a set of Cucumber+Capybara scenarios that test all major user-facing flows and most boxoffice-facing flows, and additional specs to fill gaps.  The current configuration uses [CodeClimate Test Reporter](https://docs.codeclimate.com/docs/configuring-test-coverage) (CCTR) as part of the Travis CI flow, and reports combined Cucumber + RSpec coverage to CodeClimate to serve the coverage badge.

Because my `config/application.yml.asc` is GPG-encrypted, Travis expects to find the decryption passphrase in the environment variable `KEY`.  See the main page for how to set up your own `application.yml` which you can version or encrypt or make available to Travis however you like.

## Test database

The versioned file `config/database.yml.test` specifies a SQLite3 database for testing.

## Time

Many features depend on the current time (to test things like reservation
cutoffs, etc.)  **All Cucumber scenarios fix the current date and time
as Jan 1, 2010, 00:00:00 in the application timezone** in
`features/support/env.rb`.  To suppress this for certain scenarios, tag
them with `@time`.

## Stubbing credit card payments

Most scenarios that test payments do stubbing (in `env.rb`) at the level
of the `Store` methods that wrap calls to Stripe.  A few scenarios use
the `FakeStripe` gem.
