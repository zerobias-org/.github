# Zerobias Compliance Framework Documentation

Welcome to the Zerobias compliance framework documentation. This guide helps contributors understand how to create and maintain compliance artifacts in our ecosystem.

## Quick Links

- [Contributing Guide](CONTRIBUTING.md) - How to contribute
- [Artifact Types](artifact-types/) - Detailed documentation for each artifact type
- [Examples](examples/) - Real-world examples
- [Schemas](schemas/) - JSON/YAML validation schemas

## Overview

Our compliance framework ecosystem consists of four interconnected artifact types:

### 1. [Vendors](artifact-types/vendor.md)
Organizations that publish compliance standards (e.g., NIST, ISO, PCI SSC)

### 2. [Suites](artifact-types/suite.md)
Specific regulations or standards from vendors (e.g., GDPR, HIPAA, NIST SP 800-53)

### 3. [Frameworks](artifact-types/framework.md)
Versioned implementations with actual controls (e.g., NIST SP 800-53 Rev 5, ISO 27001:2022)

### 4. [Crosswalks](artifact-types/crosswalk.md)
Mappings between different frameworks (e.g., NIST → SCF, ISO → SCF)

## Dependency Structure

```
Vendor
  └── Suite (depends on Vendor)
      └── Framework (depends on Suite)
          └── Crosswalk (depends on both Frameworks)
```

## Package Naming Convention

All packages use the `@zerobias/` npm scope:

- **Vendor**: `@zerobias/vendor-{vendor_code}`
- **Suite**: `@zerobias/suite-{vendor_code}-{suite_code}`
- **Framework**: `@zerobias/framework-{vendor}-{suite}-{version}`
- **Crosswalk**: `@zerobias/crosswalk-{target}-{source}`

## Getting Started

### For Contributors

1. **Identify what you want to add**: Is it a new vendor, suite, framework, or crosswalk?
2. **Check if it already exists**: Search the relevant repository
3. **Read the documentation**: Review the specific [artifact type documentation](artifact-types/)
4. **Follow the examples**: Look at our [example artifacts](examples/)
5. **Submit a PR**: Follow our [contributing guide](CONTRIBUTING.md)

### For Maintainers

1. **Validate structure**: Use our [schemas](schemas/) to validate submissions
2. **Check dependencies**: Ensure all dependencies exist
3. **Verify metadata**: Confirm accuracy of descriptions and URLs
4. **Test locally**: Build and test the package before merging

## Common Use Cases

### Adding a New Compliance Standard

To add a completely new compliance standard (e.g., a new ISO standard):

1. **Add the vendor** (if not exists): ISO organization
2. **Add the suite**: The specific standard family (e.g., ISO 27001)
3. **Add the framework**: The versioned implementation (e.g., ISO 27001:2022)
4. **Add crosswalks**: Map to SCF or other frameworks

### Adding a New Version

When a standard releases a new version:

1. **Keep existing framework**: Don't modify old versions
2. **Create new framework**: Add the new version as a separate framework
3. **Update crosswalks**: Create new mappings for the new version

### Adding State Regulations

US state regulations follow a special pattern:

1. **Vendor code**: `us_{state_code}` (e.g., `us_ca` for California)
2. **Vendor name**: `{State}, United States` (e.g., `California, United States`)
3. **Type**: `regional`

## Validation

Before submitting, validate your artifacts:

1. **Structure validation**: Matches required directory structure
2. **Schema validation**: Conforms to JSON/YAML schemas
3. **Dependency validation**: All referenced packages exist
4. **Content validation**: Accurate metadata and descriptions

## Support

- **Questions**: Open an issue in the relevant repository
- **Discussions**: Join our [GitHub Discussions](https://github.com/orgs/zerobias-org/discussions)
- **Security**: Report security issues privately to security@zerobias.com

## License

All public artifacts are released under the ISC license. See individual repositories for specific terms.

---

*Building compliance automation for everyone* 🚀