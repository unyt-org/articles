<!--
	{
		description: "A simple guide for beginners",
		preview: "res/uix_banner.png",
		date: ~2023-07-18~,
		tag: "Developer",
		author: "unyt.org",
		authorRef: https://unyt.org
	};
-->

# Hello World - Getting started with UIX

For the best developer experience, we recommend using 
[Visual Studio Code](https://code.visualstudio.com/download) 
with the [DATEX Workbench Extension](https://marketplace.visualstudio.com/items?itemName=unytorg.datex-workbench).

## Installing UIX

First let's install UIX.
If you are using a Mac or Linux device, run
```sh
curl -s https://dev.cdn.unyt.org/uix/install.sh | sh
```
in your terminal.

## Setup

For our simple hello world application, we just need a single TypeScript file and two configuration files:
 * A `deno.json` file, containing an import map for required libaries and a JSX configuration:
	```json
	{
		"imports": {
			"unyt_core": "https://dev.cdn.unyt.org/unyt_core/datex.ts",
			"uix": "https://dev.cdn.unyt.org/uix/uix.ts",
			"unyt_core/": "https://dev.cdn.unyt.org/unyt_core/",
			"uix/": "https://dev.cdn.unyt.org/uix/",
			"uix_std/": "https://dev.cdn.unyt.org/uix/uix_std/",
			"unyt_tests/": "https://dev.cdn.unyt.org/unyt_tests/",
			"unyt_web/": "https://dev.cdn.unyt.org/unyt_web/",
			"unyt_node/": "https://dev.cdn.unyt.org/unyt_node/",
			"uix/jsx-runtime": "https://dev.cdn.unyt.org/uix/jsx-runtime/jsx.ts",
			"backend/": "./backend/",
			"common/": "./common/",
			"frontend/": "./frontend/"
		},
		"compilerOptions": {
			"jsx": "react-jsx",
			"jsxImportSource": "uix",
			"lib": [
				"deno.window",
				"dom"
			]
		}
	}
	```
 * An `app.dx` file, containing parameters for the UIX app, like the name and icon:
	```datex
	name: "Hello World"
	```

Now we will create a `backend` directory next to the configuration files, containing an `entrypoint.ts` file with the content
```ts
export default "Hello World"
```

## Running our UIX App

We can now start our app locally by running `uix` in the root directory.

The app should be available on `http://localhost:80`.
Per default, our app is also immediately accessible via a unyt.app subdomain (todo: restricted access).


## A more advanced example

It is good practice to always add a `satisfies UIX.Entrypoint` to default entrypoint exports to make
sure the exported value can be rendered on the browser:
```ts
import {UIX} from "uix";
export default "Hello World" satisfies UIX.Entrypoint
```