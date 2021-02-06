There is a set of Cucumber+Capybara scenarios that test all major user-facing flows and most boxoffice-facing flows, and additional specs to fill gaps.  The current configuration uses [CodeClimate Test Reporter](https://docs.codeclimate.com/docs/configuring-test-coverage) (CCTR) as part of the Travis CI flow, and reports combined Cucumber + RSpec coverage to CodeClimate to serve the coverage badge.

## PhantomJS

You need a local install of `phantomjs` to run the JavaScript-dependent scenarios.  On MacOS, you can install it
via Homebrew; on *nix, use your favorite package manager.  Capybara needs to be able to find the `phantomjs` executable
so make sure it is in your default execution path.

## Test database

The versioned file `config/database.yml.test` specifies a SQLite3 database for testing.
Once you have created a proper `config/application.yml` as described on the main page, you should be able to successfully
run all scenarios and all specs.  A local run takes 12-15 minutes to run all tests.

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

## Travis CI setup

The tests rely on the values of `config/application.yml`, so you need to set these environment variables in the Travis
repo as well.  Fortunately
Audience1st includes a convenient `rake` task to do this.  You only need to do this 
once, or whenever the secrets' values change:

1. Assuming you normally login to Travis using GitHub credentials, in your GitHub account [create a personal access token](https://github.com/settings/tokens) with the [minimal required scopes (capabilities)](https://docs.travis-ci.com/user/github-oauth-scopes/#repositories-on-httpstravis-ciorg) for public repos.

2. Use this token to create a Travis access token:

`travis login --org --github-token` _your-GitHub-token_

`travis token`

Save the value of this Travis token somewhere safe.

3. To propagate all values from `config/application.yml` to Travis, say

`RAILS_ENV=test TRAVIS_TOKEN=`_your-Travis-token_ `REPO=`_your-repo-name_ `rake figaro:travis:set`

`REPO` should be whatever your repo is named in Travis, something like `your-github-username/audience1st`.

You should now be able to do successful CI runs in Travis.