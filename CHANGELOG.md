# CHANGELOG

## v0.8.0 (2025-05-12)

- Warn on unused uses for PHP

## v0.7.0 (2025-01-07)

- Migrates ESLint config from v8 to v9 syntax
- Adds Node and PHP minimum version constraints

## v0.6.0 (2024-05-31)

- Ignores `at-rule-no-unknown` in CSS linting.

## v0.5.0 (2023-08-07)

- Disables the `no-descending-specificity` rule of stylelint as I'm almost always using nested SASS which doesn't play nicely with this rule

## v0.4.0 (2023-04-27)

- Enforces string convention for `import-notation` in `Stylelint`

## v0.3.0 (2023-04-05)

- Enforces a line-length of 120 characters for PHP projects

## v0.2.0 (2022-11-24)

- Removes the `env` target for ESLint because this will change on a per-project basis and should be configured via the CLI

## v0.1.0 (2022-11-24)

- Initial release. Includes basic styles for CSS, Javascript, PHP, Python, Ruby, and Swift
- Released via NPM and Composer
