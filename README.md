# Testing & Commits

## Table of Contents

- [Testing & Commits](#testing--commits)
  - [Table of Contents](#table-of-contents)
  - [Unit Testing](#unit-testing)
  - [Commits](#commits)
    - [Pre-commit hooks](#pre-commit-hooks)
    - [Commitlint](#commitlint)
    - [Test commitlint configs](#test-commitlint-configs)
  - [Steps to set-up](#steps-to-set-up)
  - [References](#references)
  - [Glossary](#glossary)
    - [lint](#lint)
    - [hook](#hook)

## Unit Testing

- Python

  - [`unittest`](https://docs.python.org/3/library/unittest.html)
    - built-in with Python
  - [`pytest`](https://docs.pytest.org/en/7.1.x/)?
    - Flask's official testing [documentation](https://flask.palletsprojects.com/en/1.1.x/testing/) uses `pytest`

- JavaScript
  - [`jest`](https://jestjs.io/)

## Commits

### Pre-commit hooks

Pre-commit hooks will allow us to run something before a commit. In our case, we will be running pre-commit hooks to format code, lint code, and run unit tests. If any of the unit tests fail, the commit will not be able to finish. If your test(s) fail, fix them until they pass, then commit your changes.

- Python

  - [`pre-commit`](https://pre-commit.com/)
    - changes must be staged before pre-commit hooks can run
    - configurable to run post-commits also

- JavaScript
  - [`husky`](https://typicode.github.io/husky/#/)

Commits will be linted. If your commit message does not follow [conventional commits](https://www.conventionalcommits.org/), then you will receive an error. [Here](https://www.conventionalcommits.org/en/v1.0.0/#specification) is conventional commits' specifications and guidelines for commit messages. [Here](https://github.com/conventional-changelog/commitlint) is their github page.

### Commitlint

- [Configuration](https://commitlint.js.org/#/reference-configuration)
- [Rules](https://commitlint.js.org/#/reference-rules)

Ex:

```javascript
module.exports = {
  extends: ["@commitlint/config-conventional"],
  rules: {
    "type-enum": [
      2, // level: 0 - disable, 1 - warning, 2 - error
      "always", // applicable: always | never
      [
        "ci",
        "chore",
        "docs",
        "feat",
        "fix",
        "perf",
        "refactor",
        "revert",
        "style",
        "test",
      ],
    ],
  },
}
```

### Test commitlint configs

```bash
echo "foo: test" | npx commitlint -V
```

## Steps to set-up

- Python

  1. Set-up your virtual environment if you have not already. You can use the `requirements.txt` found in the `python` directory here.
  2. Install `pre-commit`. Instructions [here](https://pre-commit.com/#installation). Follow the quick start until you finish the "Quick start" section.
  3. Install `commitlint`. Instructions [here](https://github.com/conventional-changelog/commitlint#getting-started). Configure `commitlint` via `.commitlintrc.json`.

  ```bash
  # Install commitlint cli and conventional config
  npm install --save-dev @commitlint/{config-conventional,cli}
  # For Windows:
  npm install --save-dev @commitlint/config-conventional @commitlint/cli

  # Configure commitlint to use conventional config
  echo "module.exports = {extends: ['@commitlint/config-conventional']}" > commitlint.config.js
  ```

  5. Write your code and unit tests. Refer to `/python/hello_world.py` and `/python/test_hello_world.py` files. Basic explanation video for `unittest` in [References](#references).
  6. Configure your `pre-commit-config.yaml` to include `commitlint` and unit testing. Refer to the `pre-commit-config.yaml` in this repo. [Documentation](https://github.com/alessandrojcm/commitlint-pre-commit-hook#configuration) to set-up `commitlint` in `pre-commit`. _Optional: add formatter (autopep8, black, etc) or linting (pylint, pylance, etc) to your `pre-commit-config.yaml` as well._ **This will only work if the directory has been initialized as a Git repo.**
  7. Stage your files and commit using conventional commit messages.

- JavaScript

  1. Install the correct packages

  ```bash
  npm install --save-dev jest husky @commitlint/cli @commitlint/config-conventional
  ```

  2. Set-up your scripts in your `package.json`. Make sure you have a `prepare` script for installing `husky` and `test` script for unit testing via `jest`.

  ```json
   "scripts": {
    "prepare": "husky install",
    "test": "jest"
  }
  ```

  3. Activate husky hooks. **This will only work if the directory has been initialized as a Git repo.**

  ```bash
  npm run prepare
  ```

  4. Add test hook and commitlint to husky. Configure your `/javascript/.commitlintrc.json` to match your Python's.

  ```bash
  # Add test hook
  npx husky add .husky/pre-commit "npm test"

  # Add commitlint
  npx husky add .husky/commit-msg 'npx --no-install commitlint --edit '
  ```

  5. Write your code and unit tests. Refer to `/javascript/hello-world.js` and `/javascript/__tests__/hello-world.test.js`.
  6. Stage your files and commit using using conventional commit messages.

## References

- Unit testing [video](https://www.youtube.com/watch?v=6tNS--WetLI) for Python using `unittest`

## Glossary

### lint

[Lint](<https://en.wikipedia.org/wiki/Lint_(software)>), or a linter, is a static code analysis tool used to flag programming errors, bugs, stylistic errors, and suspicious constructs.

### hook

Hooks allow us to do something and/or run code (or script) before something else.
