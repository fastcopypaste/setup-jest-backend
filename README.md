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

#### For existed project

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

### Happy testing with Jest
