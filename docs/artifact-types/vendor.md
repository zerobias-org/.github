# Vendor Artifact Documentation

A vendor represents an organization that publishes compliance frameworks, standards, or regulations.

## Structure Overview

```
vendor/
└── package/
    └── {vendor_code}/
        ├── package.json
        ├── index.yml
        └── logo.svg
```

## Required Fields

### index.yml

```yaml
code: string              # Unique identifier (lowercase, underscores)
name: string              # Full organization name
status: string            # Status: active or inactive
id: string                # UUID v4 format
created: string           # ISO 8601 timestamp
updated: string           # ISO 8601 timestamp
description: string       # Brief description of the vendor
url: string               # Official website (optional)
logo: string              # CDN URL for logo (optional)
ownerId: string           # Owner UUID (default: 00000000-0000-0000-0000-000000000000)
```

## Field Descriptions

### code
- **Format**: Lowercase letters, numbers, and underscores only
- **Examples**: `nist`, `iso`, `pci_ssc`, `sony_pictures`

### name
- **Format**: Proper organization name
- **Examples**: 
  - `National Institute of Standards and Technology`
  - `International Organization for Standardization`
  - `Sony Pictures Entertainment`

### status
- **Allowed values**:
  - `active` - Currently publishing standards
  - `inactive` - No longer publishing new standards

## Package.json Structure

```json
{
  "name": "@zerobias/vendor-{vendor_code}",
  "version": "1.0.0",
  "description": "Vendor package for {vendor_name}",
  "author": "team@neverfail.com",
  "license": "ISC",
  "publishConfig": {
    "registry": "https://npm.pkg.github.com/"
  },
  "scripts": {
    "nx:publish": "../../scripts/publish.sh"
  },
  "repository": {
    "type": "git",
    "url": "git@github.com:zerobias-org/vendor.git",
    "directory": "package/{vendor_code}/"
  },
  "files": ["index.yml", "logo.svg"],
  "auditmation": {
    "package": "{vendor_code}",
    "import-artifact": "vendor",
    "dataloader-version": "0.5.4"
  }
}
```

## Examples

### Example 1: NIST

```yaml
code: nist
name: National Institute of Standards and Technology
status: active
id: 550e8400-e29b-41d4-a716-446655440000
created: 2024-01-15T10:30:00.000Z
updated: 2024-01-15T10:30:00.000Z
description: Government agency that publishes compliance frameworks and standards.
url: https://www.nist.gov
logo: https://cdn.auditmation.io/logos/nist.svg
ownerId: 00000000-0000-0000-0000-000000000000
```

### Example 2: PCI SSC

```yaml
code: pci_ssc
name: Payment Card Industry Security Standards Council
status: active
id: 6ba7b810-9dad-11d1-80b4-00c04fd430c8
created: 2024-01-15T10:30:00.000Z
updated: 2024-01-15T10:30:00.000Z
description: Industry organization that publishes compliance frameworks and standards.
url: https://www.pcisecuritystandards.org
logo: https://cdn.auditmation.io/logos/pci_ssc.svg
ownerId: 00000000-0000-0000-0000-000000000000
```

### Example 3: Sony Pictures

```yaml
code: sony_pictures
name: Sony Pictures Entertainment
status: active
id: 6ba7b814-9dad-11d1-80b4-00c04fd430c8
created: 2024-01-15T10:30:00.000Z
updated: 2024-01-15T10:30:00.000Z
description: Entertainment industry organization that publishes compliance frameworks and standards.
url: https://www.sonypictures.com
logo: https://cdn.auditmation.io/logos/sony_pictures.svg
ownerId: 00000000-0000-0000-0000-000000000000
```

## Logo Requirements

- **Format**: SVG preferred, PNG accepted
- **Size**: 64x64 pixels minimum
- **Naming**: `logo.svg` or `logo.png`
- **Fallback**: If no logo provided, a placeholder will be generated

## Validation Rules

1. **Code uniqueness**: No duplicate vendor codes
2. **UUID format**: ID must be valid UUID v4
3. **Timestamp format**: ISO 8601 format required
4. **URL format**: Must be valid URL starting with http/https

## Contributing a New Vendor

1. Check if vendor already exists
2. Choose appropriate vendor code
3. Create the directory structure
4. Add required files
5. Submit pull request

## Common Mistakes

- Using spaces in vendor codes (use underscores)
- Missing required fields
- Invalid UUID format
- Incorrect timestamp format

## Questions?

For questions about vendor artifacts, please open an issue in the [vendor repository](https://github.com/zerobias-org/vendor).