# Pulumi Publish Go SDK Action

GitHub action for publishing a Go SDK to a tag via pushing a new commit with the specified source files.

This action uses its own isolated checkout of the repository, so any original checkout will be unaffected.

## Usage

```yaml
- uses: pulumi/publish-go-sdk-action@v1
  with:
    # Repository to publish the Go SDK to.
    # E.g. pulumi/pulumi-example-provider
    # This input is required.
    repository: ${{ github.repository }}
    # Ref to use as the parent commit. This can be a branch, tag or commit SHA.
    # This input is required.
    base-ref: ${{ github.sha }}
    # Path to the directory containing the Go source to be published. This folder should contain at least a go.mod file.
    # This input is required.
    source: sdk
    # Path within the repository to publish the go module (e.g. "sdk").
    # This will be used for coping the source into and prefixing the tag.
    # Optional: If not specified, the root of the repository will be assumed.
    path: test-sdk
    # Version to be used to publish the SDKs. This should be valid semver version 2 format.
    # This input is required.
    version: ${{ steps.version.outputs.version }}
    # Set to "true" to not delete all existing files before copying new source.
    # Including only the SDK files reduces the download for end users.
    # Default: false
    additive: false
```

## Examples

Publish the `sdk` folder to the current repository, using a version number provided by the Pulumi provider version action.

```yaml
- uses: actions/checkout@v4

- id: version
  uses: pulumi/provider-version-action@v1

- uses: pulumi/publish-go-sdk-action@v1
  with:
    repository: ${{ github.repository }}
    base-ref: ${{ github.sha }}
    source: sdk
    path: sdk
    version: ${{ steps.version.outputs.version }}
    additive: false
```
