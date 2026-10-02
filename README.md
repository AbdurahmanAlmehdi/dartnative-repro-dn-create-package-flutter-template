# Repro: `dn create -t package` generates Flutter's package template

Issue: https://github.com/DartNative/dartnative/issues/74

This repo is the unmodified output of

```sh
dn create -t package --project-name dn_create_package_repro .
```

(the generated README is kept as `GENERATED_README.md`; this file replaces it).

The project describes itself as "A new Flutter package project.", depends on
`flutter: sdk: flutter`, `flutter_test` and `flutter_lints`, links to the
Flutter docs, and doesn't pass its own checks: `dn analyze` reports 5 errors
and `dn test` refuses to run.

## Run

```sh
dn analyze
dn test
```

## What you'll see

```console
$ dn analyze
   info • The imported package 'flutter_test' isn't a dependency of the importing package • test/dn_create_package_repro_test.dart:1:8 • depend_on_referenced_packages
  error • Target of URI doesn't exist: 'package:flutter_test/flutter_test.dart' • test/dn_create_package_repro_test.dart:1:8 • uri_does_not_exist
  error • The function 'test' isn't defined • test/dn_create_package_repro_test.dart:6:3 • undefined_function
  error • The function 'expect' isn't defined • test/dn_create_package_repro_test.dart:8:5 • undefined_function
  error • The function 'expect' isn't defined • test/dn_create_package_repro_test.dart:9:5 • undefined_function
  error • The function 'expect' isn't defined • test/dn_create_package_repro_test.dart:10:5 • undefined_function

$ dn test
Downloading packages...
Got dependencies!
Error: cannot run without a dependency on either "package:flutter_test" or "package:test". Ensure the following lines are present in your pubspec.yaml:

dev_dependencies:
  flutter_test:
    sdk: flutter
```

The generated `pubspec.yaml`:

```yaml
name: dn_create_package_repro
description: "A new Flutter package project."
version: 0.0.1
homepage:
environment:
  sdk: ^3.12.0-192.0.dev
  flutter: ">=1.17.0"
dependencies:
  flutter:
    sdk: flutter
dev_dependencies:
  flutter_lints: ^6.0.0
flutter:
```

## Expected

A DartNative package: `dartnative` as the dependency, `test` (or the
DartNative test package) for tests, DartNative docs links, and a project that
passes `dn analyze` and `dn test` as created — as `dn create` (app) and
`-t plugin` already do.

## Environment

- DartNative 1.0.0 (SDK `113c27aacb2`, framework edition `7ae29132`), Dart 3.12.0
- macOS 26.7.1
