# action-update-api

This is a repository contains a [GitHub action](https://docs.github.com/en/actions)
that takes an [OpenAPI](https://www.openapis.org) spec file and uploads it
to [alphadoc](https://alphadoc.io).

## Setup

### Alphadoc Organisation

Follow [these steps](https://docs.github.com/en/actions/learn-github-actions/variables#creating-configuration-variables-for-a-repository) to set the Alphadoc Organisation as an environment variable for your repo. Name the variable "ALPHADOC_ORGANISATION" and assign the Organisation value to it. If my login link is https://demo.alphadoc.io, than my organisation name is `demo`.

### Alphadoc Project ID

Visit to the editor https://{organisation}.alphadoc.io/editor, click on your Project and retrieve the Project ID from the URL. The first value after `editor` is the Project ID. In our example the Project ID is `a6bf4715-111d-4d59-bcd4-de90aa725b93`.

```
https://demo.alphadoc.io/editor/a6bf4715-111d-4d59-bcd4-de90aa725b93/pages/26e3aba7-77e0-4f70-9554-8e827fe90b44
```

Follow the same steps as for the Organisation to configure the Project ID as a Github Action variable.

### Alphadoc Document ID

Visit to the editor (https://{yourcompanyname}.alphadoc.io/editor), click on your Project and go to the API upload section. Inspect the network tab and grab the ID that is returned in the `items` parameter in the `GET /documents` endpoint.

Follow the same steps as for the Project ID to configure the Document ID as a Github Action variable.

### Alphadoc Username and Password

As the final step of the process, generate an Alpadoc API Key by visiting
`<ORGANIZATION>.alphadoc.io/editor/settings/organization?section=api_key`
and clicking `Generate API Key`.

Copy the generated API key to you clipboard and go to
[secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets#about-encrypted-secrets)
in Github. Save the secret as "ALPHADOC_APIKEY".

### Location of OpenAPI file

The final step is to add the location of your OpenAPI file in your repo to the workflow .yaml file as "OPENAPI_FILE".

## Usage
### Generated Action
You can generate a GitHub Action per API directly from the "API Uploads" section in Alphadoc. Just copy it, and set up the location of your API and your username/password.

### Custom Action

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
        uses: alphadoc-io/action-update-api@v1
        with:
          OPENAPI_FILE: "packages/ui/public/bikeshop-openapi.json"
          ORGANISATION: ${{ vars.ALPHADOC_ORGANISATION }}
          PROJECT_ID: ${{ vars.ALPHADOC_PROJECT_ID }}
          DOCUMENT_ID: ${{ vars.ALPHADOC_DOCUMENT_ID }}
          APIKEY: ${{ secrets.ALPHADOC_APIKEY }}
```

## Parameters

| Parameter    | Description                                                  |
| ------------ | ------------------------------------------------------------ |
| OPENAPI_FILE | Relative path to the OpenAPI file from the root of your repo |
| PROJECT_ID   | Alphadoc Project ID                                          |
| DOCUMENT_ID  | Alphadoc Document ID                                         |
| APIKEY       | Alphadoc API KEY                                             |

## Development

The master branch is `v1`. Contributions are welcome! Please create a
[Pull Request](https://github.com/alphadoc-io/action-update-api/pulls)
