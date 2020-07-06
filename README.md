# 1. Install Jest Builder

## 1.1. Remove Karma

```sh
npm remove karma karma-chrome-launcher karma-coverage-istanbul-reporter karma-jasmine karma-jasmine-html-reporter

rm ./karma.conf.js
rm ./src/test.ts
```

## 1.2. Install Jest Builder

```sh
npm install --save-dev --save-exact jest @types/jest @angular-builders/jest
```

### 1.2.1. Edit tsconfig.base.json

- Add `esModuleInterop`.

```json
{
  "compilerOptions": {
    // ....
    "esModuleInterop": true
    // ...
  }
}
```

### 1.2.2. Edit tsconfig.spec.json

- Replace `jasmine` in `types` array with `jest`.
- Remove `test.ts` entry from `files` array.

```json
{
  "extends": "./tsconfig.base.json",
  "compilerOptions": {
    "outDir": "./out-tsc/spec",
    "types": ["jest"]
  },
  "files": ["src/polyfills.ts"],
  "include": ["src/**/*.spec.ts", "src/**/*.d.ts"]
}
```

### 1.2.3. Edit tsconfig.app.json

- Add `jest` to `types` array.

```json
{
  "extends": "./tsconfig.base.json",
  "compilerOptions": {
    "outDir": "./out-tsc/app",
    "types": ["jest"]
  },
  "files": ["src/main.ts", "src/polyfills.ts"],
  "include": ["src/**/*.d.ts"]
}
```

### 1.2.4. Edit angular.json

```json
"architect": {
    // ...
    "test": {
        "builder": "@angular-builders/jest:run",
        "options": {}
    }
}
```

### 1.2.5. Run the tests:

```sh
ng test
```

### 1.2.6. Debug Jest Tests

Edit .vscode/launch.json

```json
{
    "name": "Debug Jest Tests",
    "type": "node",
    "request": "launch",
    "runtimeArgs": [
      "--inspect-brk",
      "${workspaceRoot}/node_modules/@angular/cli/bin/ng",
      "test",
      "--runInBand",
      "--no-cache",
      "--env=jsdom"
    ],
    "console": "integratedTerminal",
    "internalConsoleOptions": "neverOpen",
    "port": 9229
},
```

# 2. Configure vscode-jest extension

## 2.1. Edit .vscode/settings.json

```json
{
  "jest.pathToJest": "ng test",
  "jest.enableInlineErrorMessages": true
}
```

## 2.2. Create jest.config.js

```js
module.exports = {
  testMatch: ["<rootDir>/src/app/**/*.spec.ts"],
};
```

## 2.3. Edit .vscode/launch.json

```json
{
  "name": "vscode-jest-tests",
  "type": "node",
  "request": "launch",
  "program": "${workspaceFolder}/node_modules/@angular/cli/bin/ng",
  "args": ["test", "--runInBand", "--testPathPattern"],
  "console": "integratedTerminal",
  "internalConsoleOptions": "neverOpen"
}
```

# References

- [Jest Builder](https://github.com/just-jeb/angular-builders/tree/master/packages/jest)
- [jest-preset-angular](https://github.com/thymikee/jest-preset-angular)
- [vscode-jest](https://github.com/jest-community/vscode-jest)
- [Jest GitHub](https://github.com/facebook/jest)
- [Jest](https://jestjs.io/)
