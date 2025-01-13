
# Licenses Action

![GitHub release (latest by date)](https://img.shields.io/github/v/release/mvdkleijn/licenses-action)
![GitHub](https://img.shields.io/github/license/mvdkleijn/licenses-action)

A GitHub Action to run the [mvdkleijn/licenses](https://github.com/mvdkleijn/licenses) tool. This allows you to generate
a simple, human readable overview of the licenses mentioned in a provided SBOM.

## Features

- **Based on provided SBOM**: Use a provided (CycloneDX) SBOM file in XML or JSON formats.
- **Customizable Output**: Use custom Go template for the report output.
- **Easy Integration**: Integrate seamlessly with your CI/CD workflows.

## Inputs

- `sbom` (required): Path to the SBOM file to use.
- `type` (optional): The format of the SBOM file, either `xml` or `json`. Defaults to `xml`.
- `filename` (optional): The filename for the generated report file. Defaults to `licenses.md`.
- `template` (required): Template content used to generate the report.
- `evaluate` (optional): Whether or not to evaluate license compatibility based on the
                         way [mvdkleijn/licenses](https://github.com/mvdkleijn/licenses) works.

## Outputs

- `output`: Path to the generated report file.

## Usage

To use this action in your workflow, add the following step:

```yaml
jobs:
  licenses:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Run Licenses Action
        uses: mvdkleijn/licenses-action@v1
        with:
          sbom: sbom.xml
          type: xml
          filename: licenses.md
          template: |
            # Licenses

            The following third-party licenses are applicable to this project:

            {{range .SortedKeys}}## {{.}}

            {{range index $.ComponentsByLicense .}}- {{.Name}} ({{.Version}})
            {{end}}
            {{end}}
          evaluate: true
```

## Contributing

Contributions are welcome! Please open an issue or submit a pull request for any
changes you would like to make.

## License

This software is made available under the [MPL-2.0](https://choosealicense.com/licenses/mpl-2.0/) license.
The full details are available from the [LICENSE](/LICENSE) file.

Copyright (C) 2024  Martijn van der Kleijn
