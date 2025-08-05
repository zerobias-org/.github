# Contributing to Zerobias Compliance Packages

Thank you for your interest in contributing to our compliance framework ecosystem! This guide will help you understand how to contribute new artifacts or improve existing ones.

## Table of Contents

- [Getting Started](#getting-started)
- [Types of Contributions](#types-of-contributions)
- [Artifact Structure](#artifact-structure)
- [Submission Process](#submission-process)
- [Code of Conduct](#code-of-conduct)

## Getting Started

### Prerequisites

- Basic understanding of compliance frameworks
- Familiarity with YAML/JSON format
- GitHub account

### Repository Structure

Our compliance packages are organized into four main types:

1. **Vendor** - Organizations that publish standards
2. **Suite** - Specific regulations or standards
3. **Framework** - Versioned implementations
4. **Crosswalk** - Mappings between frameworks

## Types of Contributions

### 1. New Vendor

Add a new organization that publishes compliance standards.

**Example**: Adding Sony Pictures as a vendor for their security standards.

### 2. New Suite

Add a new regulation or standard from an existing vendor.

**Example**: Adding a new privacy regulation from a state government.

### 3. New Framework

Add a specific version of a compliance framework.

**Example**: Adding ISO 27001:2022 as a new framework version.

### 4. Crosswalk Mappings

Create or improve mappings between different frameworks.

**Example**: Mapping NIST controls to ISO 27001 requirements.

## Artifact Structure

Each artifact type has a specific structure. See our detailed documentation:

- [Vendor Structure](artifact-types/vendor.md)
- [Suite Structure](artifact-types/suite.md)
- [Framework Structure](artifact-types/framework.md)
- [Crosswalk Structure](artifact-types/crosswalk.md)

## Submission Process

### 1. Fork the Repository

Fork the appropriate repository (vendor, suite, framework, or crosswalk).

### 2. Create Your Branch

```bash
git checkout -b add-vendor-sony-pictures
```

### 3. Add Your Artifact

Follow the structure defined in our artifact documentation.

### 4. Validate Your Changes

Ensure your artifact:
- Follows the correct structure
- Has all required fields
- Uses consistent naming conventions
- Includes accurate metadata

### 5. Submit Pull Request

1. Push your branch to your fork
2. Create a pull request with:
   - Clear title (e.g., "Add Sony Pictures vendor")
   - Description of what you're adding
   - Any relevant references or documentation

### 6. Review Process

Our team will review your submission for:
- Completeness
- Accuracy
- Compliance with our standards
- No duplicate entries

## Validation

Before submitting, validate your artifact against our schemas:

- [Vendor Schema](schemas/vendor.schema.json)
- [Suite Schema](schemas/suite.schema.json)
- [Framework Schema](schemas/framework.schema.json)
- [Crosswalk Schema](schemas/crosswalk.schema.json)

## Examples

Check our [examples directory](examples/) for real-world artifacts:
- [Example Vendor](examples/vendor-example/)
- [Example Suite](examples/suite-example/)
- [Example Framework](examples/framework-example/)
- [Example Crosswalk](examples/crosswalk-example/)

## Questions?

- Open an issue in the relevant repository
- Join our [GitHub Discussions](https://github.com/orgs/zerobias-org/discussions)

## Code of Conduct

Please note that this project is released with a [Contributor Code of Conduct](CODE_OF_CONDUCT.md). By participating in this project you agree to abide by its terms.

---

Thank you for contributing to the compliance automation ecosystem! 🎉