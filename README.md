# Angular workspace configuration

This is a guide for setting up a robust and efficient Angular workspace. It includes instructions for integrating ESLint and Prettier, for improving code quality and consistency.

## Install Angular ESLint

```shell
ng add @angular-eslint/schematics
```

## Install Prettier and Prettier-ESLint dependencies

```shell
npm install prettier prettier-eslint eslint-config-prettier eslint-plugin-prettier --save-dev
```

## ESLint configuration

### .eslintrc.json

```json
{
  "root": true,
  "ignorePatterns": ["projects/**/*"],
  "overrides": [
    {
      "files": ["*.ts"],
      "parserOptions": {
        "project": ["tsconfig.json", "e2e/tsconfig.json"],
        "createDefaultProgram": true
      },
      "extends": [
        "eslint:recommended",
        "plugin:prettier/recommended",
        "plugin:@typescript-eslint/recommended",
        "plugin:@angular-eslint/recommended",
        "plugin:@angular-eslint/template/process-inline-templates"
      ],
      "rules": {
        "@angular-eslint/directive-selector": [
          "error",
          {
            "type": "attribute",
            "prefix": "app",
            "style": "camelCase"
          }
        ],
        "@angular-eslint/component-class-suffix": [
          "warn",
          { "suffixes": ["Page", "Component"] }
        ],
        "@angular-eslint/component-selector": [
          "warn",
          {
            "type": "element",
            "prefix": "app",
            "style": "kebab-case"
          }
        ],

        "@typescript-eslint/member-ordering": 0,
        "@typescript-eslint/naming-convention": 0,

        // "no-unused-vars": "off",
        "@typescript-eslint/no-unused-vars": "warn",

        "@typescript-eslint/no-explicit-any": "off"
      }
    },
    // NOTE: WE ARE NOT APPLYING PRETTIER IN THIS OVERRIDE, ONLY @ANGULAR-ESLINT/TEMPLATE
    {
      "files": ["*.html"],
      "extends": [
        "plugin:@angular-eslint/template/recommended",
        "plugin:@angular-eslint/template/accessibility"
      ],
      "rules": {}
    },
    // NOTE: WE ARE NOT APPLYING @ANGULAR-ESLINT/TEMPLATE IN THIS OVERRIDE, ONLY PRETTIER
    {
      "files": ["*.html"],
      "excludedFiles": ["*inline-template-*.component.html"],
      "extends": ["plugin:prettier/recommended"],
      "rules": {
        // NOTE: WE ARE OVERRIDING THE DEFAULT CONFIG TO ALWAYS SET THE PARSER TO ANGULAR (SEE BELOW)
        "prettier/prettier": ["error", { "parser": "angular" }]
      }
    }
  ]
}
```

## Prettier configuration

### .prettierrc

```json
{
  "tabWidth": 4,
  "useTabs": false,
  "singleQuote": false,
  "semi": true,
  "bracketSpacing": true,
  "arrowParens": "avoid",
  "trailingComma": "all",
  "bracketSameLine": false,
  "printWidth": 80
}
```

### .prettierignore

```
build
coverage
e2e
node_modules
dist
```

## Angular configuration

### angular.json

```json
{
  // ...
  "architect": {
    // ...
    "lint": {
      "builder": "@angular-eslint/builder:lint",
      "options": {
        "lintFilePatterns": ["src/**/*.ts", "src/**/*.html"]
      }
    }
  }
}
```

## VSCode configuration

### extensions.json

```json
{
  "recommendations": [
    "angular.ng-template",
    "dbaeumer.vscode-eslint",
    "esbenp.prettier-vscode",
    "formulahendry.auto-rename-tag",
    "aaron-bond.better-comments",
    "usernamehw.errorlens",
    "yoavbls.pretty-ts-errors"
  ]
}
```

### settings.json

```json
{
  "[html]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "editor.codeActionsOnSave": {
      "source.fixAll.eslint": "always"
    },
    "editor.formatOnSave": false
  },
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    // "editor.defaultFormatter": "dbaeumer.vscode-eslint",
    "editor.codeActionsOnSave": {
      "source.fixAll.eslint": "always"
    },
    "editor.formatOnSave": false
  },
  "editor.suggest.snippetsPreventQuickSuggestions": false,
  "editor.inlineSuggest.enabled": true
}
```

## References

- https://gist.github.com/eneajaho/17bbcf71c44eabf56d404b028572b97b
- https://github.com/angular-eslint/angular-eslint#notes-for-eslint-plugin-prettier-users
