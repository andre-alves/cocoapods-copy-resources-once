# cocoapods-copy-resources-once

The build phase for copying resources from pods can be very slow if your project or dependencies have many resources.

This patch attempts to improve the build phase by running it only when needed and not in every build.

*Tested with cocoapods 1.2.0*

## Installation

1. Place **copy_pod_resources_once.patch** in your project's root directory.
2. Add the following code to your **Podfile**:

```ruby
post_install do | installer |
    system('find "./Pods/Target Support Files" -name "*-resources.sh" | xargs -I{} patch -p0 {} -i ./copy_pod_resources_once.patch && find "./Pods/Target Support Files" -name "*-resources.sh" -exec sh -c "echo \"create_nonce_file\" >> \"{}\"" \;')
end
```
3. Run `pod install`.
