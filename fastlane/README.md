# Fastlane Configuration for Ionic Capacitor Android Debug Build

This directory contains the Fastlane configuration for building your Ionic Capacitor Android debug APK in Bitrise CI.

## Available Lanes

### Android Lanes
- `debug` - Builds a debug APK

## Usage in Bitrise CI

### Running Fastlane Lanes

To run the debug lane in Bitrise, use the "Script" step with:

```bash
# Install dependencies
bundle install

# Run the debug lane
bundle exec fastlane debug
```

### Environment Variables

Set these environment variables in Bitrise:

#### General:
- `BITRISE_BUILD_NUMBER` - Build number (used for version code)
- `BITRISE_BUILD_URL` - Build URL (used for artifact uploads)

## Bitrise Workflow Example

Here's an example Bitrise workflow configuration:

```yaml
workflows:
  debug:
    steps:
    - activate-ssh-key@4:
        run_if: '{{getenv "SSH_RSA_PRIVATE_KEY" | length}}'
    - git-clone@6: {}
    - cache-pull@2: {}
    - install-missing-android-tools@3:
        inputs:
        - gradle_version: '8.0'
    - script@1:
        title: Install Ruby dependencies
        inputs:
        - content: |
            #!/bin/bash
            set -e
            gem install bundler
            bundle install
    - script@1:
        title: Build Android APK
        inputs:
        - content: |
            #!/bin/bash
            set -e
            bundle exec fastlane debug
    - deploy-to-bitrise-io@2: {}
```

## Customization

### App Configuration
Edit `fastlane/Appfile` to update:
- Package name
- App name
- Version information
- SDK versions

### Build Configuration
Edit `fastlane/Fastfile` to:
- Add custom build steps
- Modify build parameters
- Customize artifact handling

## Troubleshooting

### Common Issues

1. **Ruby version conflicts**: Ensure Bitrise uses a compatible Ruby version
2. **Missing dependencies**: Run `bundle install` before fastlane commands
3. **Permission issues**: Ensure proper file permissions for build artifacts
4. **Environment variables**: Verify all required environment variables are set

### Debug Mode

Run fastlane with verbose output:
```bash
bundle exec fastlane debug --verbose
```

## Dependencies

- Ruby 2.7+
- Bundler
- Fastlane
- Android SDK
- Node.js and npm (for Ionic/Capacitor)
