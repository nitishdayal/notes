# Angular 2: Guide

## Setup For Local Development

Angular 2 provides a [Live-Coding Example](http://plnkr.co/edit/?p=preview&open=app%2Fapp.component.ts)
  hosted on Plunker, which is fine and well for tinkering and toying; no setup is pretty sweet.
  But if your interest is in _learning_ Angular 2, not just fiddling about for 5 minutes, set up
  a local development environment. There's no reason not to. And it's **REALLY
  EASY.**



## Prerequisites

- Install Node - [Link](https://nodejs.org/en/)
- Confirm Node & NPM install:
  - From command line, run the following commands:
    * `node -v` (should be v4.x.x or higher)
    * `npm -v` (should be v3.x.x or higher)



## Directions

1. Create a folder for your project and navigate to it in your terminal.
2. From command line: `git clone https://github.com/angular/quickstart.git`
3. From command line: `rm-rf .git` (non-Windows) `rd .git /S/Q` (windows)  - Remove existing
    Git repo information.
4. From command line: `npm install` - Installs all packages listed in the package.json from QuickStart
5. From command line: `npm start` - Starts the dev server and launch application (**KEEP THIS RUNNING**)

The QuickStart tree structure is as follows:
(An asterisk (*) implies the file/folder is not necessary)
```bash
└── quickstart                                         # Root Project Directory
    ├── CHANGELOG.md*                                  # Angular 2 QuickStart Change log
    ├── LICENSE                                        # MIT License Information
    ├── README.md*                                     # README containing everything I will say here
    ├── app                                            # Roop Application Folder
    │   ├── app.component.spec.ts*                         # Component Unit Test
    │   ├── app.component.ts                               # Root Application Component
    │   ├── app.module.ts                                  # Root Application Module 
    │   └── main.ts                                        # Application starting point (bootstrap)
    ├── e2e*                                           # End-to-End Testing
    │   └── app.e2e-spec.ts                                # Bare-bones E2E test for sample app
    ├── favicon.ico                                    # Icon to be displayed when 'Favorited'
    ├── index.html                                     # Loading point of the application
    ├── karma-test-shim.js*                            # Script to run karma w/ SystemJS
    ├── karma.conf.js*                                 # Karma test runner configuration
    ├── node_modules/                                  # Installed node modules
    ├── package.json                                   # Info about the application, scripts, dependencies, etc.
    ├── protractor.config.js*                          # Protractor (e2e test runner) configuration
    ├── styles.css*                                    # Global styles for the application
    ├── systemjs.config.extras.js                      # Optional SystemJS configuration file for adding custom mappings
    ├── systemjs.config.js                             # SystemJS configuration settings
    ├── tsconfig.json                                  # Instructions for TypeScript compiler
    └── tslint.json*                                   # Linting rules favored by the Angular style guide - https://angular.io/docs/ts/latest/guide/style-guide.html
```
Feel free to delete the files marked with an asterisk; if you don't intend on learning about
  Angular 2 testing currently, having some of these additional files creates more clutter and can 
  add to the 'overwhelming' factor that some people feel about Angular 2. My opinion: Leave them.
  **Leave comments on the top of each file to remind yourself what it's there for** so you can
  come back to it when you're ready. They're there for a reason.

But, once again, if your goal is to learn the core concepts of Angular 2, they are 
  not necessary. All you really need is the `app/` directory sans the spec.ts file, 
  `index.html`, `node_modules/` directory (which will appear after running npm install), `package.json`,
  and the configuration files for SystemJS, the TypeScript compiler, and TSLint.

