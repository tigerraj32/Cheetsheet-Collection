# Compiling TypeScript

TypeScript is a typed superset of JavaScript that compiles to plain JavaScript. It offers classes, modules, and interfaces to help you build robust components. The TypeScript language specification has full details about the language.

### Install the TypeScript compiler
Visual Studio Code includes TypeScript language support but does not include the TypeScript compiler, tsc. You will need to install the TypeScript compiler either globally or in your workspace to transpile TypeScript source code to JavaScript (tsc HelloWorld.ts).

The easiest way to install TypeScript is through npm, the Node.js Package Manager. If you have npm installed, you can install TypeScript globally (-g) on your computer by:

```bash 
npm install -g typescript
```

You can test your install by checking the version or help.

```bash
tsc --version
tsc --help
```
Another option is to install the TypeScript compiler locally in your project (npm install --save-dev typescript) and has the benefit of avoiding possible interactions with other TypeScript projects you may have.

### Compiler versus language service

It is important to keep in mind that VS Code's TypeScript language service is separate from your installed TypeScript compiler. You can see the VS Code's TypeScript version in the Status Bar when you open a TypeScript file.

![TypeScript version displayed in the Status Bar](./resources/typescript-1.png)

Later in the article, we'll discuss how you can change the version of TypeScript language service that VS Code uses.

### tsconfig.json
Typically the first step in any new TypeScript project is to add a tsconfig.json file. A tsconfig.json file defines the TypeScript project settings, such as the compiler options and the files that should be included. To do this, open up the folder where you want to store your source and add a new file named tsconfig.json. Once in this file, IntelliSense (⌃Space) will help you along the way.

![tsconfig.json IntelliSense](./resources/typescript-2.png)

A simple tsconfig.json looks like this for ES5, CommonJS modules and source maps:

```json
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "sourceMap": true
  }
}
```

Now when you create a .ts file as part of the project we will offer up rich editing experiences and syntax validation.

### Transpile TypeScript into JavaScript

VS Code integrates with tsc through our integrated task runner. We can use this to transpile .ts files into .js files. Another benefit of using VS Code tasks is that you get integrated error and warning detection displayed in the Problems panel. Let's walk through transpiling a simple TypeScript Hello World program.

**Step 1: Create a simple TS file**

Open VS Code on an empty folder and create a helloworld.ts file, place the following code in that file...

```js
let message: string = 'Hello World';
console.log(message);
```

To test that you have the TypeScript compiler tsc installed correctly and a working Hello World program, open a terminal and type 
`tsc helloworld.ts.` You can use the Integrated Terminal (`⌃`) directly in VS Code.

You should now see the transpiled helloworld.js JavaScript file, which you can run if you have Node.js installed, by typing node helloworld.js.

![build and run Hello World](./resources/typescript-3.png)

**Step 2: Run the TypeScript build**

Execute Run Build Task `(⇧⌘B)` from the global Terminal menu. If you created a tsconfig.json file in the earlier section, this should present the following picker:

![TypeScript Build](./resources/typescript-4.png)

Select the `tsc: build` entry. This will produce a HelloWorld.js and HelloWorld.js.map file in the workspace.

If you selected `tsc: watch`, the TypeScript compiler watches for changes to your TypeScript files and runs the transpiler on each change.

Under the covers, we run the TypeScript compiler as a task. The command we use is: tsc -p .

**Step 3: Make the TypeScript Build the default#**

You can also define the TypeScript build task as the default build task so that it is executed directly when triggering Run Build Task (⇧⌘B). To do so, select Configure Default Build Task from the global Terminal menu. This shows you a picker with the available build tasks. Select TypeScript tsc: build, which generates the following tasks.json file in a .vscode folder:

```
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
        {
            "type": "typescript",
            "tsconfig": "tsconfig.json",
            "problemMatcher": [
                "$tsc"
            ],
            "group": {
                "kind": "build",
                "isDefault": true
            }
        }
    ]
}
```
Notice that the task has a group JSON object that sets the task kind to build and makes it the default. Now when you select the Run Build Task command or press (⇧⌘B), you are not prompted to select a task and your compilation starts.

> Tip: You can also run the program using VS Code's Run/Debug feature. Details about running and debugging Node.js applications in VS Code can be found in the Node.js tutorial

**Step 4: Reviewing build issues**

The VS Code task system can also detect build issues through a problem matcher. A problem matcher parses build output based on the specific build tool and provides integrated issue display and navigation. VS Code ships with many problem matchers and $tsc seen above in tasks.json is the problem matcher for TypeScript compiler output.

As an example, if there was a simple error (extra 'g' in console.log) in our TypeScript file, we may get the following output from tsc:

HelloWorld.ts(3,17): error TS2339: Property 'logg' does not exist on type 'Console'.
This would show up in the terminal panel `(⌃)` and selecting the Tasks - build tsconfig.json in the terminal view dropdown.

You can see the error and warning counts in the Status Bar. Click on the error and warnings icon to get a list of the problems and navigate to them.

![Error in Status Bar](./resources/typescript-5.png)

You can also use the keyboard to open the list ⇧⌘M.

>Tip: Tasks offer rich support for many actions. Check the Tasks topic for more information on how to configure them.

**JavaScript source map support**

TypeScript debugging supports JavaScript source maps. To generate source maps for your TypeScript files, compile with the --sourcemap option or set the sourceMap property in the tsconfig.json file to true.

In-lined source maps (a source map where the content is stored as a data URL instead of a separate file) are also supported, although in-lined source is not yet supported.

**Output location for generated files**

Having the generated JavaScript file in the same folder at the TypeScript source will quickly get cluttered on larger projects. You can specify the output directory for the compiler with the outDir attribute.

```jspn
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "outDir": "out"
  }
}
```
**Hiding derived JavaScript files**

When you are working with TypeScript, you often don't want to see generated JavaScript files in the File Explorer or in Search results. VS Code offers filtering capabilities with a files.exclude workspace setting and you can easily create an expression to hide those derived files:

`**/*.js: { "when": "$(basename).ts" }`

This pattern will match on any JavaScript file `(**/*.js)` but only if a sibling TypeScript file with the same name is present. The File Explorer will no longer show derived resources for JavaScript if they are compiled to the same location.

![Hiding derived resourcesHiding derived resources](./resources/typescript-6.png)

Add the files.exclude setting with a filter in the workspace settings.json file, located in the .vscode folder at the root of the workspace. You can open the workspace settings.json via the Preferences: Open Workspace Settings (JSON) command from the Command Palette (⇧⌘P).

To exclude JavaScript files generated from both .ts and .tsx source files, use this expression:

"files.exclude": {
    "**/*.js": { "when": "$(basename).ts" },
    "**/**.js": { "when": "$(basename).tsx" }
}
This is a bit of a trick. The search glob pattern is used as a key. The settings above use two different glob patterns to provide two unique keys but the search will still match the same files.


## Running nodejs application in Production [pm2]

`pm2` is a great tool to manage node server in production.  To install `pm2`

```
npm install pm2 -D // for development or
npm install pm2 // globally
```

**Run node server via mp2**

``` 
pm2-dev out/app.js //or
pm2 out/app.js

```

pm2 will monitor for change in file and restart server. Also if the server crash it will restart the node server

**Automate build process and server restart**

> Step 1: Configure script in package.json

Here we create a dev script to run node server via pm2
```

"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "pm2-dev out/app.js"
  }
```
We can run the server via `node run dev`

> Step 2: To add dev script to vscode task 

- Go to terminal in Vscode Toolbar
- Select configure task
- Select dev script

THis will add new item in **task.json** as following

```
{
	"version": "2.0.0",
	"tasks": [
		{
			"type": "typescript",
			"tsconfig": "tsconfig.json",
			"problemMatcher": [
				"$tsc"
			],
			"group": "build",
			"label": "tsc: build - tsconfig.json"
		},
		{
			"type": "npm",
			"script": "dev",
			"problemMatcher": [],
			"label": "npm: dev",
			"detail": "pm2-dev out/app.js",
			"group": {
				"kind": "build",
				"isDefault": true
			}
		}
	]
}
```

Now if we build or run the project [SHIFT + COMMAND + B]

  


### Refrence Link
[Compiling TypeScript](https://code.visualstudio.com/docs/typescript/typescript-compiling)
[Video Explanation](https://www.youtube.com/watch?v=BLoFPda7fmI)
