---
layout: post
title: "Setting up code signing and provisioning profiles with Git for Swift projects"
description: " "
date: 2023-09-28
tags: [Swift]
comments: true
share: true
---

When working on Swift projects, it is essential to have proper code signing and provisioning profiles set up, especially when collaborating with teammates using Git. This ensures that your app can be properly signed and provisioned for testing and distribution. In this blog post, we will guide you through the process of setting up code signing and provisioning profiles with Git for Swift projects.

## Step 1: Generate Signing Certificates and Provisioning Profiles

Before setting up code signing and provisioning profiles, you need to generate the necessary certificates and profiles in your Apple Developer account. Here's how:

1. **Certificates:**

   a. Open the [Apple Developer Portal](https://developer.apple.com/account/) and navigate to "Certificates, IDs & Profiles."
   
   b. Under "Certificates," click on "All" or "+", and follow the instructions to create a new certificate signing request (CSR).

   c. Download the generated certificate file (`.cer`) and double-click to install it in your Keychain Access.

   d. Back in the Developer Portal, download the corresponding private key (`.p12`) associated with the certificate. Double-click to install it in Keychain Access.

   e. Now you have the necessary certificates installed in your Keychain Access.

2. **Provisioning Profiles:**

   a. In the Developer Portal, navigate to "Provisioning Profiles" under "Certificates, IDs & Profiles."

   b. Click on "+," and select the appropriate profile type (e.g., iOS App Development, App Store Distribution, etc.).

   c. Follow the instructions to select the relevant App ID and associate it with the appropriate certificates.

   d. Download the generated provisioning profile(s) with the appropriate bundle identifier and provisioning method.

## Step 2: Create a `.gitignore` File

To avoid committing sensitive information related to code signing and provisioning profiles to your Git repository, it is crucial to create a `.gitignore` file. This file specifies the patterns of files and directories that Git should ignore. Here's an example of what your `.gitignore` file may look like:

```git
# Ignore provisioning profiles
*.mobileprovision

# Ignore code signing files
*.p12
*.cer

# Ignore Xcode generated files
*.xcworkspace
*/xcuserdata/
*.xcodeproj/project.xcworkspace

# Ignore user-specific Xcode settings
*.xcuserstate
```

By adding these patterns to your `.gitignore` file, you can prevent accidentally committing your code signing and provisioning profile files.

## Step 3: Setting Up Git Hooks

To automate the code signing and provisioning profile setup process when switching branches or pulling changes from Git, you can utilize Git hooks. Git hooks are scripts that run automatically when specific events occur in a Git repository.

1. **Pre-Checkout Hook:**

   Create a pre-checkout hook script that copies the appropriate code signing and provisioning profile files for the branch being checked out. Here's an example of what your pre-checkout hook script (`pre-checkout`) may look like:

   ```bash
   #!/bin/bash

   BRANCH_NAME=$(git rev-parse --abbrev-ref HEAD)

   case "$BRANCH_NAME" in
       "develop")
           cp ~/Certificates/development.mobileprovision .
           cp ~/Certificates/development.p12 .
           cp ~/Certificates/development.cer .
           ;;
       "release")
           cp ~/Certificates/distribution.mobileprovision .
           cp ~/Certificates/distribution.p12 .
           cp ~/Certificates/distribution.cer .
           ;;
   esac
   ```

   Make sure to update the script with the correct file paths and naming conventions for your certificates and provisioning profiles.

2. **Post-Merge and Post-Checkout Hooks:**

   Create post-merge and post-checkout hook scripts that ensure the code signing and provisioning profile files are not accidentally committed to the repository. Here's an example of what your post-merge and post-checkout hook scripts (`post-merge` and `post-checkout`) may look like:

   ```bash
   #!/bin/bash

   git update-index --assume-unchanged *.mobileprovision
   git update-index --assume-unchanged *.p12
   git update-index --assume-unchanged *.cer
   ```

   These scripts use the `git update-index --assume-unchanged` command to mark the code signing and provisioning profile files as unchanged, preventing them from being committed.

   **Note:** The post-merge hook is necessary to handle cases where others may have accidentally committed the code signing files.

Make sure to make the hook scripts executable by running `chmod +x pre-checkout post-merge post-checkout`.

## Conclusion

By following these steps, you can properly set up code signing and provisioning profiles with Git for your Swift projects. This ensures that your app's signing and provisioning information can be securely shared among team members while keeping sensitive files out of version control. Proper code signing and provisioning profiles are vital for smooth testing and distribution of your Swift apps. #Swift #Git