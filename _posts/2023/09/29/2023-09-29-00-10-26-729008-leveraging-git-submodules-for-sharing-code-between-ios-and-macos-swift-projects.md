---
layout: post
title: "Leveraging Git submodules for sharing code between iOS and macOS Swift projects"
description: " "
date: 2023-09-29
tags: [submodules]
comments: true
share: true
---

As a developer, you may often encounter scenarios where you need to share code between different projects or platforms. This is especially true when working on iOS and macOS development, where the codebase can largely overlap. Git submodules offer a powerful solution for managing shared code repositories and keeping them in sync across different projects.

## What are Git submodules?

Git submodules allow you to include one Git repository (referred to as the submodule) as a subdirectory within another repository (referred to as the parent repository). With submodules, you can track and manage changes to the submodule independently from the parent repository. This enables code sharing without duplicating the codebase across projects.

## Setting up a submodule

To set up a Git submodule, you'll first need to create a submodule repository. Assuming you already have a repository for your shared code, you can follow these steps:

1. Navigate to the parent repository's root directory in your terminal.
2. Execute the following command to add the submodule:
   ```bash
   git submodule add <submodule-repository-url> <destination-path>
   ```
   Replace `<submodule-repository-url>` with the URL of the submodule repository, and `<destination-path>` with the directory path where you want the submodule to be added within the parent repository.

## Updating a submodule

Once you have added a submodule to your parent repository, it is essential to understand how to update it as changes are made in the upstream submodule repository. To update the submodule:

1. Navigate to the submodule's directory within the parent repository.
2. Use the following commands to fetch the latest changes and update the submodule to the latest commit:
   ```bash
   git fetch
   git checkout <desired-branch>
   git pull origin <desired-branch>
   ```

Make sure to replace `<desired-branch>` with the branch you want to update to.

## Using submodules in Xcode projects

When working with iOS and macOS projects in Xcode, you need to add the submodule to your project workspace to use the shared code. Follow these steps:

1. Open your Xcode workspace.
2. Drag and drop the submodule's `.xcodeproj` file into your Xcode project's file navigator panel.
3. In the Xcode project settings, navigate to the "Build Phases" tab for your target.
4. Under "Link Binary With Libraries," click the "+" button and add the submodule's library.
5. In your source code files, import the necessary modules from the submodule and use them as needed.

With the shared code now accessible in your Xcode project, any changes made to the submodule can be pulled and integrated into your Xcode project using the submodule update steps mentioned earlier.

## Conclusion

Git submodules provide a powerful mechanism for sharing code between different projects or platforms, such as iOS and macOS Swift projects. By effectively managing submodules, you can avoid code duplication and ensure consistency across your codebases. With the steps mentioned above, you can leverage Git submodules to streamline code sharing and improve development efficiency. #git #submodules