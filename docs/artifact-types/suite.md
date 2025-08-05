# Suite Artifact Documentation

A suite represents a specific regulation, standard, or framework published by a vendor. Suites are the actual compliance documents (like GDPR, HIPAA, NIST SP 800-53) that organizations need to comply with.

## Structure Overview

```
suite/
└── package/
    └── {vendor_code}/
        └── {suite_code}/
            ├── package.json
            ├── index.yml
            ├── logo.svg
            └── .npmrc
```

## Required Fields

### index.yml

```yaml
code: string              # Unique suite identifier
name: string              # Full suite name
vendorCode: string        # Parent vendor code
vendorId: string          # Parent vendor UUID
vendorName: string        # Parent vendor name
type: string              # Always "suite"
status: string            # Status: active or inactive
id: string                # UUID v4 format
created: string           # ISO 8601 timestamp
updated: string           # ISO 8601 timestamp
description: string       # Brief description
url: string               # Official documentation URL (optional)
logo: string              # CDN URL for logo (optional)
```

## Field Descriptions

### code
- **Format**: Lowercase letters, numbers, underscores, hyphens
- **Examples**: `gdpr`, `hipaa`, `sp_800_53`, `privacy_act`
- **Note**: Should be short but recognizable

### name
- **Format**: Full official name of the regulation/standard
- **Examples**:
  - `General Data Protection Regulation`
  - `Health Insurance Portability and Accountability Act`
  - `NIST Special Publication 800-53`

### vendorCode, vendorId, vendorName
- **Important**: These MUST match an existing vendor
- **Example**:
  ```yaml
  vendorCode: nist
  vendorId: 550e8400-e29b-41d4-a716-446655440000
  vendorName: National Institute of Standards and Technology
  ```

### type
- **Value**: Always `suite` for suite artifacts

### status
- **Allowed values**:
  - `active` - Currently in force
  - `inactive` - Deprecated or superseded

## Package.json Structure

```json
{
  "name": "@zerobias/suite-{vendor_code}-{suite_code}",
  "version": "1.0.0",
  "description": "Suite package {suite_name} for vendor {vendor_name}.",
  "author": "team@neverfail.com",
  "license": "ISC",
  "publishConfig": {
    "registry": "https://npm.pkg.github.com/"
  },
  "repository": {
    "type": "git",
    "url": "git@github.com:zerobias-org/suite.git",
    "directory": "package/{vendor_code}/{suite_code}/"
  },
  "scripts": {
    "nx:prepublish": "../../../scripts/prepublish.sh",
    "nx:publish": "../../../scripts/publish.sh"
  },
  "files": ["index.yml", "logo.svg"],
  "auditmation": {
    "package": "{vendor_code}.{suite_code}.suite",
    "import-artifact": "suite",
    "dataloader-version": "3.5.7"
  },
  "dependencies": {
    "@zerobias/vendor-{vendor_code}": "^1.0.0"
  }
}
```

## Examples

### Government Suite (NIST SP 800-53)

```yaml
code: sp_800_53
name: NIST Special Publication 800-53
vendorCode: nist
vendorId: 550e8400-e29b-41d4-a716-446655440000
vendorName: National Institute of Standards and Technology
type: suite
status: active
id: 6ba7b810-9dad-11d1-80b4-00c04fd430c8
created: 2024-01-15T10:30:00.000Z
updated: 2024-01-15T10:30:00.000Z
description: Security and Privacy Controls for Information Systems and Organizations
url: https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final
logo: https://cdn.auditmation.io/logos/nist_sp_800_53.svg
```

### Regulation Suite (GDPR)

```yaml
code: gdpr
name: General Data Protection Regulation
vendorCode: eu
vendorId: 7ca7b820-9dad-11d1-80b4-00c04fd430c8
vendorName: European Union
type: suite
status: active
id: 8da8b830-9dad-11d1-80b4-00c04fd430c8
created: 2024-01-15T10:30:00.000Z
updated: 2024-01-15T10:30:00.000Z
description: Regulation on the protection of natural persons with regard to the processing of personal data
url: https://gdpr.eu/
logo: https://cdn.auditmation.io/logos/eu_gdpr.svg
```

### State Regulation (California Privacy Rights Act)

```yaml
code: cpra
name: California Privacy Rights Act
vendorCode: us_ca
vendorId: 9ea9b840-9dad-11d1-80b4-00c04fd430c8
vendorName: California, United States
type: suite
status: active
id: afaab850-9dad-11d1-80b4-00c04fd430c8
created: 2024-01-15T10:30:00.000Z
updated: 2024-01-15T10:30:00.000Z
description: California state privacy law that amends and expands the CCPA
url: https://oag.ca.gov/privacy/ccpa
logo: https://cdn.auditmation.io/logos/us_ca_cpra.svg
```

## Suite Code Generation Guidelines

### Acronym Extraction
1. **Known acronyms**: Use if widely recognized (GDPR, HIPAA, SOC)
2. **From parentheses**: Extract from name (e.g., "Act (POPIA)" → `popia`)
3. **Generate from words**: Create meaningful acronym (e.g., "Decision on Strengthening Network Information Protection" → `dsnip`)

### Examples of Code Generation
- `General Data Protection Regulation` → `gdpr`
- `Health Insurance Portability and Accountability Act` → `hipaa`
- `Payment Card Industry Data Security Standard` → `pci_dss`
- `California Consumer Privacy Act` → `ccpa`
- `NIST Special Publication 800-53` → `sp_800_53`

## Dependencies

### Vendor Dependency
Every suite MUST depend on its parent vendor package:

```json
"dependencies": {
  "@zerobias/vendor-nist": "^1.0.0"
}
```

This ensures the vendor exists before the suite is installed.

## Validation Rules

1. **Vendor exists**: vendorCode must match existing vendor
2. **Vendor ID match**: vendorId must match vendor's UUID
3. **Code uniqueness**: No duplicate suite codes within vendor
4. **Valid status**: Must be active or inactive
5. **Required dependency**: Must depend on parent vendor package

## Contributing a New Suite

1. Verify parent vendor exists
2. Check for duplicate suites
3. Generate appropriate suite code
4. Get vendor ID from vendor package
5. Create directory structure
6. Add all required files
7. Ensure vendor dependency is correct
8. Submit pull request

## Common Mistakes

- Incorrect vendor ID (must match exactly)
- Missing vendor dependency in package.json
- Using long suite codes (keep them short)
- Not following acronym conventions
- Missing .npmrc file

## Special Cases

### Multi-Part Standards
Some standards have multiple parts (e.g., ISO 27001, ISO 27002):
- Create separate suites for each part
- Use clear naming: `iso_27001`, `iso_27002`

### Versioned Regulations
When regulations have major versions:
- Suite represents the regulation family
- Framework represents specific versions

## Questions?

For questions about suite artifacts, please open an issue in the [suite repository](https://github.com/zerobias-org/suite).