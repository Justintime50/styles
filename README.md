# Styles

A collection of style guides and best practices used for my projects and teams.

[![Build](https://github.com/Justintime50/styles/workflows/build/badge.svg)](https://github.com/Justintime50/styles/actions)
[![Licence](https://img.shields.io/github/license/justintime50/styles)](LICENSE)

This document is fluid and many changes will be coming over the next few months as I collect more information to include here. By the time I'm finished, I'd love the final product to look something like: <https://codeguide.co/>

## Tooling Configuration

You can find various configuration files for styling/formatting tools across various langauges found in the `styles` directory.

### NPM Install

```bash
npm install --save-dev justintime50-styles
```

### Composer Install

```bash
composer require --dev justintime50/styles
```

## General

### Architecture

- Simple Design:
  - Runs all the tests
  - Contains no duplication
  - Expresses the intent of the programmer
  - Minimizes the number of classes and methods
- Build out clean interfaces over ad-hoc scripts or SQL queries. This will ensure consistency, safety, and it can be code reviewed
- Separate constructing a system from using it, meaning that the “setup” code should be separate from the main business logic
- Abstract your data structures. Abstract interfaces allow its users to manipulate the essence of the data without having to know how it’s implemented
  - One example is a “vehicle fuel tank” interface where we expose the fuel tank capacity and number of gallons of gas variables. The better approach is to simply expose the percentage of fuel remaining (high level abstraction) so that if necessary we can change the implementation under the hood whenever we want
- Objects expose behavior and hide data. Data structures expose data and have no significant behavior
- When creating software, assume you are creating it for someone else to use. Allow for configuration, use CLI flags/env vars where possible, document how to use your tool, use simple interfaces, and build it so that anyone can use it and not just you and your one use-case
  - Keep internal variables private, that way you can change their type of implementation on a whim. Make stable, consistent interfaces where users can then plug into. Every variable doesn’t need a getter and setter
- Use the Model, View, Controller framework (MVC), especially for frontend websites
- Use the Singleton design pattern when possible
- Routes -> Views/Actions -> Service -> Repo -> DB
- Use frontend and backend validation, this is important because when you have both, we can skip sending an API call for instance when we know there is bad data. We will then only send the payload across once the data is valid. Backend validation is important as the source of truth and because things like developer tools in a browser can defeat frontend validations

### Checklist

- Ensure the code you are writing is `NULL` safe
- Ensure the code you are writing either has a unit test (especially when you need to guard against a regression) or that you have end-to-end tested it. Preferably, you would accomplish both
- Ensure the code is readable, remember, you will have other contributors looking at this in the future, maybe even years later
  - Will it make sense then as it does now?
- Ensure your code is easily to maintain. Can things be added to this easily, can we tweak the configuration if requirements change in the future, does it make sense enough that someone with no context could jump in down the road and work on this?
- Ensure your code preserves git blame (eg: Trailing commas, breaking up lists to different lines)
- Ensure your code is self documenting (eg: function and variable names give a clear indicator to the reader of what is happening and what it is doing)
- Ensure your code does not introduce side effects. Functions and classes should do one thing without bleeding into other areas

### Classes

- Class variables should be listed public static constants first, then private static variables, followed by private instance variables. There is seldom reason to have a public variable
- The name of a class should describe what responsibilities it fulfills
- You should be able to define a class in 25 words or less without using the words “if”, “and”, “or”, or “but”, otherwise it probably has too many responsibilities
- SPR: Single Responsibility Principle, classes should only ever do one thing. A “dashboard” class shouldn’t grab the version number and show stats for your app
  - Break out the version data into a separate class
- Programs should be made up of many small classes
- OCP: Open-Closed Principle, classes are open to extension but closed to modification
- DIP: Dependency Inversion Principle, classes should depend on abstractions, not concrete details
- Using instance variables is a powerful way to cut down on passing large amounts of parameters around your app when everything is stored within the `self` namespace, it can be easily referenced from any instance method and seemingly numberless variables can be associated with `self` (eg: Python)

### Comments

- Comments should be used when we fail to express properly our intentions in code
- Comments should explain why we did what we did
- Comments can live for years, make sure they’ll age well hundreds of commits later

### Concurrency

- Keep your concurrency related code separate from other code
- Limit the access of any data that may be shared across threads
- Make copies of the data used in threads, try not to edit data in threads as it can lead to deadlock and sync issues
- Keep threads as independent as possible
- Avoid using more than one method on a shared object
- Keep your synchronized sections as small as possible
- Shutting down a system gracefully is hard. Start planning this piece of the concurrent puzzle early
- Don’t ignore one-off failures in concurrency, typically it is an indicator your system isn’t built well
- Make thread based code “pluggable”, where the logic and the threading happen separately and can be easily configured

### Databases

- All inserts/updates must be run inside a transaction. Validate your change before committing it.

### Error Handling

- Extract try/catch block bodies into their own functions so that error processing can be separated from normal logic
- Error handling should be in its own function if we are following the “functions do one thing” rule
- If error handling obscures logic, it’s wrong

### Functions

- Have a single return statement (don’t return early)
- Return statements should have a line break betwen them and the content above to clearly define the function is now complete and to separate focuses
- Avoid returning null, avoid passing null as a parameter
- Avoid using class methods where possible and instead use instance or static methods.
- Don’t use “selector arguments” or booleans for arguments to a function because this breaks the single responsibility principle. If your function does something different depending on a Boolean parameter, it really does two things and therefore the name will be misleading. Instead, you should break up the function into smaller functions, each doing one branch of the boolean flag

#### Proper Function Structure

Functions should follow the 1-2-3 principle:

1. The first grouping of code (split by an empty newline) is usually boilerplate code or setup where we assign variables that we'll need later on
2. The second grouping of code (split by an empty newline) is the logic where we actually do what the function is called
3. The third grouping (split by an empty newline) should be the final return statement. Functions should also only return once at the end for easy maintenance and readability (TODO: provide research into this)

There are obviously larger and smaller functions than this, but don't be afraid of newlines in functions. I've seen countless 30 line functions that had no line breaks which can be very hard on the eyes when scanning or reasoning about code. Breaking up related blocks of code makes maintaining and reading that code much easier.

```python
def is_number_large(my_number, threshold = 100):
    """Returns true if a number is larger than a custom threshold."""
    number_as_int = int(my_number)
    threshold_as_int = int(threshold)

    if number_as_int > threshold_as_int:
        large_number = True
    else:
        large_number = False

    return large_number
```

### Meta

- Put all instance variables at the top of your file, don’t mix them with public functions
- “Main” functions should be declared first in a file so that you can then keep reading from top to bottom as you drill down
- Files shouldn’t exceed 200 lines of code
- Line lengths should not exceed 120 chars
- Use trailing commas where possible so that the list can easily grow in the future while keeping the next diff small
- Create a consistent lexicon and share it across the company for things like names (eg: fetch, get, retrieve all ultimately mean the same thing, which will you use across your code base?). If you do something someway, do it that way everywhere. This leans back to the principle of least surprise
- Don’t pack lists or block pack them, always unpack lists so that each item is on its own line. It’s easier to read vertically than it is horizontally
- Use object literals over complex if/else or switch/case statements
- Use implicit true statements when possible (eg: `if im_awesome is True:` — vs — `if im_awesome:`)
- When making string comparisons, lowercase/uppercase and strip the whitespace on the strings you are comparing
- Always sort your lists unless there is an explicit reason not to. This ensures that diffs stay small and sorted lists are much easier to maintain and find info in
- Positives are easier to understand than negatives, in the case of if/else statements, have the “if” section be the positive logic
- Don’t include dead code. This could be code in an if/else block that will never be reached or code in a try/except block that can never throw
- Think of “vertical separation”, variables and functions should be defined close to where they are used, not hundreds of lines apart
- General purpose static functions should not be contained in a class since they typically aren’t actually coupled to the class
- Shoot for brevity, be precise! Don’t use a float for currency (break it down into an integer for "cents"), don’t avoid adding lock/transaction management on concurrency, etc - “Don’t be lazy about the precision of your decision”
- Avoid words like `filter` when naming thnigs because it can either mean `filter out` or `filter in`

### Naming

- Don’t try to be smart, spell things out (eg: variable and function names)
  - Abbreviating or coming up with clever names only leads to more taxing code scanning by the next engineer
- Use constants or variables to define integers and other strings that aren’t easily identifiable (vs just passing them inline as parameters without declaring what they are). A great example of a snippet of code where this could be useful is instead of directly returning the following, you could assign it a descriptive variable and return that clean variable instead: `{k: cls._objects_to_ids(v) for k, v in six.iteritems(params)}`. This also goes for having multiple and/or statements, assign them to a variable to help describe intent

### Open Source

- Projects should be placed in a top-level `src` folder so that project config and documents can live outside the project folder
- Target versions of a language back to the the oldest maintained version up through the newest version where possible. Drop support for versions of a language that no longer receive maintenance (security updates, etc) and adopt new versions as early as is feasible

### Testing

- Clean tests follow five rules:
  - Fast: tests must be fast, otherwise you will stop running them and code will rot
  - Independent: tests should not need to run in a specific order. They should not setup conditions for another test
  - Repeatable: tests should be repeatable in any environment and produce the same output each time
  - Self-validating: tests should be pass/fail, you shouldn’t need to inspect logs or compare files to see if a test passed
  - Timely: tests should be written shortly before the production code they’ll be testing
- Unit tests should follow the Build-Operate-Check model where test data is built, then the function is operated, and finally the result is asserted against an expectation. Don’t add extra noise to tests
- Test only a single concept per unit test
- Don’t mock to make yourself feel better; mock because you have to

## Language Specific

### CSharp

#### Tools

- **Linter:** [Dotnet format](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-format)
- **VCR:** [EasyVCR](https://github.com/EasyPost/easyvcr-csharp)

### CSS

#### CSS Tools

- **Linter:** [stylelint](https://github.com/stylelint/stylelint)

#### CSS Styles

The following is a checklist of items that every website should have:

- Site must use a `fallback font` for every font used

### Docker

#### Docker Tools

- **Linter:** [Hadolint](https://github.com/hadolint/hadolint)

### Golang

#### Golang Tools

- **Formatter:** [Gofmt](https://pkg.go.dev/cmd/gofmt)
- **Linter:** [Golangci-lint](https://golangci-lint.run)
- **Security:** [Gosec](https://github.com/securego/gosec)
- **VCR:** [go-vcr](https://github.com/dnaeon/go-vcr)

### HTML

#### HTML Tools

- **Linter:** [HTMLHint](https://github.com/htmlhint/HTMLHint)

#### HTML Styles

- Don't repeat `&nbsp;` over and over when you want to space something, use a CSS class in a `<span>` to do this instead
- Avoid using inline CSS styles (use CSS classes and external stylesheets)
- `<br />` tags should only be contained inside a set of `<p>` tags. CSS classes should be used otherwise to provide vertical spacing
- Always list the `href/src` first for stylesheets and scripts for easy readability and visual scanning (eg: `<a href="https://example.com" target="_blank">example</a>`)
- `rel` is required for stylesheets

The following is a checklist of items that every website should have:

- Site must have a Favicon
- Site must show a well-formed `404 Page` when users visit a page that does not exist
- Site must appropriately handle other `4xx` and `5xx` pages
- Site must have `Google Analytics` or another method of analytics
- Site must use modularized pages (eg: nav/footer are their own components and can be reused)
- Site must have a `meta author tag`
- Site must have a `meta keyword tag` containing no duplicates
- Site must have a `meta description tag`
- Site must define a `language` in the `html` tag
- Site must define a `meta character set` (eg: UTF-8)
- Site must define a `robots.txt` file that whitelists all public pages (can use wildcard), and explicitly blacklist pages that should not be accessible
- Site must define `alt & title tags` on every image
- Site must have a publicly available `Sitemap`

### Java

#### Java Tools

- **Linter:** [Checkstyle](https://github.com/checkstyle/checkstyle)
- **Static Analysis:** [Error Prone](https://github.com/google/error-prone)
- **VCR:** [EasyVCR](https://github.com/EasyPost/easyvcr-java)

### Javascript

#### Javascript Tools

- **Formatter:** [Prettier](https://prettier.io)
  - Config file found in this repo: `.prettierrc.yml`
- **Linter:** [ESLint](https://github.com/eslint/eslint)
  - Config file found in this repo: `.eslintrc.yml`
- **Tests:** [Mocha](https://github.com/mochajs/mocha)
- **VCR:** [Polly.js](https://github.com/Netflix/pollyjs)

### PHP

#### PHP Tools

- **Linter:** [PHP_CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer)
- **Tests:** [PHPUnit](https://github.com/sebastianbergmann/phpunit)
- **VCR:** [PHP VCR](https://github.com/php-vcr/php-vcr)

### Python

#### Python Tools

- **Formatter:** [Black](https://github.com/psf/black)
  - Config file found in this repo: `pyproject.toml`
- **Import Sorter:** [iSort](https://github.com/PyCQA/isort)
- **Linter:** [Flake8](https://github.com/PyCQA/flake8)
  - Config file found in this repo: `.flake8`
- **Security:** [Bandit](https://github.com/PyCQA/bandit)
- **Static Analysis:** [mypy](https://github.com/python/mypy)
- **Tests:** [Pytest](https://github.com/pytest-dev/pytest)
- **VCR:** [VCR.py](https://github.com/kevin1024/vcrpy)

#### Python Styles

- You can’t kill threads easily in Python, keep this in mind when playing with concurrency
- Do not define raw paths, you must use the `os.path.join()` function as this will automatically build the paths for you depending on what OS you're on (eg: slashes on Windows)
- Use `datetime.timedelta` to offset a date which will automatically roll over months and years as needed. Do not add `+6` to a date or year as you will run into an error such as a month not being able to contain 35 days
- "Mock an item where it is used, not where it came from."

It's generally an anti-pattern to do something like the following. If a file got deleted between the check and it being removed, it will error:

```python
# Anti-pattern
if os.path.isfile(file_path):
    os.remove(file_path)

# Use instead
try:
    os.remove(file_path)
except FileNotFoundError:
    pass
```

Ensure that error assertions are unindented from a pytest context helper (bites me all the time):

```python
# This will fail
with pytest.raises(Error) as error:
    my_function('BAD_INPUT')

    assert str(error.value) == 'You sent bad input'

# This will succeed
with pytest.raises(Error) as error:
    my_function('BAD_INPUT')

assert str(error.value) == 'You sent bad input'
```

### Ruby

#### Ruby Tools

- **Linter:** [RuboCop](https://github.com/rubocop-hq/rubocop)
- **Security:** [Brakeman](https://github.com/presidentbeef/brakeman)
- **Tests:** [RSpec](https://github.com/rspec/rspec)

### Shell (Bash)

#### Shell Tools

- **Linter:** [ShellCheck](https://github.com/koalaman/shellcheck)

#### Shell Styles

- Build shell scripts and tools for the largest compatible surface area possible, ensure they are POSIX compliant and able to not only run on the latest versions of Bash but also the oldest versions of `sh`, `dash`, or `ksh`

### Swift

#### Swift Tools

- **Linter:** [Swiftlint](https://github.com/realm/SwiftLint)

### Websites & Infrastructure

#### Websites & Infrastructure Styles

- Site must have a `www.` alias configured
- Site must have `SSL` configured (eg: LetsEncrypt)
- Site must pass an `SSL cert test` with an `A` or better rating
- Site must pass a `speed test` check
- Site must pass a `compatibility test` check
- Site must have minified HTML, CSS, and JS where possible
- Databases should be configured to have at least a single main read/write node and multiple read-only replicas. This helps with scaling and allows for failover when necessary. Applications should then only read from the replicas and write to the main node. Reporting and analytics can then be pulled from a dedicated replica so production data is not affected.
- Services should be load balanced to at least 2 instances for each "container", providing high availability when one or more nodes fail
- Services should have a healthcheck, be killed upon failure, and automatically restarted when those healthchecks fail for automated recovery
