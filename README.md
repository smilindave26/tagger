# tagger
GitHub Action for updating version tag.  It checks for the existance of a `version` file in the root directory of the repositoy.  This file contains a semver string (`<major> "." <minor> "." <patch>`)

If the version from the `version` file is greater than the current version tag (or no tag is found), a new tag is created with the contents of the `version` file.

Otherwise, a new tag is created by bumping the `<patch>` number from the previos tag.

An output variable called `version` is created by the action.

## Usage
Create a `yml` file in your repository's `.github/workflow`, something like this:
```yml
name: tagger-example
on: push

jobs:
  test:
    runs-on: macos-latest
    if: github.ref == 'refs/heads/master'
    steps:
    - name: Checkout Project
      uses: actions/checkout@v2
      with:
        fetch-depth: 0

    - name: Update Tag
      id: tagger
      uses: smilindave26/tagger@v1
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      
    - name: Print Tag
      run: echo ${{ steps.tagger.outputs.version }}
```