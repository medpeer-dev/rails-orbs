[![CircleCI](https://circleci.com/gh/medpeer-dev/rails-orbs.svg?style=svg)](https://circleci.com/gh/medpeer-dev/rails-orbs)
[![CircleCI Orb Version](https://img.shields.io/endpoint.svg?url=https://badges.circleci.io/orb/medpeer/rails)](https://circleci.com/orbs/registry/orb/medpeer/rails)

# Rails Orb

An Orb for Rails projects.

## Description

There are some tips for testing Rails projects on CircleCI.

- Cache gems and npm packages as much as possible
- Prepare assets before running tests in parallel
- Restore only the cache needed for each job

Rails Orb provides commands to write these concisely.

## Example

```yaml
version: 2.1

orbs:
  rails: medpeer/rails@x.y.z
  ruby: circleci/ruby@0.2.1

executors:
  ruby:
    docker:
      - image: &docker_ruby circleci/ruby:2.7.0-node-browsers
  ruby_with_db:
    docker:
      - image: *docker_ruby
      - image: circleci/postgres:12.1-alpine-ram
    environment:
      DATABASE_URL: 'postgres://postgres:postgres@127.0.0.1:5432'

jobs:
  rb-deps:
    executor: ruby
    steps:
      - checkout

      # Install gems and save cache
      - rails/bundle-install

  js-deps:
    executor: ruby
    steps:
      - checkout

      # Install npm packages and save cache
      - rails/yarn-install

  js-test:
    executor: ruby
    steps:
      - checkout

      # Restore npm packages from cache
      - rails/yarn-install

      # Test with Yarn
      - run: yarn test

  assets:
    executor: ruby
    steps:
      - checkout

      # Restore gems and npm packages from caches
      - rails/bundle-install
      - rails/yarn-install

      # Precompile assets and save cache
      - rails/assets-precompile

  rspec:
    executor: ruby_with_db
    parallelism: 4
    steps:
      - checkout

      # Restore files from caches
      - rails/bundle-install
      - rails/yarn-install
      - rails/assets-precompile

      # Wait for DB
      - run: dockerize -wait tcp://localhost:5432 -timeout 1m

      # Database setup
      - run: bin/rails db:create db:test:prepare

      # Test with RSpec
      - ruby/run-tests

workflows:
  rspec:
    jobs:
      - rb-deps
      - js-deps
      - js-test:
          requires:
            - js-deps
      - rails/assets:
          requires:
            - rb-deps
            - js-deps
      - rspec:
          requires:
            - assets
```

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/medpeer-dev/rails-orbs/.

## License

The Orb is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).
