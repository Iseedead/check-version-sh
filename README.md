# check-version-sh

[![Marketplace](https://img.shields.io/badge/version-1.2.1-blue)](https://github.com/marketplace/actions/check-version-sh)

Action functionality:

- Ensure version changes regardless base branch on pull request
- Ensure version changes regardless previous commit on push

## Limitations

- Currently supported files
    - `pom.xml` - check for the first `<version>\d.\d.\d</version>`
    - `package.json` - check for the first `"version": "\d.\d.\d""` version
    - `package-lock.json` - check for the first `"version": "\d.\d.\d""` version
    - `README.md`
        - `readme-badge` - check for the `https://*.+/badge/version-\d.\d.\d` version
        - `readme-changelog` - check for the `- **\d.\d.\d**` version
        - `readme-action` - check for the `<current-repo-name>@v\d.\d.\d` version. Suppose to be useful for actions only
- Currently supported numeric versions only
- MacOS implementation works through installing `ggrep` through `homebrew`, hence it works slower than on Ubuntu

## Changelog

- **1.1.0**
    - Removed `README.md` standard option
    - Add labels to the version-containing files
    - Add `readme-badge` check
    - Add `readme-changelog` check
- **1.1.1** - Bump for no reason
- **1.2.0**
    - Add `package-lock.json` file support
    - Add `readme-action` label support
    - Fix digit regexp for the `readme-badge` label
- **1.2.1**
    - Fix regexp for the old version in the `readme-changelog` version
    - Fix `change-only-for` array-split
    - Change Git current branch retrieval to support Git version < 2.22 
    - Fix label for the `readme-action` label

## Usage

Make sure to check out first since the script needs some files to check.   
check-version-sh action will exit with success code `0` if check

- for all specified (with optional `check-only-for` option) files were successful
- for at least one present (from all supported version files) file was successful

And with fail code `1` if check

- for any specified files (with optional `check-only-for` option) fails
- for at least one present (from all supported version files) file fails
- if none of the files (from all supported version files) is present in the `git diff`

### Parameters

See [action.yml](action.yml) or [info.sh](src/check-version/info.sh).

- `log-level` - logging level that will be used by the shell script
    - TRACE
    - DEBUG
    - INFO
    - WARN
    - ERROR
- `check-only-for` - a list of coma separated labels to check for (since GitHub Actions does not support array inputs
  yet). Do not insert space after coma, or the list will be parsed as a separate arguments.

### Sample Configuration

```yaml
jobs:
  # ...
  steps:
    # ...
    - name: Check Version Changes
      uses: sivkovych/check-version-sh@v1.2.1
      with:
        log-level: "INFO"
        check-only-for: "pom.xml,package.json,readme-badge"
    # ...
```
