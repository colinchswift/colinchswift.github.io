---
layout: post
title: "Internationalization in Swift Playgrounds"
description: " "
date: 2023-09-26
tags: [internationalization]
comments: true
share: true
---

Internationalization is the process of designing and developing software applications that can be adapted to different languages and regions. It allows you to create applications that can be easily localized to cater to a wider audience.

In this article, we will explore how to implement internationalization in Swift Playgrounds, making our code ready for localization.

## Step 1: Enable Internationalization

To begin, open your Swift Playground project. In the Project navigator, select the project file, then go to the **Info** tab. Scroll down to the **Localizations** section and click on the "+" button to add a new localization.

## Step 2: Add Localization Files

After adding a new localization, Xcode will prompt you to create localization files for the added languages. Accept the prompt, and Xcode will generate a `<Localization>.lproj` folder for each language you added.

## Step 3: Identify Localizable Strings

Identify the strings in your code that need to be localized. Wrap these strings with the `NSLocalizedString` function. For example:

```swift
let greeting = NSLocalizedString("Hello", comment: "Greeting")
```

The first parameter is the key used to look up the localized value, while the second parameter is a comment used to provide more context for translators.

## Step 4: Generate Localization Files

After adding localized strings to your code, go to **Editor ➞ Export For Localization**. This will generate a `.xliff` file containing all the localizable strings. Send this file to your translation team for localization.

## Step 5: Add Localized Strings

Once you receive the localized strings from your translation team, add them to the relevant `<Localization>.lproj` folders generated in Step 2. Open each `.strings` file and add the translated values. Remember to keep the keys the same as in your original code.

## Step 6: Test Localization

To test the localization, go to the Project settings again and add a new scheme by duplicating the existing scheme. Edit the duplicated scheme and set the **Application Language** to the desired language for testing. Now, when you run the playground, it will be localized to the selected language.

## Conclusion

With internationalization, you can make your Swift Playgrounds accessible to a global audience by supporting multiple languages and cultures. Follow the steps outlined in this article to enable internationalization and localization in your Swift Playground projects.

#swift #internationalization