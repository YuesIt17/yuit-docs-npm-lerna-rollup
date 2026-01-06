# Project Npm package lerna + rollup + jfrog

## 📝Here is 👉 [**my article**](https://dev.to/eugene_yues_17/how-to-create-an-npm-packages-using-rollupjs-lernajs-jfrog-artifactory-2g80) about the settings of this application

#### Preparation

- before to run npx lerna commands need to set up access git through the terminal: ssh -T git@github.com

---

#### Initialization

- init project packages: yarn install
- to build packages: npx lerna run build
- to set internal dependencies (for example "@portfolio-yues-it-npm/icons": "file:../icons") to run again: yarn install

---

#### Versioning to git and Publishing to artifactory

- run build via: npx lerna run build
- then: yarn install
- make commit changes
- run versioning via: npx lerna version --no-private (--no-private is needed to publish packages other than privates
  ones)
- select the necessary version: Patch / Minor / Major (after that will be push to your git repository)
- make publish to artifactory via yarn (see below FYI): yarn workspaces foreach --all --no-private npm publish

FYI: when publishing via: npx lerna publish --no-private, sometimes there is a problem after publishing with installing
packages, i.e. with slash decoding (instead of '/' to '%2f'), because of which it doesn't find the path to the package
and there is the error:

```
YN0027: @portfolio-yues-it-npm/ui@unknown can't be resolved to a satisfying range
YN0035: The remote server failed to provide the requested resource
YN0035: Response Code: 404 (Not Found)

```

---

#### Install a npm package for a specific lerna package

- yarn workspace @portfolio-yues-it-npm/utils add date-fns

---

#### Run build, test or lint

- to run test via: npx lerna run test
- to run commands at the same time: npx lerna run test,build,lint
- to run tests or build for one package (scope):
  - npx lerna run build --scope=@portfolio-yues-it-npm/ui
  - npx lerna run test --scope=@portfolio-yues-it-npm/utils

---

#### Run the build to update packages inside workspaces

- for example, if you need to update @portfolio-yues-it-npm/utils to @portfolio-yues-it-npm/ui, then to run build: npx
  lerna run build --scope=@pportfolio-yues-it-npm/utils
- after that to run install: yarn install

---

#### To install portfolio-yues-it-npm packages into your project

Write in the .yarnrc.yml file:

- add registry setting: npmRegistryServer: "https://some-art.com/artifactory/api/npm/portfolio-yues-it-npm/"
- set a timeout if necessary: httpTimeout: 600000
- run the installation of the required package:
  - yarn add @portfolio-yues-it-npm/ui
  - yarn add @portfolio-yues-it-npm/utils
  - yarn add @portfolio-yues-it-npm/icons
- to check how the package works in your project, you can enter the package name to package.json:
  "@portfolio-yues-it-npm/ui": "file:../portfolio-yues-it-npm/packages/ui" and then yarn install

---

#### Run storybook

- yarn storybook
