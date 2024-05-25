# Change Log

All notable changes to this project will be documented in this
file. This change log follows the conventions of
[keepachangelog.com](http://keepachangelog.com/).

## [Unreleased]

## [0.4.0] - 2024-05-25

### Changed

- PostgreSQL setup playbook now creates a user as `superuser` to
  support migrations (require DDL commands).

- Ansible version bumped from 8.2.0 to 9.6.0

## [0.3.0] - 2023-08-03

### Added

- Support for `dienstplan` scheduling worker using `systemd timers`
  ([#8](https://github.com/pilosus/dienstplan-deploy/issues/8))
- Support for Java system properties
  ([#10](https://github.com/pilosus/dienstplan-deploy/issues/10))

### Changed

- Template files and respective vars naming for Jetty server changed
  for consistency
- Bumped `ansible` version to 8.2.0
  ([#9](https://github.com/pilosus/dienstplan-deploy/issues/9))

## [0.2.0] - 2022-04-16

### Added
- PostgreSQL server provisioning playbook

## [0.1.0] - 2022-01-09

### Added
- Server provisioning playbook
- App provisioning playbook
- App deploy playbook

[Unreleased]: https://github.com/pilosus/dienstplan/compare/0.4.0...HEAD
[0.4.0]: https://github.com/pilosus/dienstplan/compare/0.3.0...0.4.0
[0.3.0]: https://github.com/pilosus/dienstplan/compare/0.2.0...0.3.0
[0.2.0]: https://github.com/pilosus/dienstplan/compare/0.1.0...0.2.0
[0.1.0]: https://github.com/pilosus/dienstplan/compare/0.0.0...0.1.0
