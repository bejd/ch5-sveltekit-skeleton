# Crestron HTML5 with SvelteKit

A barebones CH5 + SvelteKit project.

![Crestron + Sveltekit](screenshot.png?raw=true)

## Contents
1. [Requirements](#1-requirements)
2. [Installation](#2-installation)
3. [Communicating with a Crestron processor](#3-communicating-with-a-crestron-processor)
4. [Sending your project to a panel](#4-sending-your-project-to-a-panel)
5. [More stuff](#5-more-stuff)

## 1. Requirements

If you haven't already, install [Node.js](https://nodejs.org/en) and [Git](https://git-scm.com/)

## 2. Installation

1. Open a terminal window
2. Navigate to where you want to create the project folder
	- E.g. `cd '.\Projects\`
3. Clone this repo into your project folder
	- `git clone https://github.com/bejd/ch5-sveltekit-skeleton`
	- `cd ch5-sveltekit-skeleton` to navigate to the newly cloned folder
	- `npm install` to install all the Node modules and set things up
	- `npm run dev` to start the development server

Once the developer server is started, you can view the project by opening a web browser and navigating to [localhost:5173](http://localhost:5173/). A Chromium-based browser (e.g. Chrome, Edge) is recommended as the Crestron panel runs a version of Chrome.

To stop the dev server (e.g. if you want to install some more modules for your project), focus on your terminal window and press `Ctrl + C`.

## 3. Communicating with a Crestron processor

Crestron provide a library, [CrComLib](https://sdkcon78221.crestron.com/sdk/Crestron_HTML5UI/Content/Topics/QS-Installation.htm), which exposes several functions we can use to send and receive signals to and from a processor.

### Sending signals

In the following Svelte example when the button is clicked, `pressed()` is called. The `publishEvent()` function here is the equivalent of a digital join ("b" for boolean) sent to join number 21 with a value of true/high/1/on. [Crestron's help pages](https://sdkcon78221.crestron.com/sdk/Crestron_HTML5UI/Content/Topics/Advanced/Events-Joins-CS.htm) have more information.

```html
<script>
	import { CrComLib } from "@crestron/ch5-crcomlib/build_bundles/cjs/cr-com-lib.js";

	function pressed() {
		CrComLib.publishEvent("b", "21", true);
	}
</script>

<button type="button" on:mousedown={pressed}>
	Click me
</button>
```

### Receiving signals

Similarly, your JavaScript can receive information from the processor via the `subscribeState()` function.

## 4. Sending your project to a panel

To build the app, Crestron provide some [command line tools](https://sdkcon78221.crestron.com/sdk/Crestron_HTML5UI/Content/Topics/QS-Installation.htm).

### Installing the tools

Use npm to install the tools globally.

1. `npm install -g @crestron/ch5-utilities-cli`
2. `npm install -g @crestron/ch5-shell-utilities-cli`

### Building, archiving, and deploying

1. Build the app
	- `npm run build`
	- This will put your files in a `.\build` subfolder
2. Use Crestron's archival tool to prepare files for deployment
	- `ch5-cli archive -p archive -d .\build -o ..\my-ch5-folder`
	- This makes a file `archive.ch5z` using the files in `.\build`, and places them in a folder that's a sibling of your project directory called `my-ch5-folder`.
3. Send the archive to the panel
	- `ch5-cli deploy -p -H [panel IP address] -t touchscreen ..\my-ch5-folder\archive.ch5z`
	- You'll be prompted for the panel's username and password (the `-p` option tells the tool to do this)

## 5. More stuff

- [Cres-Svelte-Tron](https://github.com/purebordem/Cres-Svelte-Tron) is a similar project, but using a version of Svelte which uses Rollup instead of Vite. It also has some really neat scripts which build and push to your panel all at once.
- [Crestron-Community-Rescources](https://github.com/purebordem/Crestron-Community-Resources) has lots of useful links.
- [Crestron Professionals Discord](https://discord.gg/VU4xT9j) for lots of expert help.

