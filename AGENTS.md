# Repository guide

## Layout and setup

This Ruby gem adapts inbound Amazon SES/SNS email for Griddler. `lib/griddler/ses/` contains the adapter, middleware, configuration, and notification preparation; `message_content/` separates inline payload and S3 fetching. Specs under `spec/` include an S3 client double, so unit tests should not need live AWS access.

Use `bundle install` with `Gemfile` and `griddler-ses.gemspec`. The legacy `circle.yml` specifies Ruby 2.2.4; the gemspec requires Bundler ~> 1.11, Rake ~> 10.0, RSpec ~> 3.0, and AWS SDK ~> 2.3.12. No lockfile is tracked. Report dependency/runtime incompatibilities rather than treating current Ruby/Bundler as a verified replacement.

`bundle exec rake` runs the default RSpec task; use `bundle exec rspec spec/path_spec.rb` for focused changes. `bundle exec rake build` packages the gem locally. No separate lint script or application server is defined. The README's SES/SNS setup is integration guidance for a consuming application, not a local test prerequisite.

## Completion and boundaries

Start with `git status --short` and preserve unrelated work. Carry authorized local changes through relevant verification and repair, choosing ordinary reversible implementation details directly. Preserve payload handling for both inline and S3-backed messages and add focused regression tests when behavior changes. Use synthetic email and AWS doubles; do not expose raw email, access keys, or private S3 objects.

AWS receipt rules/subscriptions, sending email, live object reads, and gem publishing need authorization for the specific action. If a dependency, missing integration environment, or unresolved product decision blocks progress, name the exact prerequisite and continue independent work. For prose-only changes, inspect referenced paths and run `git diff --check`. Close with changed paths, checks actually run/results, and unverified integration behavior.
