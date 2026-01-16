# Agent Steering Guide

This document provides guidance for AI agents working with this Renovate configuration repository.

## Project Overview

This is a shared Renovate configuration repository that provides reusable presets for dependency management across multiple projects. It is NOT an Amazon internal package - it's a standard open-source project using Renovate Bot.

## Repository Structure

```
.
├── default.json          # Main config extending all presets
├── managers/            # Custom manager configurations
│   └── aws.json        # Smithy package manager regex
└── packages/           # Package-specific rules
    ├── aws.json        # AWS SDK grouping rules
    └── noisy-npm.json  # NPM package scheduling
```

## Key Concepts

### Renovate Configuration Files

All JSON files follow the [Renovate schema](https://docs.renovatebot.com/renovate-schema.json). When modifying:

- Validate against the schema
- Use proper Renovate field names (e.g., `matchDatasources` not `matchDataSources`)
- Test configurations before committing

### Package Rules

Package rules control how Renovate handles dependency updates:

- `matchPackageNames`: Exact package name matches
- `matchPackagePatterns`: Regex patterns for package names
- `matchDatasources`: Source type (npm, pip, maven, etc.)
- `groupName`/`groupSlug`: Group related updates together
- `schedule`: When updates should be created

### Custom Managers

Custom managers (in `managers/`) define how to extract dependencies from non-standard files using regex patterns.

## Common Tasks

### Adding New Package Rules

1. Determine the appropriate file:
   - AWS-related packages → `packages/aws.json`
   - Noisy NPM packages → `packages/noisy-npm.json`
   - Create new file if needed

2. Add rule with proper structure:
   ```json
   {
     "description": ["Clear description"],
     "matchDatasources": ["npm"],
     "matchPackageNames": ["package-name"],
     "groupName": "group-name",
     "schedule": ["on saturday"]
   }
   ```

3. Update `default.json` if adding new preset file

### Adding Custom Managers

1. Create or update file in `managers/`
2. Define regex pattern to extract:
   - `packageName`: Dependency identifier
   - `currentValue`: Current version
3. Specify `datasourceTemplate` and `versioningTemplate`
4. Reference in `default.json` extends array

### Testing Changes

Before committing:
1. Validate JSON syntax
2. Check against Renovate schema
3. Test regex patterns if using custom managers
4. Consider impact on consuming repositories

## Best Practices

- Keep configurations modular and reusable
- Use descriptive names for groups and files
- Schedule noisy updates (like linters) for weekends
- Group related packages together
- Document the purpose of each rule

## Common Pitfalls

- **Case sensitivity**: `matchDatasources` vs `matchDataSources` - use lowercase 's'
- **Regex escaping**: In JSON, backslashes must be escaped (`\\` not `\`)
- **Schema validation**: Always validate against the Renovate schema
- **Circular extends**: Don't create circular references in extends

## External Resources

- [Renovate Documentation](https://docs.renovatebot.com/)
- [Configuration Options](https://docs.renovatebot.com/configuration-options/)
- [Package Rules](https://docs.renovatebot.com/configuration-options/#packagerules)
- [Custom Managers](https://docs.renovatebot.com/modules/manager/regex/)
