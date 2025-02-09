# openapi2crd

[![Go Report Card](https://goreportcard.com/badge/github.com/fybrik/openapi2crd)](https://goreportcard.com/report/github.com/fybrik/openapi2crd)
[![Go Reference](https://pkg.go.dev/badge/github.com/fybrik/openapi2crd.svg)](https://pkg.go.dev/github.com/fybrik/openapi2crd)
[![golangci-lint](https://github.com/fybrik/openapi2crd/actions/workflows/golangci-lint.yml/badge.svg)](https://github.com/fybrik/openapi2crd/actions/workflows/golangci-lint.yml)
[![CodeQL](https://github.com/fybrik/openapi2crd/actions/workflows/codeql-analysis.yml/badge.svg)](https://github.com/fybrik/openapi2crd/actions/workflows/codeql-analysis.yml)
[![gosec](https://github.com/fybrik/openapi2crd/actions/workflows/golang-security.yml/badge.svg)](https://github.com/fybrik/openapi2crd/actions/workflows/golang-security.yml)


`openapi2crd` is a CLI to generate Kubernetes Custom Resource Definition (CRD) resources from [OpenAPI 3.0](https://www.openapis.org/).

## Install

Download the appropriate version for your platform from [Releases](https://github.com/fybrik/openapi2crd/releases/latest). You may want to install the binary to somewhere in your system's PATH such as `/usr/local/bin`.

Alternatively, if you have go 1.16 or later then you can also use `go install`. This will put `openapi2crd` in `$(go env GOPATH)/bin`:

```bash
go install fybrik.io/openapi2crd@latest
```

## Usage

1. Create an OpenAPI 3.0 document with `components.schemas` (see [example/spec.yaml](example/spec.yaml))
    * The document must comply with the listed [limitations](#limitations)
    * The document must include a component named the same as the `kind` you want to generate. 
1. Invoke `openapi2crd` command using one of the following methods:
    * Pass an input directory with `CustomResourceDefinition` resources lacking schema information (see [example/input](example/input)):
        ```bash
        openapi2crd SPEC_FILE --input INPUT_DIR --output OUTPUT_FILE
        ```
    * Pass the group version and kind of each `CustomResourceDefinition` you want to generate:
        ```bash
        openapi2crd SPEC_FILE --gvk GROUP/VERSION/KIND --output OUTPUT_FILE
        ```
    * A mix of the above is also valid

An output YAML file will be generated in the specified output location (see [example/output/output.yaml](example/output/output.yaml))

## Limitations

Only [structural schemas](https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definitions/#specifying-a-structural-schema) are allowed with the following exceptions:

1. You can use `$ref` as long as the referenced definition adheres to the listed limitations.
1. You can use `oneOf` that contains just a list of `$ref` items. This is translated to an object with a string `type` field and an optional property for each `$ref` item.

## Acknowledgements

The work is inspired by https://github.com/ant31/crd-validation and https://github.com/kubeflow/crd-validation.
