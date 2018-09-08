# Application Architecture Guide
#### This guide explains the underlying structure of a Fitbit application, and how the various elements are related.

## Folder Structure
A new application typically begins with the following folder structure:

```
/app/
/common/
/companion/
/resources/
/settings/
```

This folder structure allows the build process to correctly compile and bundle the application for installation on the device, and build the companion and settings for installation on the mobile device (if required).

> The `/common/`, `/companion/`, and `/settings/` folders are optional.

## Size
The maximum size of an application at installation time is `10` megabytes. The total file system space used by an installed is limited to `15` megabytes, including any resources, and files written using the File System API.

## JavaScript
The `/app/`, `/common/` and `/companion/` folders can contain multiple JavaScript (.js) or TypeScript (.ts) files.

During the build process, the scripts are automatically compiled, bundled and optimized by the [TypeScript](https://www.typescriptlang.org/) compiler and [rollup.js](https://rollupjs.org/guide/en). This produces a single ECMAScript 5.1 file for the application, and another file for the companion.

JavaScript is run on the device using the [JerryScript engine](http://jerryscript.net/).

The `/settings/` folder should contain a single [React JSX](https://reactjs.org/docs/introducing-jsx.html) file, named `index.jsx`.

## Project Folders
### Application
**/app/**

This folder contains the application logic which executes on the device. Code in this folder has access to the [Device API](https://dev.fitbit.com/build/reference/device-api/) and is capable of interacting directly with the presentation layer, communicating with the companion, or reading and writing settings.

An `index.js` or `index.ts` file must exist in this folder, and must not be empty, or the build process will fail.

### Companion
**/companion/**

This folder contains the companion logic which executes on the mobile device. Code in this folder has access to the [Companion API](https://dev.fitbit.com/build/reference/companion-api/) and is capable of making direct requests to the internet, and communicating with the application.

If an `index.js` or `index.ts` file exist in this folder, the companion will be built.

### Shared Code
**/common/**

Files within this folder can be shared between the application and companion to minimize duplication. Create each file as an [ES6 module](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import), then import that module into your application or companion.

The build process will automatically perform [tree shaking](https://rollupjs.org/guide/en) to exclude any unused modules from the final output.

### Resources
**/resources/**

This folder contains all of the resources which are bundled with the application during the build process.

**/resources/index.gui**

This is the main Fitbit SVG file where the application user interface markup is defined. This is a mandatory file.

**/resources/widgets.gui**

This Fitbit SVG file controls which system widgets are available for use within the `index.gui` file. This is a mandatory file.

**/resources/*.css**

Fitbit CSS files can be included in the application by creating a `<link>` in the `index.gui` file.

**/resources/*.png** and **/resources/*.jpg**

All image files which are included in the resources folder can be used in the presentation layer by referencing them within an `<image>` element in the `index.gui`.

### Settings
**/settings/**

This folder contains the declaration for application settings, written using React JSX. This can be used to make an app configurable by the user. Code within this file has access to the [Settings API](https://dev.fitbit.com/build/reference/settings-api/).
