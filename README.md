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

## Usage

_For full usage guidelines, see the [orb registry listing](https://circleci.com/orbs/registry/orb/medpeer/rails)_

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/medpeer-dev/rails-orbs/.

## License

The Orb is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).
