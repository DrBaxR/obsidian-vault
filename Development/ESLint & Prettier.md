---
tags:
 - frontend
---

These two tools are used for [[Linting]] [[Front End]] applications. This note contains the steps that need to be followed to set them up for a project and integrated with [[VS Code]].

add a bunch of dev dependencies and init eslint for project

```sh
npm install --save-dev eslint prettier prettier-eslint
npm install --save-dev @typescript-eslint/parser # (if using typescript)
npm init @eslint/config
```

at this point, you will be able to lint your files via ESLint (if it doesn't work, delete the **_"eslint2021": true_** environment)

```sh
npm install --save-dev eslint-config-prettier
```

also, add "prettier" to plugins in .eslintrc:

```json
{
	"env": {
		"browser": true
	},
	"extends": [
		"eslint:recommended",
		"plugin:react/recommended",
		"plugin:@typescript-eslint/recommended",
		"prettier"
	],
	"parser": "@typescript-eslint/parser",
	"parserOptions": {
		"ecmaFeatures": {
			"jsx": true
		},
		"ecmaVersion": "latest",
		"sourceType": "module"
	},
	"plugins": [
		"react",
		"@typescript-eslint"
	],
	"rules": {}
}
```

### Resources
[https://eslint.org/docs/user-guide/getting-started](https://eslint.org/docs/user-guide/getting-started)
[https://prettier.io/docs/en/integrating-with-linters.html](https://prettier.io/docs/en/integrating-with-linters.html)
[https://github.com/prettier/eslint-config-prettier](https://github.com/prettier/eslint-config-prettier)

**Note:** as of writing this (04/05/2022) you need to switch to pre-release version of prettier eslint vscode plugin