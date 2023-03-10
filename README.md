# node-typescript-api


## SETUP
### DEPENDENCIES
```powershell
npm install typescript -D
npm install @types/node -D
npm i module-alias
npm i @types/module-alias -D
npm i eslint -D
npm i @typescript-eslint/eslint-plugin -D
npm i @typescript-eslint/parser -D
npm i ts-node-dev -D
npm i jest -D
npm i ts-jest -D
npm i @types/jest -D
npm i sueprtest -D
npm i @types/sueprtest -D
```
### COMMANDS
```powershell
tsc.cmd --init # Init TypeScript
```

### ALIASES
`src/util/module-alias.ts`
```ts
import * as path from "path";
import moduleAlias from "module-alias";
const files = path.resolve(__dirname, "../../");
moduleAlias.addAliases({
    "@src": path.join(files, "src"),
    "@test": path.join(files, "test")
});
```

### ESLINT
`.eslintrc`
```json
{
    "root": true,
    "parser": "@typescript-eslint/parser",
    "plugins": [
        "@typescript-eslint"
    ],
    "extends": [
        "eslint:recommended",
        "plugin:@typescript-eslint/eslint-recommended",
        "plugin:@typescript-eslint/recommended"
    ]
}
```


### JEST
`.jest.config.js`
```js
const {resolve} = require("path");
const rootDir = resolve(__dirname);
module.exports = {
    rootDir,
    displayName: "root-tests",
    testMatch: ["<rootDir>/src/**/*.test.ts"],
    testEnvironment: "node",
    clearMocks: true,
    preset: "ts-jest",
    moduleNameMapper: {
        "@src/(.*)": "<rootDir>/src/$1",
        "@test/(.*)": "<rootDir>/test/$1"
    }
}
```
`/test/jest.config.js`
```js
const { resolve } = require("path");
const rootDir = resolve(__dirname, "..");
const rootConfig = require(`${rootDir}/jest.config.js`)
module.exports = {
    ...rootConfig,
    rootDir,
    displayName: "end2end-tests",
    setupFilesAfterEnv: ["<rootDir>/test/jest-setup.ts"],
    testMatch: ["<rootDir>/test/**/*.test.ts"]
} 
```

### SCRIPTS
`package.json`
```json
{
    // ...
    "scripts": {
        // ...
        "build": "tsc",
        "start": "npm run build && node dist/src/index.js",
        "start:dev": "ts-node-dev 'src/index.ts'",
        "lint": "eslint ./src ./test --ext .ts",
        "lint:fix": "eslint ./src ./test --ext .ts --fix",
        "test:functional": "jest --projects ./test --runInBand",
    }
}
```