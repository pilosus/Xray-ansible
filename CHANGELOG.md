# Change Log

All notable changes to this project will be documented in this
file. This change log follows the conventions of
[keepachangelog.com](http://keepachangelog.com/).

## [Unreleased]

Nothing here yet.

## [0.1.0] - 2023-10-26

The release is done by [@pilosus](https://github.com/pilosus)

### Added

- Documentation

### Fixed

- Relative path to a role in the playbook
- Bug in a new version installation fixed for the case when no
  previous version was found (absence of a version file)
- Systemd service gets restarted if a new version is deployed

## 0.0.0 - 2023-10-25

The release is done by [@dmgening](https://github.com/dmgening)

### Added
- `xray` Ansible role
- Ansible playbook to set up Xray

[Unreleased]: https://github.com/pilosus/dienstplan/compare/0.1.0...HEAD
[0.1.0]: https://github.com/pilosus/dienstplan/compare/0.0.0...0.1.0
