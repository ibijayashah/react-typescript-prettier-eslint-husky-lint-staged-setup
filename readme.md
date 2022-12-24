# Set up prettier, eslint, husky (pre commit hook), and lint-staged on react + typescript project.

## Table of Contents

1. [Create react project](#create_react_project)
2. [Setup Prettier](#setup_prettier)
3. [Setup Eslint](#setup_eslint)
4. [Setup Husky](#setup_husky)
5. [Test](#test)

## Step 1 :: Create react project[](#create_react_project)

#### Create a new react app using pnpm and vite

```
$ pnpm create vite@latest my-react-app --template typescript
```

> Note: In this project i am using react typescript, vite (for rapid development).

## Step 2 :: Setup prettier[](#setup_prettier)

#### 2.1 install prettier as dev dependency [optional]

```
$ pnpm i --save-d save-exact prettier
```

#### 2.2 create .prettierrc file on root directory ad paste the following code

> Note: You can change the setting format or make your own.

```
{
  "trailingComma": "es5",
  "semi": false,
  "singleQuote": true,
  "bracketSpacing": true,
  "printWidth": 120,
  "tabWidth": 2,
  "jsxSingleQuote": true,
  "arrowParens": "always"
}
```

#### 2.3 Execute the command below to confirm the proper setup of .prettierrc

```
$ npx prettier --check .
```

> You can create a script in package.json to run the command above

```
"scripts": {
    "format": "npx prettier --write .", // format all files
    "format:check": "npx prettier --check .", // check if all files are formatted
  },
```

## Step 3 :: Setup eslint[](#setup_eslint)

#### 3.1 install eslint as dev dependency

```
$ pnpm i --save-d save-exact eslint
```

#### 2.2 create .eslintrc file on root directory ad paste the following code

```
{
  "env": {
    "browser": true,
    "es2021": true
  },
  "extends": ["plugin:react/recommended", "standard-with-typescript", "prettier"],
  "overrides": [],
  "parserOptions": {
    "ecmaVersion": "latest",
    "sourceType": "module",
    "project": "./tsconfig.json"
  },
  "plugins": ["react"],
  "rules": {
    "react/react-in-jsx-scope": "off",
    "@typescript-eslint/explicit-function-return-type": "off"
  }
}

```

> You can setup eslint by following command below

> NOTE : I recommend to use this command to setup eslint, because it will automatically install all the dependencies that you need.

```
$ npx eslint --init
```

You will need to respond to a few questions to configure eslint.

```
* How would you like to use ESLint?
   * To check syntax, find problems, and enforce code style
* What type of modules does your project use?
    * JavaScript modules (import/export)
* Which framework does your project use?
    * React
* Does your project use TypeScript?
    * Yes
* Where does your code run?
    * Browser
* How would you like to define a style for your project?
    * Use a popular style guide
* Which style guide do you want to follow?
    * Standard: https://github.com/standard/eslint-config-standard-with-typescript
* What format do you want your config file to be in?
    * JSON
* The config that you've selected requires the following dependencies: Would you like to install them now with npm?
    * Yes
```

#### 3.3 Execute the command below to confirm the proper setup of .eslintrc

```
$ npx eslint --ext .jsx,.js,.tsx,.ts src/
```

> It should show messages similar like "XXXX problems (XXXX errors, XX warnings)"

> Note: if you does't receive eslint message and have errors while executing following command, please fix them before proceeding to the next step.

#### 3.4 Need to connect .eslintrc with prettier

> you need to install eslint-config-prettier as dev dependency

```
$ pnpm i --save-d save-exact eslint-config-prettier
```

#### 3.5 Add the following code to .eslintrc file to connect prettier

```
{
    ....other-configs
    "extends": [
        ....other-configs
        "prettier"
    ],
    ....other-configs
}
```

#### 3.6 Execute the command below to confirm the proper setup of .eslintrc

```
$ npx eslint --ext .jsx,.js,.tsx,.ts src/
```

> You can create a script in package.json to run the command above

```
"scripts": {
    "lint": "npx eslint --ext .js,.jsx,.ts,.tsx,.vue src", // check if any file has error
    "lint:fix": "npx eslint --ext .js,.jsx,.ts,.tsx,.vue src --fix",  // fix all errors automatically
},
```

## Step 4 :: Setup husky[](#setup_husky)

#### 4.1 install husky as dev dependency

```
$ pnpm i --save-d save-exact husky
```

#### 4.2 create prepare script in package.json

```
$ pnpm pkg set script.prepare "husky install"  // create prepare script
```

> or you can manually set prepare script in package.json

```
"scripts": {
    "prepare": "husky install",
    ....other-scripts
}
```

> Execute the command below to run prepare script

```
$ pnpm run prepare
```

#### 4.3 add a hook to run prettier and eslint before commit

```
npx husky add .husky/pre-commit "pnpm run lit:fix && pnpm run format && pnpm run format:check && pnpm run lint"
```

> You can set your own hook in .husky/pre-commit

```
Folder directory of .husky/pre-commit

.husky/
├── _/
│   └── _
└── pre-commit

open pre-commit file and paste the following code or you can set your own hook

#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

pnpm run lint:fix
pnpm run format
pnpm run format:check
pnpm run lint

```

## Step 5 :: Test[](#test)

> Add file and commit it

```
$ git add .
$ git commit -m "test"
// all script will run automatically which is set in .husky/pre-commit
```
