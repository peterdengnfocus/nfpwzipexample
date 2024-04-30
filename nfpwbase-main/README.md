# nFocus Playwright base configuration project
A repo to decide on informal standards for new Playwright projects created by nFocus SDETs


## Using this Repo Template
### Using GitHub
1. Open this repo on [Github](http://www.github.com/nfocustesting/nfpwbase)
2. Click the green "Use this template" button in the top right hand corner of the screen
   - If you don't see this button, you need to log into GitHub
3. Select "Create a new repository"
4. Select **Owner**
5. Enter new repository name
6. Select 'Public' or 'Private'
7. Click 'Create repository'
8. Clone repo to local machine
9. Open repo in VSCode
10. Run `npm install`


### Using alternative Git repository

1. Open this repo on [Github](http://www.github.com/nfocustesting/nfpwbase)
2. Click on '<> Code'
3. Click 'Download ZIP'
4. Unzip the project
5. Create a new repo with your host of choice
6. Clone new repo to your local machine
7. Copy files from downloaded project folder into cloned directory
   - Do not copy .git folder if it is visible
8. Commit new files to repo and push back to server
9. Run `npm install`
10. Close VSCode and reopen to apply new settings

## Install into existing project
\*Only do this with permission from the team leader.\*
### npm modules
Run the following command to install the required npm modules:
```bash
npm install -D @playwright/test @types/node @eslint/js@8.57.0 eslint@8.57.0 typescript@5.4.5 typescript-eslint dotenv globals
```

Typescript-ESLint currently requires specific npm package versions:
   - eslint - 8.57.0
   - @eslint/js - 8.57.0
   - typescript - 5.4.5

### Files

Download the following files and ***merge*** them into the project.
- [`eslint.config.js`](#eslintconfigjs)
- [`.prettier.config.js`](#prettierconfig)
- [`playwright.config.js`](#playwrightconfig)
- [`tsconfig.json`](#tsconfig)

Inside the package.json file add:
```json
  "type": "module",
```
to the root of the JSON object.

You will need to restart VSCode for all the changes to be made to the editor.

## VSCode extensions
These extensions are required to allow VSCode to interact with the npm modules installed.
- [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
  - Typescript-ESLint requires the pre-release version (v3.0.5) of this extension
- [Playwright](https://marketplace.visualstudio.com/items?itemName=ms-playwright.playwright)
  - Recommend turning off the 'Show browser' option in the Playwright panel in the 'Testing' tab
- [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)


## Files

- [`eslint.config.js`](#eslintconfigjs) - ESLint config
- [`.prettier.config.js`](#prettierconfig) - Prettier config
- [`playwright.config.js`](#playwrightconfig) - Playwright config
- [`tsconfig.json`](#tsconfig) - Typescript config

---

### <a name="#eslintconfig"></a>`eslint.config.js`
Uses the `typescript-eslint` package to allow `ESLint` to lint Typescript files. 
#### [Rules Documentation](https://typescript-eslint.io/rules)
Use the link above to find new rules.\
Use `.prettier.config.js` for stylistic rules.\
The following rules have been implemented.

- `"@typescript-eslint/restrict-template-expressions": "off"`
- `"@typescript-eslint/no-base-to-string": ["warn", { ignoredTypeNames: ["Locator", "Date"] }]`

---
### <a name="#prettierconfig"></a>`.prettier.config.js`
Used by the Prettier VSCode plugin to have consistant styling across all js/ts files.

#### [Rules Documentation](https://prettier.io/docs/en/options)
The following rules have been implemented.

- `tabWidth: 2`
- `trailingComma: "es5"`
- `experimentalTernaries: true`

---
### <a name="#playwrightconfig"></a>`playwright.config.ts`
[Documentation](https://playwright.dev/docs/test-configuration)

---
### <a name="#tsconfig"></a>`tsconfig.json`
[Documentation](https://www.typescriptlang.org/tsconfig)

#### `baseUrl`
Sets the base directory to resolve non-relative module names.\
Defaults to `"."` (root folder).\
DO NOT CHANGE UNLESS YOU KNOW WHAT YOU ARE DOING!!!

#### `paths`
This allows you to simplify your imports in a `.ts` file, so instead of 
```js
import Homepage from '../../pages/Homepage'
```
you can write
```js
import Homepage from '@pages/Homepage'
```

## Folder Structure 

### `components`
Somewhere to store Component Object Models (COMs)
### `enums`
Somewhere to store Enumerated objects (enums)
### `fixtures`
Somewhere to store fixtures
### `models`
Somewhere to store data models, interfaces, types
### `pages`
Somewhere to store Page Object Models (POMs)
### `utils`
Somewhere to store extra files such as helper functions

---
### `index.ts`
Say you have the following pages folder structure:
```
pages
 ┣ Account.ts
 ┣ AccountOrders.ts
 ...
```
Normally you'd have to import `Account.ts` and `AccountOrders.ts` files (using path alias syntax) as below:
```js
import Account from '@pages/Account.ts'
import AccountOrders from '@pages/AccountOrders.ts'
```

If you create an `index.ts` inside the `pages` folder with the following:
```ts
export { default as Account } from "./Account"; // If default export
export { AccountOrders } from "./AccountOrders"; // If named export
...
```
You could change tsconfig.json from
```json
"@pages/*": ["pages/*"],
```
to
```json
"@pages": ["pages/index"],
```
and then import `Account.ts` and `AccountOrders.ts` using
```ts
import { Account, AccountOrders } from '@pages'
```