# Styles

A collection of style guides and best practices used for my projects and teams.

[![Licence](https://img.shields.io/github/license/justintime50/styles)](LICENSE)

This document is fluid and many changes will be coming over the next few months as I collect more information to include here. By the time I'm finished, I'd love the final product to look something like: https://codeguide.co/

## General

* Don’t try to be smart, spell things out (eg: variable and function names)
    * Abbreviating or coming up with clever names only leads to more taxing code scanning by the next engineer
* Don’t pack lists or block pack them, always unpack lists so that each item is on its own line. It’s easier to read vertically than it is horizontally 
* Target versions of a language back to the the oldest maintained version up through the newest version where possible. Drop support for versions of a language that no longer receive maintenance (security updates, etc) and adopt new versions as early as is feasible
* Use object literals over complex if/else or switch/case statements
* Use implicit true statements when possible (eg: `if im_awesome is True:` — vs — `if im_awesome:`)
* Use constants or variables to define integers and other strings that aren’t easily identifiable (vs just passing them inline as parameters without declaring what they are)
* When making string comparisons, lowercase and strip the whitespace on the strings you are comparing
* Have a single return statement (don’t return early)
* Return statements should have a line break betwen them and the content above to clearly define the function is now complete and to separate focuses
* Use trailing commas where possible so that the list can easily grow in the future while keeping the next diff small
* Build out clean interfaces over ad-hoc scripts or SQL queries. This will ensure consistency, safety, and it can be code reviewed
* Ensure the code you are writing is `NULL` safe
* Ensure the code you are writing either has a unit test (especially when you need to guard against a regression) or that you have end-to-end tested it. Preferably, you would accomplish both
* Ensure the code is readable, remember, you will have other contributors looking at this in the future, maybe even years later * will it make sense then as it does now?
* Ensure your code is easily to maintain. Can things be added to this easily, can we tweak the configuration if requirements change in the future, does it make sense enough that someone with no context could jump in down the road and work on this?
* Ensure your code preserves git blame (eg: Trailing commas, breaking up lists to different lines)
* Ensure your code is self documenting (eg: function and variable names give a clear indicator to the reader of what is happening and what it is doing)
* Ensure your code does not introduce side effects. Functions and classes should do one thing without bleeding into other areas
* When creating software, assume you are creating it for someone else to use. Allow for configuration, use CLI flags/env vars where possible, document how to use your tool, use simple interfaces, and build it so that anyone can use it and not just you and your one use-case
    * Keep internal variables private, that way you can change their type of implementation on a whim. Make stable, consistent interfaces where users can then plug into. Every variable doesn’t need a getter and setter
* Abstract your data structures. Abstract interfaces allow its users to manipulate the essence of the data without having to know how it’s implemented
    * One example is a “vehicle fuel tank” interface where we expose the fuel tank capacity and number of gallons of gas variables. The better approach is to simply expose the percentage of fuel remaining (high level abstraction) so that if necessary we can change the implementation under the hood whenever we want 
* Create a consistent lexicon and share it across the company for things like names (eg: fetch, get, retrieve all ultimately mean the same thing, which will you use across your code base?)
* Extract try/catch block bodies into their own functions so that error processing can be separated from normal logic
* Comments should be used when we fail to express properly our intentions in code
* Comments should explain why we did what we did
* Comments can live for years, make sure they’ll age well hundreds of commits later
* Error handling should be in its own function if we are following the “functions do one thing” rule
* Put all instance variables at the top of your file, don’t mix them with public functions
* “Main” functions should be declared first in a file so that you can then keep reading from top to bottom as you drill down
* Files shouldn’t exceed 200 lines of code
* Line lengths should not exceed 120 chars
* Objects expose behavior and hide data. Data structures expose data and have no significant behavior
* If error handling obscures logic, it’s wrong
* Avoid returning null, avoid passing null as a parameter
* Unit tests should follow the Build-Operate-Check model where test data is built, then the function is operated, and finally the result is asserted against an expectation. Don’t add extra noise to tests
* Test only a single concept per unit test
* Clean tests follow five rules:
    * Fast: tests must be fast, otherwise you will stop running them and code will rot
    * Independent: tests should not need to run in a specific order. They should not setup conditions for another test
    * Repeatable: tests should be repeatable in any environment and produce the same output each time
    * Self-validating: tests should be pass/fail, you shouldn’t need to inspect logs or compare files to see if a test passed
    * Timely: tests should be written shortly before the production code they’ll be testing
* Classes
    * Class variables should be listed public static constants first, then private static variables, followed by private instance variables. There is seldom reason to have a public variable
    * The name of a class should describe what responsibilities it fulfills
    * You should be able to define a class in 25 words or less without using the words “if”, “and”, “or”, or “but”, otherwise it probably has too many responsibilities
    * SPR: Single Responsibility Principle, classes should only ever do one thing. A “dashboard” class shouldn’t grab the version number and show stats for your app * break out the version data into a separate class
    * Programs should be made up of many small classes
    * OCP: Open-Closed Principle, classes are open to extension but closed to modification
    * DIP: Dependency Inversion Principle, classes should depend on abstractions, not concrete details
* Separate constructing a system from using it, meaning that the “setup” code should be separate from the main business logic
* Simple Design:
    * Runs all the tests
    * Contains no duplication
    * Expresses the intent of the programmer
    * Minimizes the number of classes and methods
* Concurrency
    * Keep your concurrency related code separate from other code
    * Limit the access of any data that may be shared across threads
    * Make copies of the data used in threads, try not to edit data in threads as it can lead to deadlock and sync issues
    * Keep threads as independent as possible
    * Avoid using more than one method on a shared object
    * Keep your synchronized sections as small as possible
    * Shutting down a system gracefully is hard. Start planning this piece of the concurrent puzzle early
    * Don’t ignore one-off failures in concurrency, typically it is an indicator your system isn’t built well
    * Make thread based code “pluggable”, where the logic and the threading happen separately and can be easily configured
* Open source projects should be placed in a top-level `src` folder so that project config and documents can live outside the project folder

## Language Specific

Each language below will have a suite of tools listed that are the standard technologies to use for that specific language. Listed below those will be language-specific styles and best practices.

### Bash

* **Linter:** [ShellCheck](https://github.com/koalaman/shellcheck)

### C#

* **VCR:** [Scotch](https://github.com/mleech/scotch)

### CSS

* **Linter:** [stylelint](https://github.com/stylelint/stylelint)

The following is a checklist of items that every website should have:
* Site must use a `fallback font` for every font used

### Docker

* **Linter:** [Hadolint](https://github.com/hadolint/hadolint)

### HTML

* **Linter:** [HTMLHint](https://github.com/htmlhint/HTMLHint)

The following is a checklist of items that every website should have:
* Site must have a Favicon
* Site must show a well-formed `404 Page` when users visit a page that does not exist
* Site must appropriately handle other `4xx` and `5xx` pages
* Site must have `Google Analytics` or another method of analytics
* Site must use modularized pages (eg: nav/footer are their own components and can be reused)
* Site must have a `meta author tag`
* Site must have a `meta keyword tag` containing no duplicates
* Site must have a `meta description tag`
* Site must define a `language` in the `html` tag
* Site must define a `meta character set` (eg: UTF-8)
* Site must define a `robots.txt` file that whitelists all public pages (can use wildcard), and explicitly blacklist pages that should not be accessible
* Site must define `alt & title tags` on every image
* Site must have a publicly available `Sitemap`

### Java

* **Linter:** [Checkstyle](https://github.com/checkstyle/checkstyle)

### Javascript

* **Linter:** [ESLint](https://github.com/eslint/eslint)
* **Tests:** [Mocha](https://github.com/mochajs/mocha)
* **VCR:** [Polly.JS](https://github.com/Netflix/pollyjs)

### PHP

* **Linter:** [PHP_CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer)
* **Tests:** [PHPUnit](https://github.com/sebastianbergmann/phpunit)
* **VCR:** [PHP VCR](https://github.com/php-vcr/php-vcr)

### Plist Files

```bash
# The following will tell you if you have syntax errors or not in a plist file.
plutil local.myfile.plist
```

### Python

* **Formatter:** [Black](https://github.com/psf/black)
* **Import Sorter:** [iSort](https://github.com/PyCQA/isort)
* **Linter:** [Flake8](https://github.com/PyCQA/flake8)
* **Tests:** [Pytest](https://github.com/pytest-dev/pytest)
* **VCR:** [VCR.py](https://github.com/kevin1024/vcrpy)

* Use Black for formatting (passing the `--skip-string-normalization` and `--line-length 120` options)
    * Use other methods to ensure `Black` plays nicely with `Flake8` and `iSort`
* You can’t kill threads easily in Python, keep this in mine when playing with concurrency

### Ruby

* **Linter:** [RuboCop](https://github.com/rubocop-hq/rubocop)
* **Tests:** [RSpec](https://github.com/rspec/rspec)

### Websites & Infra

* Site must have a `www.` alias configured
* Site must have `SSL` configured (eg: LetsEncrypt)
* Site must pass an `SSL cert test` with an `A` or better rating
* Site must pass a `speed test` check
* Site must pass a `compatibility test` check
* Site must have minified HTML, CSS, and JS where possible
