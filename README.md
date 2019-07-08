# Setup Jest for testing your backend project

## Step 1:
### For vanilla Nodejs project

```
yarn add --dev jest
```
Or:
```
npm install --save-dev jest
```

### For ES6 (Babel) project

#### For brand-new project

You can run the command:

```
yarn create jest-babel-tdd your-app-name --babel
```

For more detail please visit: https://github.com/fastcopypaste/create-jest-babel-tdd

#### For existing project

```
yarn add --dev babel-jest @babel/core @babel/preset-env
```

Config your `babel.config.js` like this:
```
// babel.config.js
module.exports = {
  presets: [
    [
      '@babel/preset-env',
      {
        targets: {
          node: 'current',
        },
      },
    ],
  ],
};
```
We need this line: `targets: { node: 'current', }` to target `babel` for your current Nodejs version.

## Step 2:

Add the test script below to your `package.json` file:

For running in your local computer:

```
"scripts": {
  "test": "jest --detectOpenHandles"
}
```
From the Jest docs:

>detectOpenHandles: Attempt to collect and print open handles preventing Jest from exiting cleanly.
This implies --runInBand, making tests run serially
This option has a significant performance penalty and should only be used for debugging.

So for debug local we can use the flag `detectOpenHandles`, but for running CI/CD, I suggest that you should remove the option because it causes Jest run serially that is slow.

For running in CI/CD:

```
"scripts": {
  "test": "jest"
}
```


## Step 3:
Make a config file for Jest using the command:
```
jest --init
```
You can see a lot of things, just read it or you can use my config, it is good enough for every project:

```
// File: jest.config.js

module.exports = {
  clearMocks: true,
  verbose: true,
  coverageDirectory: 'coverage',
  testEnvironment: 'node',
  collectCoverageFrom: [
    'src/**/*.js',
    '!<rootDir>/src/lib/**',
  ],
  collectCoverage: true,
  coverageReporters: ['html', 'text', 'lcov'],
  testMatch: ['<rootDir>/tests/**/*.test.js'],
  coverageThreshold: {
    global: {
      functions: 100,
      branches: 100,
      lines: 100,
      statements: 100,
    },
  },
};
```
Full configuration  can be found here: https://jestjs.io/docs/en/configuration

Explain for some options:

`collectCoverageFrom`: Here is all files that we need collect coverage report, we can use `!` to exclude files or folders we do not want to collect coverage from them.

`collectCoverage`: Show coverage report

`coverageDirectory`: Folder that you wan to see Jest's report

`coverageReporters`: Type of coverage report

`testMatch`: Pattern your test files, modify it if you need

`verbose`: `true` if you want to see every test cases or `false` if you do not want to see them, default is `false`

`coverageThreshold`: coverage threshold for your project, modify it as  you need

## Additional information
1. Coverage

You can ignore coverage for line or function using this:
```
/* istanbul ignore next: I dont want to cover this line */
```

2. Eslint

Maybe you will encounter Eslint problems in Visual Code or you want to add Eslint For Jest. Let run this command:

```
yarn add --dev eslint-plugin-jest
```
Add the plugin to your `.eslintrc` configuration file.

```
{
  "plugins": ["jest"]
}
```
And we can whitelist the environment variable

```
{
  "env": {
    "jest/globals": true
  }
}
```

Maybe Your `eslintrc` is look like this:
```
{
  "extends": "airbnb-base",
  "parser": "babel-eslint",
  "root": true,
  "plugins": [
    "babel",
    "jest"
  ],
  "env": {
    "node": true,
    "mocha": true,
    "jest/globals": true
  },
  "rules": {
    "babel/generator-star-spacing": 1,
    "func-names": 0,
    "generator-star-spacing": 0,
    "id-length": 0,
    "import/no-extraneous-dependencies": [
      "error",
      {
        "devDependencies": [ "**/*.test.js" ]
      }
    ],
    "import/prefer-default-export": 0,
    "require-jsdoc": 2
  }
}
```

### Happy testing with Jest
