# Changed Files for GitHub Actions

This GitHub Action returns a list of files changed by a pull request or a push, and a list of files matching a given pattern list.

## Usage

``` yaml
on:
  push:

  pull_request:

jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - id: changed-files
        name: Check changed files
        uses: knu/changed-files@v1
        with:
          paths: |-
            **/*.{json,js,ts,[cm]js}
            !**/*.d.ts
      - name: Show changed files
        run: |
          echo "Changed files:"
          echo "${{ steps.changed-files.outputs.changed_files }}" | sed 's/^/  /'
          echo "Matched files:"
          echo "${{ steps.changed-files.outputs.matched_files }}" | sed 's/^/  /'
      - name: Run if any file matching paths is changed
        if: steps.changed-files.outputs.matched_files != ''
        run: |
          echo "Do something"
      - name: Run if a specific file is changed
        if: contains(fromJson(steps.changed-files.outputs.changed_files_json), 'package.json')
        run: |
          echo "Do something"
```

## Outputs

- `changed_files`

  A multi-line string containing the list of changed files.

- `changed_files_json`

  A JSON string containing the list of changed files.

- `matched_files`

  A multi-line string containing the list of changed files matching `paths` patterns.  Empty (`""`) if `paths` is not given.

- `matched_files_json`

  A JSON string containing the list of changed files matching `paths` patterns.  Empty (`"[]"`) if `paths` is not given.

## Author

Copyright (c) 2024 Akinori MUSHA.

Licensed under the 2-clause BSD license.  See `LICENSE.txt` for details.

Visit the [GitHub Repository](https://github.com/knu/update-tools) for the latest information.
