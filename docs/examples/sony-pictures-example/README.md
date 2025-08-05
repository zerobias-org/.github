# Sony Pictures Vendor Example

This example demonstrates how to add Sony Pictures Entertainment as a new vendor to the compliance framework ecosystem.

## Step 1: Vendor Structure

Create the vendor directory structure:

```
vendor/package/sony_pictures/
├── package.json
├── index.yml
├── logo.svg
└── .npmrc
```

## Step 2: package.json

```json
{
  "name": "@zerobias/vendor-sony_pictures",
  "version": "1.0.0",
  "description": "Vendor package for Sony Pictures Entertainment",
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
    "directory": "package/sony_pictures/"
  },
  "files": ["index.yml", "logo.svg"],
  "auditmation": {
    "package": "sony_pictures",
    "import-artifact": "vendor",
    "dataloader-version": "0.5.4"
  }
}
```

## Step 3: index.yml

```yaml
code: sony_pictures
name: Sony Pictures Entertainment
status: active
id: 12345678-1234-5678-9abc-123456789abc
created: 2024-08-04T10:30:00.000Z
updated: 2024-08-04T10:30:00.000Z
description: Entertainment industry organization that publishes compliance frameworks and standards.
url: https://www.sonypictures.com
logo: https://cdn.auditmation.io/logos/sony_pictures.svg
ownerId: 00000000-0000-0000-0000-000000000000
```

## Step 4: Logo (Optional)

Create a placeholder SVG logo or provide a proper logo file:

```svg
<svg width="64" height="64" viewBox="0 0 64 64" xmlns="http://www.w3.org/2000/svg">
  <rect width="64" height="64" fill="#f0f0f0" stroke="#ccc" stroke-width="1"/>
  <text x="32" y="40" text-anchor="middle" font-family="Arial, sans-serif" font-size="12" font-weight="bold" fill="#666">SPE</text>
</svg>
```

## Key Points

1. **Unique Code**: `sony_pictures` follows the naming convention
2. **Generated ID**: Use a valid UUID v4
3. **Current Timestamps**: Use ISO 8601 format

## Next Steps

After adding the vendor, you could add:

1. **Sony Pictures Suite**: A specific compliance program
2. **Sony Pictures Framework**: Versioned security requirements
3. **Crosswalks**: Map to other frameworks like SCF

## Validation

Before submitting:

- [ ] Code follows naming convention
- [ ] All required fields present
- [ ] Valid UUID format
- [ ] Correct timestamps

This vendor would then be available for creating dependent suites and frameworks!