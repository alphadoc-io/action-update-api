# action-update-api

This is a repository contains a [GitHub action](https://docs.github.com/en/actions)
that takes an [OpenAPI](https://www.openapis.org) spec file and uploads it
to [alphadoc](https://alphadoc.io).

## Usage

Here is an example workflow. It should be placed in the folder `.github/workflows/`
in the root of your GitHub repository.

```yaml
name: Test alphadoc-io/action-upload-api

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
          PROJECT_ID: "a6bf4715-111d-4d59-bcd4-de90aa725b94"
          DOCUMENT_ID: "bb8f93ca-e0b6-4176-8209-54aab38896b7"
          USERNAME: ${{ secrets.ALPHADOC_USERNAME }}
          PASSWORD: ${{ secrets.ALPHADOC_PASSWORD }}
```

## Parameters

| Parameter    | Description                                                  |
| ------------ | ------------------------------------------------------------ |
| OPENAPI_FILE | Relative path to the OpenAPI file from the root of your repo |
| PROJECT_ID   | Alphadoc Project ID                                          |
| DOCUMENT_ID  | Alphadoc Document ID                                         |
| USERNAME     | Name of the user                                             |
| PASSWORD     | Password of the user. Store this in a Github Secret.         |
