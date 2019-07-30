# NgJestBuilder

## 1. Create new Angular App

```sh
ng new ng-app-a --style=css --routing
```

## 2. Install Prettier

```sh
npm install --save-dev --save-exact prettier
```

### 2.1. Edit package.json

```json
"scripts": {
  // ...
  "prettier": "prettier --write \"**/*.{js,json,css,scss,less,md,ts,html,component.html}\"",
  // ...
},
```

### 2.2. Add prettier.config.js

```js
module.exports = {
  printWidth: 80,
  tabWidth: 2,
  useTabs: false,
  semi: true,
  singleQuote: true,
  trailingComma: 'es5',
  bracketSpacing: true,
  jsxBracketSameLine: false,
  arrowParens: 'always',
  rangeStart: 0,
  rangeEnd: Infinity,
  requirePragma: false,
  insertPragma: false,
  proseWrap: 'preserve',
};
```

### 2.3. Add .prettierignore

```
package.json
dist
```

## 3. Install husky & lint-staged

### 3.1. Edit package.json

```json
{
  "devDependencies": {
    // ...
    "husky": "3.0.1",
    // ...
    "lint-staged": "9.2.1"
    // ...
  },
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "*.{js,json,css,scss,less,md,ts,html,component.html}": [
      "prettier --write",
      "git add"
    ]
  }
}
```

### 3.2. Install

```sh
npm install
```

## 4. Add Launch Chrome configuration

### 4.1 Edit launch.json

```json
"configurations": [
  {
    "type": "chrome",
    "request": "launch",
    "name": "Launch Chrome",
    "url": "http://localhost:4200",
    "webRoot": "${workspaceFolder}",

    // Set port to avoid 'Cannot connect to runtime process, timeout after 10000 ms' error
    "port": 4000
    // "trace": "verbose",
  }
]
```

## 5. Install Jest Builder

### 5.1. Remove Karma

```sh
npm remove karma karma-chrome-launcher karma-coverage-istanbul-reporter karma-jasmine karma-jasmine-html-reporter

rm ./karma.conf.js
rm ./src/test.ts
```

### 5.2 Install Jest Builder

```sh
npm install --save-dev --save-exact jest@24.8.0 @types/jest@24.0.15 @angular-builders/jest@8.0.4
```

### 5.3 Edit tsconfig.spec.json

Remove `"types"` and `test.ts`.

```json
{
  "extends": "./tsconfig.json",
  "compilerOptions": {
    "outDir": "./out-tsc/spec"
  },
  "files": ["src/polyfills.ts"],
  "include": ["src/**/*.spec.ts", "src/**/*.d.ts"]
}
```

### 5.4 Edit tsconfig.json

Add `"types": ["jest"],`.

```json
{
  "compilerOptions": {
    "baseUrl": "./",
    "outDir": "./dist/out-tsc",
    "sourceMap": true,
    "declaration": false,
    "downlevelIteration": true,
    "experimentalDecorators": true,
    "module": "esnext",
    "moduleResolution": "node",
    "importHelpers": true,
    "target": "es2015",
    "typeRoots": ["node_modules/@types"],
    "types": ["jest"],
    "lib": ["es2018", "dom"]
  }
}
```

### 5.5 Edit angular.json

```json
"architect": {
    // ...
    "test": {
        "builder": "@angular-builders/jest:run",
        "options": {}
    }
}
```

### 5.6 Run the tests:

```sh
ng test
```

### 5.7 Edit launch.json

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

## 6. Configure vscode-jest extension

### 6.1 Edit settings.json

```json
{
  "jest.pathToJest": "ng test ng-jest-builder"
}
```

### 6.2. Create jest.config.js

```js
module.exports = {
  testMatch: ['<rootDir>/src/app/**/*.spec.ts'],
};
```

### 6.3. Edit launch.json

```json
{
  "type": "node",
  "request": "launch",
  "name": "vscode-jest-tests",
  "program": "${workspaceFolder}/node_modules/@angular/cli/bin/ng",
  "args": ["test", "ng-jest-builder", "--runInBand", "--testPathPattern"],
  "console": "integratedTerminal",
  "internalConsoleOptions": "neverOpen"
}
```

## References

- https://github.com/prettier/prettier
- https://github.com/meltedspark/angular-builders/tree/master/packages/jest
- https://github.com/typicode/husky
- https://github.com/okonet/lint-staged

## VS Code Extensions

- [Debugger for Chrome](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-chrome)
- [Jest](https://marketplace.visualstudio.com/items?itemName=Orta.vscode-jest)
- [Prettier - Code formatter](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
