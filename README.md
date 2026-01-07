# Units of Measure Convention Metadata

- **UUID**: 3bbe438d-df37-49fe-8e2b-739296d46dfb5
- **Name**: License
- **Schema URL**: "<https://raw.githubusercontent.com/clbarnes/zarr-convention-uom/refs/tags/v1/schema.json>"
- **Spec URL**: "<https://github.com/clbarnes/zarr-convention-uom/blob/v1/README.md>"
- **Scope**: Array, Group
- **Extension Maturity Classification**: Proposal
- **Owner**: @clbarnes

## Description

This convention defines how to communicate the units of the data contained in a numeric Zarr array.
The license metadata exists in a field called `uom` at the root `attributes` level following the [Zarr Conventions Specification](https://github.com/zarr-conventions/zarr-conventions-spec).

Communicating the units of measure used by numeric is applicable across ecosystems and applications,
and should be made as low-friction as possible to facilitate re-use of FAIR data.

Units MUST be defined on each array individually.
Related arrays sharing a unit SHOULD validated at the application level.

- Examples:
  - **RECOMMENDED**: [SPDX specifier](examples/spdx.json)
  - [URL to license text](examples/url.json)
  - [License text](examples/text.json)
  - [File containing license text](examples/file.json)
  - [Path to Zarr node with license metadata](examples/path.json)

## Motivation

- **Comparability**: Units of measure are an essential piece of data for comparing raw data
- [**Not crashing your mars orbiter**](https://en.wikipedia.org/wiki/Mars_Climate_Orbiter)

## Convention Registration

The convention must be registered in `zarr_conventions`:

```json
{
  "zarr_conventions": [
    {
      "schema_url": "https://raw.githubusercontent.com/clbarnes/zarr-convention-uom/refs/tags/v1/schema.json",
      "spec_url": "https://github.com/clbarnes/zarr-convention-uom/blob/v1/README.md",
      "uuid": "3bbe438d-df37-49fe-8e2b-739296d46dfb5",
      "name": "uom",
      "description": "Units of measurement for Zarr arrays"
    }
  ]
}
```

## Applicable To

This convention can be used with these parts of the Zarr hierarchy:

- [ ] Group
- [x] Array

## Properties

The `uom` key at the root `attributes` level MUST be an object.

**Convention metadata name**: `uom`

| Field Name | Required | Type                      | Description |
| ---------- | -------- | ------------------------- | ----------- |
| uom        | yes      | [UOM Object](#uom-object) |             |

**Example**:

```json
{
  "attributes": {
    "zarr_conventions": [
      { "name": "uom", "uuid": "3bbe438d-df37-49fe-8e2b-739296d46dfb5" }
    ],
    "uom": {
      "ucum": {
        "version": "2.2",
        "unit": "kg"
      },
      "magnitude": 20,
      "description": "apple count as weight equivalent of one bushel"
    }
  }
}
```

### UOM Object

| Field Name  | Required         | Type                                     | Description                |
| ----------- | ---------------- | ---------------------------------------- | -------------------------- |
| ucum        | yes              | [UCUM Object](#ucum-object)              |                            |
| magnitude   | no, default `1`  | number                                   | Multiple of the given unit |
| description | no, default `""` | Free text description of the measurement |

### Additional Field Information

#### `"magnitude"` (`uom.magnitude`)

In some cases, the data in the zarr value may not be measured in whole units,
e.g. measuring elevation in increments of 5 feet.
In this case, this value measures increments.

If the data is in kilograms and `uom.magnitude = 20`, a value of 10 in the zarr array would represent 200kg.

### UCUM Object

| Field Name | Required                          | Type   | Description                   |
| ---------- | --------------------------------- | ------ | ----------------------------- |
| unit       | no, default `"1"`                 | string | UCUM unit string              |
| version    | no, default most recent supported | string | Version of UCUM specification |

#### `"unit"` (`uom.ucum.unit`)

This MUST be a valid **case-sensitive** unit string according to the [Unified Code for Units of Measure](https://ucum.org/ucum).

It is RECOMMENDED that the unit string does not contain a top-level magnitude term;
this SHOULD be stored in `uom.magnitude`.
If both are supplied, they MUST be multiplied together by the application.

By default (field omitted or set to `null`) the unit MUST be considered arbitrary.

## Known Implementations

### Libraries and Tools

- **[zarrs_conventions_license](https://github.com/clbarnes/zarrs_conventions/tree/main/zarrs_conventions_license)** - zarrs / zarrs_conventions implementation
  - Language: Rust
  - Status: Experimental
  - Maintainer: @clbarnes
  - Since: 2025-12-30

### Datasets Using This Convention

<!-- - **[Dataset Name](https://link-to-dataset)** - Description of the dataset and how it uses this convention -->

### Resources

_If you implement or use this convention, please add your implementation to this list by submitting a pull request._

## Acknowledgements

This template is based on the [STAC extensions template](https://github.com/stac-extensions/template/blob/main/README.md).
