# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [v2.0.0] Gradle to Maven migration (2026-04-08)

Forked from [jeffnelson/jackson-merge-patch](https://github.com/jeffnelson/jackson-merge-patch) for continued maintenance under organization ownership.

### Changed
- Migrated build system from Gradle 4.8.1 to Maven 3.9.10
- Restructured project: root `/src` extracted to `jackson-merge-patch-core` module
- Project is now a 3-module Maven build: parent, core, and validations
- Upgraded Lombok from 1.16.22 to 1.18.38 for improved compatibility
- Centralized dependency and plugin version management in parent POM

### Added
- Maven wrapper (3.9.10, only-script distribution)
- Parent POM with centralized dependency management and plugin management
- `jackson-merge-patch-core` module (previously root source)
- JaCoCo code coverage via `jacoco-maven-plugin`
- Source JAR generation via `maven-source-plugin`

## [v1.0.3-SNAPSHOT] Original Gradle build (2019-12-04)

_Original project by [Jeff Nelson](https://github.com/jeffnelson/jackson-merge-patch). Apache License 2.0._
