## "Analyze" Action for GitHub Actions
Runs an analysis on a project using the Ion Channel API.

## Usage
If your repo already contains an `.ionize.yml` file, using this action is
as simple as adding the following step to your workflow:

```
 - name: Configure AWS Credentials
      uses: ion-channel/analyze@v1
      with:
        branch: ${{ github.ref_name }}
        secret-key: ${{ secrets.IONCHANNEL_SECRET_KEY }}
```

If your `.ionize.yml` file has a different name, or isn't in the root directory of your repo,
you can specify the path to it by adding `config: path/to/my_ionize.yml` to the step's `with:` block.

See the [action.yml](action.yml) file for a list of all the action's inputs.

An `.ionize.yml` file should look like this:

```yml
team: df47e4d1-1926-4990-8ef8-f326be59d6fd
project: 8b5f1c3b-ac29-4dfb-b735-a7bf51120594
```

If you are using this action in a workflow run triggered by a `pull_request` event rather than a `push` event,
you will likely want to use `github.head_ref` to get the source branch name, rather than `github.ref_name`.

Here is a full example workflow that runs on pull requests targeting the `develop` and `main` branches:

```yml
name: Analyze
on:
  pull_request:
    branches:
      - develop
      - main

jobs:
  analysis:
    name: Analysis
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2

      - name: Analyze
        uses: ion-channel/analyze@v1
        with:
          branch: ${{ github.head_ref }}
          secret-key: ${{ secrets.IONCHANNEL_SECRET_KEY_PROD }}
```
