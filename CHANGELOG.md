# BAS Systems Inventory (`bas-systems-inventory`) - Change log

All notable changes to this role will be documented in this file.
This role adheres to [Semantic Versioning](http://semver.org/spec/v2.0.0.html).

## [Unreleased][unreleased]

## 0.1.3 - 26/02/2016

### Fixed

* Reloading facts before looking up host information for where the hostname has changed but the old value is cached

## 0.1.2 - 25/02/2016

### Fixed

* Bug which meant Ansible was unable to predictably make the relevant System Instance the first in an array

## 0.1.1 - 25/02/2016

### Fixed

* Missing dependency information which breaks Galaxy

## 0.1.0 - 23/02/2016

### Added

* Initial version
