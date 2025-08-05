# Crosswalk Artifact Documentation

A crosswalk represents mappings between two compliance frameworks, showing how controls or requirements in one framework relate to those in another. This helps organizations understand compliance overlap and coverage across multiple standards.

## Structure Overview

```
crosswalk/
└── package/
    └── {target_vendor}/
        └── {target_suite}/
            └── {crosswalk_name}/
                ├── package.json
                ├── index.yml
                ├── elements.yml
                ├── versions/
                │   └── 1.0.0.yml
                └── .npmrc
```

## Naming Convention

Crosswalk names follow this pattern:
`{target_version}_{source_vendor}_{source_suite}_{source_version}`

Examples:
- `2025_2_1_nist_sp_800_53_rev5` (SCF ← NIST)
- `2025_2_1_iso_27001_2022` (SCF ← ISO)

## Required Files

### index.yml

```yaml
id: string                # UUID v4 format
name: string              # Full crosswalk name
description: string       # Detailed description
externalId: string        # Same as name
code: string              # Crosswalk code
status: string            # Status: published or draft
sourceStandard: string    # Source framework code
targetStandard: string    # Target framework code (usually SCF)
```

### elements.yml

```yaml
elements:
  - id: string                      # UUID v4 for this mapping
    sourceElement: string           # Original element ID (preserved format)
    targetElement: string           # Target element ID (usually SCF control)
    relationshipType: string        # Type of relationship
    strengthOfRelationship: number  # 1-10 scale
```

### versions/1.0.0.yml

```yaml
id: string                # UUID v4 for this version
name: string              # Same as crosswalk name
description: string       # Same as crosswalk description  
status: string            # Version status: draft or published
elements:                 # Array of element IDs from elements.yml
  - element_id_1
  - element_id_2
  - ...
```

## Field Descriptions

### Relationship Types

- **equivalent**: Controls are functionally the same (strength: 10)
- **subset_of**: Source is a subset of target (strength: 7-8)
- **superset_of**: Source is a superset of target (strength: 7-8)
- **intersects**: Partial overlap between controls (strength: 4-6)

### Strength of Relationship

Scale from 1-10:
- **10**: Exact match/equivalent
- **7-9**: Strong alignment
- **4-6**: Moderate overlap
- **1-3**: Weak relationship

### Source/Target Standards

Format: `{vendor}.{suite}.{version}.framework`

Examples:
- `nist.sp_800_53.rev5.framework`
- `complianceforge.scf.2025_2_1.framework`

## Package.json Structure

```json
{
  "name": "@zerobias/crosswalk-{target_vendor}-{target_suite}-{crosswalk_name}",
  "version": "1.0.0",
  "description": "Crosswalk for {code}",
  "author": "team@neverfail.com",
  "license": "ISC",
  "repository": {
    "type": "git",
    "url": "git@github.com:zerobias-org/crosswalk.git",
    "directory": "package/{target_vendor}/{target_suite}/{crosswalk_name}"
  },
  "auditmation": {
    "package": "{code}.crosswalk",
    "import-artifact": "crosswalk",
    "dataloader-version": "3.6.6"
  },
  "engines": {
    "node": ">=16.11.0",
    "npm": ">=8.0.0"
  },
  "publishConfig": {
    "registry": "https://npm.pkg.github.com/"
  },
  "files": [
    "index.yml",
    "elements.yml",
    "versions/**"
  ],
  "dependencies": {
    "@zerobias/framework-{source_vendor}-{source_suite}-{source_version}": "^1.0.0",
    "@zerobias/framework-{target_vendor}-{target_suite}-{target_version}": "^1.0.0"
  },
  "scripts": {
    "nx:publish": "../../../scripts/publish.sh",
    "nx:prepublish": "../../../scripts/prepublish.sh"
  }
}
```

## Examples

### NIST to SCF Crosswalk

**index.yml**:
```yaml
id: d2dde880-9dad-11d1-80b4-00c04fd430c8
name: NIST SP 800-53 Rev 5 to Secure Controls Framework (SCF) 2025.2.1 Crosswalk
description: Crosswalk mappings from NIST SP 800-53 Rev 5 to the Secure Controls Framework (SCF) 2025.2.1
externalId: NIST SP 800-53 Rev 5 to Secure Controls Framework (SCF) 2025.2.1
code: nist_sp_800_53_rev5_complianceforge_scf_202521_crosswalk
status: published
sourceStandard: nist.sp_800_53.rev5.framework
targetStandard: complianceforge.scf.2025_2_1.framework
```

**elements.yml**:
```yaml
elements:
  - id: 28d2af76-68de-4844-a537-48efe5d3ebec
    sourceElement: AC-1
    targetElement: GOV-01
    relationshipType: equivalent
    strengthOfRelationship: 10
  - id: d449d17a-8213-438c-b527-3eddbfbf9293
    sourceElement: AC-2
    targetElement: IAC-01
    relationshipType: subset_of
    strengthOfRelationship: 8
  - id: 700cd432-6233-475e-bbd1-f02b45a2744a
    sourceElement: AC-2(1)
    targetElement: IAC-02
    relationshipType: intersects
    strengthOfRelationship: 6
```

### ISO to SCF Crosswalk

**Directory**: `complianceforge/scf/2025_2_1_iso_27001_2022/`

**elements.yml**:
```yaml
elements:
  - id: e3eef890-9dad-11d1-80b4-00c04fd430c8
    sourceElement: A.5.1.1
    targetElement: GOV-01
    relationshipType: equivalent
    strengthOfRelationship: 9
  - id: f4ff08a0-9dad-11d1-80b4-00c04fd430c8
    sourceElement: A.5.1.2
    targetElement: GOV-02
    relationshipType: subset_of
    strengthOfRelationship: 7
```

## Source Element Format

**Important**: Preserve the original element ID format from the source framework:

- NIST: `AC-1`, `AC-2(1)`, `SC-7`
- ISO: `A.5.1.1`, `A.6.2.1`
- PCI DSS: `1.1`, `2.2.1`
- AICPA TSC: `CC1.1`, `CC2.1`

Do NOT convert to lowercase or change the format.

## Target Element Format

For SCF targets, use the control number:
- `GOV-01`, `IAC-01`, `CAP-02`

NOT the control name or description.

## Dependencies

Every crosswalk MUST depend on both frameworks:

```json
"dependencies": {
  "@zerobias/framework-nist-sp_800_53-rev5": "^1.0.0",
  "@zerobias/framework-complianceforge-scf-2025_2_1": "^1.0.0"
}
```

## Common Mappings

### Relationship Type Selection

Choose based on coverage:
- **One-to-one match**: `equivalent` (strength: 9-10)
- **Source partially covers target**: `subset_of` (strength: 7-8)
- **Source fully covers target and more**: `superset_of` (strength: 7-8)
- **Some overlap but different focus**: `intersects` (strength: 4-6)

### Strength Guidelines

- **10**: Identical requirements
- **9**: Nearly identical with minor wording differences
- **8**: Same intent, different implementation
- **7**: Covers most aspects
- **6**: Covers about half
- **5**: Some overlap
- **3-4**: Minimal overlap
- **1-2**: Very weak connection

## Validation Rules

1. **Both frameworks exist**: Source and target must be valid
2. **Valid relationship types**: Must use allowed types
3. **Strength range**: Must be 1-10
4. **Element existence**: Elements should exist in frameworks
5. **No duplicate mappings**: Same source→target only once

## Contributing a New Crosswalk

1. Verify both frameworks exist
2. Analyze mapping relationships
3. Create directory structure
4. Add all required files
5. Map all relevant elements
6. Set appropriate relationships and strengths
7. Submit pull request

## Common Mistakes

- Changing source element format (preserve original)
- Using control names instead of IDs for SCF
- Missing framework dependencies
- Invalid relationship types
- Strength values outside 1-10 range
- Incomplete element listings in versions file

## Questions?

For questions about crosswalk artifacts, please open an issue in the [crosswalk repository](https://github.com/zerobias-org/crosswalk).