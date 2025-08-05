# Framework Artifact Documentation

A framework represents a specific version of a compliance standard or regulation. Frameworks contain the actual controls, requirements, or elements that organizations must implement.

## Structure Overview

```
framework/
└── package/
    └── {vendor_code}/
        └── {suite_code}/
            └── {version}/
                ├── package.json
                ├── index.yml
                ├── elements/
                │   ├── {element_id_1}.yml
                │   ├── {element_id_2}.yml
                │   └── ...
                └── .npmrc
```

## Required Fields

### index.yml

```yaml
id: string                # UUID v4 format
code: string              # Framework code
name: string              # Full framework name
description: string       # Detailed description
url: string               # Official documentation URL (optional)
status: string            # Status: approved or draft
external: boolean         # True for non-SCF frameworks
internal: boolean         # True for SCF framework
vendor: string            # Vendor code
suite: string             # Suite code
version: string           # Version identifier
elementTypes:             # Array of element type definitions
  - id: string
    code: string
    name: string
    description: string
mappingTypes: array       # Supported mapping types (optional)
```

## Field Descriptions

### code
- **Format**: `{vendor}.{suite}.{version}.framework`
- **Examples**: 
  - `nist.sp_800_53.rev5.framework`
  - `iso.27001.2022.framework`
  - `complianceforge.scf.2025_2_1.framework`

### version
- **Format**: Version with underscores replacing dots
- **Examples**: `rev5`, `2022`, `v6_0_3`, `2025_2_1`
- **Note**: Extracted from column names or LRF names

### status
- **Allowed values**:
  - `approved` - Finalized and ready for use
  - `draft` - Under development

### external/internal
- **external**: True for all non-SCF frameworks
- **internal**: True only for SCF master framework
- Only one should be true

### elementTypes
Defines the types of controls/requirements in this framework:

```yaml
elementTypes:
  - id: 550e8400-e29b-41d4-a716-446655440000
    code: control
    name: Control
    description: Security or privacy control
```

Common element types:
- `control` - Security/privacy controls
- `requirement` - Compliance requirements
- `principle` - High-level principles
- `safeguard` - Protective measures

## Package.json Structure

```json
{
  "name": "@zerobias/framework-{vendor}-{suite}-{version}",
  "version": "1.0.0",
  "description": "Framework {name}",
  "author": "team@neverfail.com",
  "license": "ISC",
  "publishConfig": {
    "registry": "https://npm.pkg.github.com/"
  },
  "repository": {
    "type": "git",
    "url": "git@github.com:zerobias-org/framework.git",
    "directory": "package/{vendor}/{suite}/{version}/"
  },
  "scripts": {
    "nx:prepublish": "../../../../scripts/prepublish.sh",
    "nx:publish": "../../../../scripts/publish.sh"
  },
  "files": ["index.yml", "elements/**/*.yml"],
  "auditmation": {
    "package": "{vendor}.{suite}.{version}.framework",
    "import-artifact": "framework",
    "dataloader-version": "3.6.0"
  },
  "dependencies": {
    "@zerobias/suite-{vendor}-{suite}": "^1.0.0"
  }
}
```

## Element Structure

Each control/requirement is stored as a separate YAML file in the `elements/` directory:

### elements/{element_id}.yml

```yaml
id: string                # UUID v4 format
externalId: string        # Original framework identifier (lowercase)
name: string              # Element title
description: string       # Full element description
elementType: string       # Type code (e.g., "control")
```

### Example Element

```yaml
# elements/ac-1.yml
id: 6ba7b810-9dad-11d1-80b4-00c04fd430c8
externalId: ac-1
name: Access Control Policy and Procedures
description: The organization develops, documents, and disseminates access control policy and procedures.
elementType: control
```

## Examples

### Government Framework (NIST SP 800-53 Rev 5)

**index.yml**:
```yaml
id: 7ca7b820-9dad-11d1-80b4-00c04fd430c8
code: nist.sp_800_53.rev5.framework
name: NIST SP 800-53 Rev 5 - Security and Privacy Controls
description: Security and Privacy Controls for Information Systems and Organizations
url: https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final
status: approved
external: true
internal: false
vendor: nist
suite: sp_800_53
version: rev5
elementTypes:
  - id: 8da8b830-9dad-11d1-80b4-00c04fd430c8
    code: control
    name: Control
    description: Security or privacy control from NIST SP 800-53
mappingTypes: []
```

### Standards Framework (ISO 27001:2022)

**index.yml**:
```yaml
id: 9ea9b840-9dad-11d1-80b4-00c04fd430c8
code: iso.27001.2022.framework
name: ISO/IEC 27001:2022 Information Security Management
description: Requirements for establishing, implementing, maintaining and continually improving an information security management system
url: https://www.iso.org/standard/27001
status: approved
external: true
internal: false
vendor: iso
suite: 27001
version: "2022"
elementTypes:
  - id: afaab850-9dad-11d1-80b4-00c04fd430c8
    code: requirement
    name: Requirement
    description: ISO 27001 control requirement
mappingTypes: []
```

### SCF Master Framework

**index.yml**:
```yaml
id: b0bbc860-9dad-11d1-80b4-00c04fd430c8
code: complianceforge.scf.2025_2_1.framework
name: Secure Controls Framework (SCF) 2025.2.1
description: A comprehensive catalog of cybersecurity and privacy controls
url: https://www.securecontrolsframework.com
status: approved
external: false
internal: true
vendor: complianceforge
suite: scf
version: 2025_2_1
elementTypes:
  - id: c1ccd870-9dad-11d1-80b4-00c04fd430c8
    code: control
    name: SCF Control
    description: Secure Controls Framework control
mappingTypes: ["nist", "iso", "pci", "gdpr"]
```

## Version Extraction Rules

### From Column Names
- `NIST SP 800-53 Rev 5` → `rev5`
- `ISO 27001:2022` → `2022`
- `TISAX ISA v6` → `v6`

### From LRF Names
- `TISAX ISA 6.0.3` → `v6_0_3`
- `NIST CSF v1.1` → `v1_1`
- `PCI DSS 4.0` → `v4_0`

### Special Cases
- Always prefer more specific versions
- Replace dots with underscores for filesystem compatibility
- Preserve "rev", "v", or version prefixes

## Dependencies

### Suite Dependency
External frameworks MUST depend on their parent suite:

```json
"dependencies": {
  "@zerobias/suite-nist-sp_800_53": "^1.0.0"
}
```

SCF framework has no dependencies (it's the master).

## Directory Naming

The directory structure must match exactly:
```
package/{vendor}/{suite}/{version}/
```

Examples:
- `package/nist/sp_800_53/rev5/`
- `package/iso/27001/2022/`
- `package/complianceforge/scf/2025_2_1/`

## Validation Rules

1. **Suite exists**: Parent suite must exist
2. **Version format**: Valid version string with underscores
3. **Element IDs**: All elements must have unique IDs
4. **External IDs**: Element external IDs must be lowercase
5. **Element types**: Must be defined in elementTypes array

## Contributing a New Framework

1. Verify parent vendor and suite exist
2. Determine correct version format
3. Create directory structure
4. Add index.yml with metadata
5. Add all framework elements
6. Ensure suite dependency is correct
7. Submit pull request

## Common Mistakes

- Using dots in versions (use underscores)
- Forgetting to lowercase external IDs
- Missing suite dependency
- Incorrect directory structure
- Not defining element types

## Element Guidelines

### External ID Format
- Always lowercase
- Preserve original format where possible
- Examples: `ac-1`, `a.5.1.1`, `4.1`, `req-1`

### Element Descriptions
- Use official text from the framework
- Include full requirement/control text
- Don't abbreviate or summarize

## Questions?

For questions about framework artifacts, please open an issue in the [framework repository](https://github.com/zerobias-org/framework).