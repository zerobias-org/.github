# Validation Schemas

This directory contains JSON Schema files for validating compliance framework artifacts. Use these schemas to ensure your contributions meet the required structure and data types.

## Available Schemas

### Core Artifacts
- **[vendor.schema.json](vendor.schema.json)** - Validates vendor index.yml files
- **[suite.schema.json](suite.schema.json)** - Validates suite index.yml files  
- **[framework.schema.json](framework.schema.json)** - Validates framework index.yml files
- **[crosswalk.schema.json](crosswalk.schema.json)** - Validates crosswalk index.yml files

### Supporting Files
- **[framework-element.schema.json](framework-element.schema.json)** - Validates individual framework element files
- **[crosswalk-elements.schema.json](crosswalk-elements.schema.json)** - Validates crosswalk elements.yml files

## Usage

### Command Line Validation

Using [ajv-cli](https://github.com/ajv-validator/ajv-cli):

```bash
# Install ajv-cli
npm install -g ajv-cli

# Validate a vendor file
ajv validate -s vendor.schema.json -d /path/to/vendor/index.yml

# Validate a suite file  
ajv validate -s suite.schema.json -d /path/to/suite/index.yml

# Validate framework elements
ajv validate -s framework-element.schema.json -d /path/to/framework/elements/ac-1.yml
```

### Online Validation

You can also validate using online JSON Schema validators:
- [JSONSchemaLint](https://jsonschemalint.com/)
- [Schema Validator](https://www.jsonschemavalidator.net/)

## Schema Details

### Common Patterns

#### UUID v4 Format
```json
"pattern": "^[0-9a-f]{8}-[0-9a-f]{4}-4[0-9a-f]{3}-[89ab][0-9a-f]{3}-[0-9a-f]{12}$"
```

#### Lowercase Codes
```json
"pattern": "^[a-z0-9_]+$"
```

#### ISO 8601 Timestamps
```json
"format": "date-time"
```

### Validation Rules

#### Vendor Schema
- Code: lowercase, underscores only
- Status: active or inactive
- ID: valid UUID v4

#### Suite Schema  
- Must reference existing vendor (vendorCode, vendorId, vendorName)
- Type: always "suite"
- Code: lowercase, underscores, hyphens allowed

#### Framework Schema
- Code: format `vendor.suite.version.framework`
- External/Internal: exactly one must be true
- ElementTypes: at least one element type required
- Version: underscores instead of dots

#### Crosswalk Schema
- Standards: must reference valid framework codes
- Status: published or draft
- Code: must end with "_crosswalk"

#### Element Schemas
- ExternalId: lowercase, preserve original format
- RelationshipType: equivalent/subset_of/superset_of/intersects
- Strength: integer 1-10

## Validation Examples

### Valid Vendor
```yaml
code: sony_pictures
name: Sony Pictures Entertainment
status: active
id: 12345678-1234-5678-9abc-123456789abc
created: 2024-08-04T10:30:00.000Z
updated: 2024-08-04T10:30:00.000Z
description: Entertainment industry organization that publishes compliance frameworks and standards.
ownerId: 00000000-0000-0000-0000-000000000000
url: https://www.sonypictures.com
logo: https://cdn.auditmation.io/logos/sony_pictures.svg
```

### Common Validation Errors

1. **Invalid UUID format**: Must be valid UUID v4
2. **Wrong case in codes**: Must be lowercase
3. **Invalid enum values**: Must match allowed values exactly
4. **Missing required fields**: All required fields must be present
5. **Invalid timestamp format**: Must be ISO 8601

## Contributing

When adding new artifact types or modifying existing schemas:

1. Update the relevant schema file
2. Add examples to the schema
3. Test with real artifact files
4. Update this README with any new patterns

## Questions?

For questions about schemas or validation, please open an issue in the relevant repository or join our [GitHub Discussions](https://github.com/orgs/zerobias-org/discussions).