# Renovate Config

Shared [Renovate](https://docs.renovatebot.com/) configuration presets for consistent dependency management across projects.

## Usage

Reference this config in your `renovate.json`:

```json
{
  "extends": ["github>layertwo/renovate-config"]
}
```

Or extend specific presets:

```json
{
  "extends": [
    "github>layertwo/renovate-config//packages/aws",
    "github>layertwo/renovate-config//packages/noisy-npm"
  ]
}
```

## Available Presets

### Default Config (`default.json`)

The main preset that includes all configurations:

- Recommended Renovate defaults
- Docker major version updates
- Merge confidence badges
- Semantic commits
- AWS package grouping
- NPM package scheduling
- Custom AWS managers

### Package Rules

#### `packages/aws` - AWS Package Grouping

Groups AWS-related packages for coordinated updates:

- **aws-cdk-lib**: Groups `aws-cdk-lib` and `aws-cdk` (weekly on Saturday)
- **boto3**: Groups `boto3` and `types-boto3` (weekly on Saturday)
- **smithy**: Groups all Smithy packages

#### `packages/noisy-npm` - NPM Package Scheduling

Schedules noisy NPM packages for weekend updates:

- TypeScript ESLint plugins
- Prettier and ESLint configs
- `@types/node`

All scheduled for Saturday to reduce weekday noise.

### Custom Managers

#### `managers/aws` - Smithy Build Manager

Extracts Smithy dependencies from `smithy-build.json` files using regex patterns.

Matches dependencies in format:
```json
"software.amazon.smithy:smithy-model:1.2.3"
```

## Examples

### Basic Setup

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>layertwo/renovate-config"]
}
```

### Custom Configuration

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": [
    "config:recommended",
    "github>layertwo/renovate-config//packages/aws",
    "github>layertwo/renovate-config//packages/noisy-npm"
  ],
  "packageRules": [
    {
      "matchPackageNames": ["my-package"],
      "schedule": ["on monday"]
    }
  ]
}
```

### AWS CDK Project

```json
{
  "extends": [
    "github>layertwo/renovate-config//packages/aws",
    "github>layertwo/renovate-config//packages/noisy-npm"
  ]
}
```

### Smithy Project

```json
{
  "extends": [
    "github>layertwo/renovate-config//packages/aws",
    "github>layertwo/renovate-config//managers/aws"
  ]
}
```

## Contributing

### Adding New Presets

1. Create a new JSON file in the appropriate directory:
   - `packages/` for package-specific rules
   - `managers/` for custom manager configurations

2. Follow the structure:
   ```json
   {
     "$schema": "https://docs.renovatebot.com/renovate-schema.json",
     "packageRules": [
       {
         "description": ["Clear description"],
         "matchDatasources": ["npm"],
         "matchPackageNames": ["package-name"],
         "groupName": "group-name"
       }
     ]
   }
   ```

3. Update `default.json` if the preset should be included by default

4. Document the preset in this README

### Testing

Validate your changes:

```bash
# Validate JSON syntax
cat packages/your-preset.json | jq .

# Test in a project
# Add to renovate.json and run Renovate in dry-run mode
```

## License

See [LICENSE](LICENSE) file.

## Resources

- [Renovate Documentation](https://docs.renovatebot.com/)
- [Configuration Options](https://docs.renovatebot.com/configuration-options/)
- [Shareable Config Presets](https://docs.renovatebot.com/config-presets/)
