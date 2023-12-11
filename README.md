# action-update-api

This repository contains a [GitHub action](https://docs.github.com/en/actions)
that takes an [OpenAPI](https://www.openapis.org) spec file and uploads it
to [alphadoc](https://alphadoc.io).

## Setup

### Alphadoc Organization

Follow [these steps](https://docs.github.com/en/actions/learn-github-actions/variables#creating-configuration-variables-for-a-repository) to set the Alphadoc Organization as an environment variable for your repo. Name the variable "ALPHADOC_ORGANIZATION" and assign the Organization value to it. If my login link is https://demo.alphadoc.io, then my organization name is `demo`.

### Alphadoc Project Version ID

Visit to the editor https://{organization}.alphadoc.io/editor, click on your Project and retrieve the Project Version ID from the URL. The first value after `editor` is the Project Version ID. In our example the Project Version ID is `a6bf4715-111d-4d59-bcd4-de90aa725b93`.

```
https://demo.alphadoc.io/editor/a6bf4715-111d-4d59-bcd4-de90aa725b93/pages/26e3aba7-77e0-4f70-9554-8e827fe90b44
```

Follow the same steps as for the Organization to configure the Project Version ID as a Github Action variable.

### Alphadoc API Spec ID

Visit to the editor (https://{yourcompanyname}.alphadoc.io/editor), click on your Project and go to the API upload section. Click the three-dots menu and copy the API Specification ID.

Follow the same steps as for the Project Version ID to configure the API Specification ID as a Github Action variable.

### Add API Key

As the final step of the process, generate an Alpadoc API Key by visiting
`<ORGANIZATION>.alphadoc.io/editor/settings/user/api-keys`
and clicking `Generate API Key`.

Copy the generated API key to you clipboard and go to
[secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets#about-encrypted-secrets)
in Github. Save the secret as "ALPHADOC_API_KEY".

### Location of OpenAPI file

The final step is to add the location of your OpenAPI file in your repo to the workflow .yaml file as "OPENAPI_FILE".

## Usage

Here is an example workflow. Save it as `action-update-api.yaml` in the `.github/workflows/` folder in the root of your GitHub repository.

```yaml
name: alphadoc-io/action-upload-api

on:
  pull_request:
    branches: [main]

jobs:
  upload_and_send:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Checkout alphadoc update-action
        uses: alphadoc-io/action-update-api@v3
        with:
          OPENAPI_FILE: "packages/ui/public/bikeshop-openapi.json"
          ORGANIZATION: ${{ vars.ALPHADOC_ORGANIZATION }}
          PROJECT_VERSION_ID: ${{ vars.ALPHADOC_PROJECT_VERSION_ID }}
          API_SPEC_ID: ${{ vars.ALPHADOC_API_SPEC_ID }}
          API_KEY: ${{ secrets.ALPHADOC_API_KEY }}
```

## Parameters

| Parameter          | Description                                                  |
| ------------------ | ------------------------------------------------------------ |
| OPENAPI_FILE       | Relative path to the OpenAPI file from the root of your repo |
| PROJECT_VERSION_ID | Alphadoc Project Version ID                                  |
| API_SPEC_ID        | Alphadoc API Specification ID                                |
| API_KEY            | Alphadoc API KEY                                             |

## Development

The main branch is `v3`. Contributions are welcome! Please create a
[Pull Request](https://github.com/alphadoc-io/action-update-api/pulls)
