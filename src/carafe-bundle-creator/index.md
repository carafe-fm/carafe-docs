# Introduction

Carafe Bundle Creator is an NPM initializer which you can use to bootstrap a new Carafe Bundle project. The initializer will bootstrap a new bundle project using a starter template and include Carafe Bundler as a devDependency.

# Prerequisites

You will need to have [Node.js installed](https://nodejs.org/en/download/) to use this utility via Node Package Manager (NPM).

# Create A New Bundle

Start a brand new Bundle project in your preferred editor or IDE

```bash
npm init @carafe-fm/bundle my-new-bundle
cd my-new-bundle
```

This command will create a new `my-new-bundle` directory containing a starter project. You need to cd into the newly created Bundle directory to be in the proper context to run the included Carafe Bundler utility scripts. Open the README for detailed instructions on how to work with Carafe Bundler in your local development environment.


# Work On An Existing Bundle

You can also use the Carafe Bundle Creator to customize or configuring any existing Bundle in your preferred editor or IDE.

To work on an existing Bundle, you should first initialize a new project just like above, but with an appropriate project name for the existing bundle you want to work on.

```bash
npm init @carafe-fm/bundle my-existing-bundle
cd my-existing-bundle
```

After you've cd'd into it, and then import the Bundle. This extracts the bundle into source files to make it easier to work on.

```bash
npm run import path/to/existing-bundle.json
```

After you've imported a bundle into a Carafe Bundle Creator project, you can work on it as if you had the original source files.