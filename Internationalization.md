# Overview

- Internationalization is a feature that lets you localize content for a culture, region or language.
- This feature is often associated with localization (l10n), which is the process of transforming content for a specific locale

## Internationalization in Adobe Commerce and Magento Open Source verus PWA Studio

- The Adobe Commerce and Magento Open Source applications include an i18n feature that provides translated text to the frontend theme.
- This feature uses dictionary files inside language packages to provide translation data for the application when it renders a page.
- The language packages themselves are extensions the application installs using Composer

- For more information [[Translations overview]]

- The tight coupling between each application's i18n feature and the frontend theme make it difficult to use the same translation mechanisms in PWA Studio storefronts.
- Instead, PWA Studio provides its own i18n feature that follows a similar design as the one in Adobe Commerce and Magento Open Source

## How it works

- PWA Studio provides a context provider for translations called the [[LocaleProvider]].
- This context provider contains translation data from dictionary files and supplies them to its child components

- The i18n feature in PWA Studio is an implementation of the [[react-intl]] library.
- The `LocaleProvider` component in PWA Studio wraps around the library's [[IntlProvider]] and provides it with translation data

- This library also provides [[formatMessage()]] and [[FormattedMessage]] to localize text in child components under `LocaleProvider`.
- You  must at least supply values for `id` and as `defaultMessage` fallback when you use either the function or component

- Example

- The i18n feature uses the `id` parameter to look up the localized text from the dictionary files which the feature supplies to the `LocaleProvider`.

## Translation dictionaries

- Translation dictionary files contain key/value pairs for localized text.
- PWA Studio's i18n feature uses a similar dictionary approach for translation files as Adobe Commerce and Mangeto Open Source, but instead of a CSV format, it uses JSON 

### Filename format

- Dictionary files must be inside an `i18n` directory and use their target locale for their filename

### ID formats 

- The JSON object's keys act as unique IDs for localized text,
- They map a placeholder string value in components to the actual rendered text

- PWA Studio recommends and uses a dot notation format in its components. 
- This format use the component name and descriptor to form the ID value

- Example

- This approach helps identify which component renders the text and provides a unique value for the ID

- However, the i18n feature in PWA Studio does not limit you to the dot notation format.
- For example, in Adobe Commerce and Magenot Open Source, the original `en_US` locale text identifies the translated text in the application

- Example

- PWA Studio's i18n feature allows you to use this notation in your own components and storefront
- Both approach have their pros and cons, and developers are free to choose which approach works for them when they develop their own components and storefronts

### Message syntax

- The i18n feature accepts the same [[message syntax]] as the underlying `react-intl` library.
- Along with static text, this syntax supports variables, dates and even conditional formatting

- To translate text with variables pass in a mapping object to the `values` prop in the `FormattedMessage` component.

- Example

- When using the `formatMessage()` function, pass in the mapping object as the second parameter.

- Example

- For more detail [[message syntax]]

## Language packages and plugins

- Language packages provide translation data for one or more locales.
- They are also used to override the text in the same locale

- Unlike the Adobe Commerce and Magento Open Source applications, which install language packages through Composer, PWA Studio storefronts install language packages as NPM dependencies

- An NPM dependency is a language package if it meets the following criteria 
	- The package contains an intercept file
	- The intercept file sets the special feature `i18n` flag to `true` for the package
	- The package contains an `i18n` directory
	- The `i18n` directory contains a dictionary file with a locale formatted name

## Build process 

- To optimize runtime performance, the i18n feature compiles all the translation data during the build process
- The following is a high level summary of the actions the i18n feature takes to compile the translation data
	- Scans all `i18n` folders within installed modules that declare `i18n` support through the `specialFeatures` flag
	- Scans the project's root directory for an `i18n` directory
	- Generates an object with locales as keys that map to an array of files matching that locale
	- Merges the files in the arrays to create a single dictionary object for a locale
		- NOTE: The dictionary files in the project itself are the last files merged and the final overrides for translations
	- Create a virtual module from this object that exposes a `__fetchLocaleData__` function 
	- Generates a dynamic import in the application for the virtual module

## Runtime process

- During runtime, the `LocaleProvider` component uses the `__fetchLocaleData__` function to get the correct translation data for the current locale

- If a components changes the value of the current locale during runtime , the application sends a GraphQL query to verify the new value.
- Event if you install a language package plugin for a locale, you must enable the locale on the backend to use the translations in the storefront UI

- For example, if you use a store switcher and you provide an `i18n/fr_FR.json` file, you must enable the French locale in the backend application to make the store switcher work