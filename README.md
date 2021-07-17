<div align="center">
  <p><img src="https://i.imgur.com/loLmHkX.png"></p>
  <br>
  <p><a href="https://www.youtube.com/watch?v=bcDOv09-5RI"><img src="https://media.giphy.com/media/eABiZprIEtouRZIc75/giphy.gif"></a></p>
  <h2>Map face and hand gestures to a mouse, keyboard, virtual game controller, MIDI device, robot, drone...basically anything!</h2>
  <br>
  <h3>🐞 This is still a prototype and may be buggy 🐞</h3>
  <br>
  <p>
    <a href="https://github.com/midiblocks/midblocks-web"><img src="https://img.shields.io/github/stars/midiblocks/midiblocks-web?style=social"></a>&nbsp;
    <a href="https://github.com/MIDIBlocks/midiblocks-web/milestone/1"><img src="https://img.shields.io/github/milestones/progress/midiblocks/midiblocks-web/1"></a>&nbsp;
    <a href="https://github.com/MIDIBlocks/midiblocks-web"><img src="https://img.shields.io/github/last-commit/midiblocks/midiblocks-web"></a>&nbsp;
  </p>
</div>




<br>
<br>
<br>

-------------------------------------------------------------------------

<br>
<br>
<br>




## Quickstart Setup
```bash
# 1) Setup
git clone https://github.com/midiblocks/midiblocks-web
cd midiblocks-web
npm install

# 2a) Web based mode (native browser events)
npm start

# 2b) Desktop mode (native desktop events)
npm i robotjs
npm run desktop

# 3) Point your browser to http://localhost:8080
```

> ### Troubleshooting
> ---
> **🐞 Windows** - If you run into errors after `npm install` it's probably due to Robot.js dependencies (most likely you need Python and node-gyp). If you don't need desktop mode then remove the line in `package.json` for `"robotjs"` and try running `npm install` again. If you do want desktop mode, then [check out this README](https://github.com/MIDIBlocks/handsfree-desktop) for a troubleshooting guide.
> 
> This will be fixed soon!



<br>
<br>
<br>

-------------------------------------------------------------------------

<br>
<br>
<br>



# Contents
- [About](#about)
  - [The Studio](#the-studio)
  - [The Factory](#the-factory)
  - [The Library](#the-library)
- [Motivation](#motivation)
<!-- @todo Move these into corresponding about section -->
- [Keyboard shortcuts](#keyboard-shortcuts)
- [Setup](#setup)
  - [Prereqs](#prereqs)
  - [Installing](#installing)
  - [Browser support](#browser-support)
- [Dev](#dev)
  - [Command line scripts](#command-line-scripts)
  - [window.app](#windowapp)
  - [localStorage](#localstorage)
- [License](#license)



<br>
<br>
<br>

-------------------------------------------------------------------------

<br>
<br>
<br>



# About

**Midiblocks is a visual, all-in-one gesture mapping and handsfree desktop automation platform designed to help you use devices handsfree!** Some use cases include:

- [Playing commercial video games handsfree with face gestures](https://www.youtube.com/watch?v=fgVIZx-jkXA)

- [Controlling robots handsfree with head movements](https://youtu.be/lb-Bkejm0Pg?t=54)

- [Handsfree and voice-free typing](https://www.youtube.com/watch?v=o0i_AivvsQ4)

- [Using a mobile device as a handsfree secondary display](https://www.youtube.com/watch?v=NNqIkVRzWbs)

Midiblocks is centered around blocks of code that are literally represented visually by blocks. Midiblocks is broken up into several views:

- [The Studio](#the-studio) is where you actually snap the blocks together to map gestures to things. All of the blocks in a Studio workspace are collectively referred to as `midiblocks`

- Each individual block represents JavaScript code and [The Factory](#the-factory) is where you design the shape of these blocks and write its code

- [The Library](#the-library) is where your Factory Blocks and Studio Midiblocks are stored. You can edit, remix, and delete them there

### The Studio

![](https://i.imgur.com/qeidIys.png)

> 🚧 More info coming soon

### The Factory

![](https://i.imgur.com/3h229Km.png)

> 🚧 More info coming soon

### The Library

![](https://i.imgur.com/iZpHeVh.png)

> 🚧 More info coming soon



<br>
<br>
<br>

-------------------------------------------------------------------------

<br>
<br>
<br>



# Setup
## Prereqs
- [Git](https://git-scm.com/downloads)
- [NodeJS LTS (eg 12.x.x)](https://nodejs.org/en/)

## Installing
- Open terminal and clone this repository with: `git clone https://github.com/midiblocks/midiblocks`
- Change into directory with: `cd midiblocks`
- Run `npm install` from the project root to install dependencies (> 250Mb)
- Run `npm start` from the project's root to start the app at [localhost:8080](http://localhost:8080)

## Browser Support
This project works in all browsers that natively support the [Web MIDI API](https://webaudio.github.io/web-midi-api/). Currently, the following browsers have built-in support:

- Chrome (macOS, GNU/Linux, Android & Windows)
- Opera (macOS, GNU/Linux, Windows)
- Android WebView component (KitKat and above)
- Edge (Windows)

For more info, including on how to support Firefox, Internet Explorer, and Safari see the [WebMidi docs](https://github.com/djipco/webmidi#browser-support)

> Starting with version 77, [Chrome deprecates Web MIDI usage on insecure origins](https://www.chromestatus.com/feature/5138066234671104). This means that, going forward, the page will need to be hosted on a secure origin (e.g. https://, localhost: or file:///) and the user will need to authorize usage (no matter if sysex is used or not). [Firebase](https://firebase.google.com/products/hosting), [Netflify](https://www.netlify.com/), Github, and others provide free hosting.



<br>
<br>
<br>

-------------------------------------------------------------------------

<br>
<br>
<br>



# Dev


## Command line scripts
```bash
# DEVELOPMENT 
# -----------
# Start the app in development mode with localStorage
npm start

# HANDSFREE DESKTOP 
# -----------
# Same as `npm start` but it controls the desktop pointer instead of a virtual one
# Adds a "Desktop Mode" toggle
npm run desktop

# PRODUCTION
# ----------
# Build the app for production with localStorage
npm run build

# DEPLOY
# ----------
# Push the app to firebase. Must have:
# - Firebase Tools: npm i -g firebase-tools
# - Initialized with: firebase init
npm run deploy
```

## `window.app`
The following is available through your browser's dev console:

```js
window.app = {
  $,        // Main Vue instance
  $studio,  // The Studio component (only present when on that route)
  $factory  // The Factory component (only present when on that route)
}
```

> 🚧 More info coming soon

## localStorage
The following is available inside `localStorage`:

```js
{
  blocks: [],           // All loaded Factory Blocks
  midiblocks: [],       // All loaded Studio Midiblocks
  currentFactory: {},   // The Factory's last state (autosave)
  currentStudio: {},    // The Studio's last state (autosave)
  splitter: 50
}
```

> 🚧 More info coming soon



<br>
<br>
<br>

-------------------------------------------------------------------------

<br>
<br>
<br>



# Keyboard Shortcuts

<table>
  <thead>
    <tr>
      <th>Location</th>
      <th>Shortcut</th>
      <th>Action</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Anywhere</td>
      <td><kbd>s</kbd></td>
      <td>Navigate to the Studio</td>
    </tr>
    <tr>
      <td>Anywhere</td>
      <td><kbd>f</kbd></td>
      <td>Navigate to the Factory</td>
    </tr>
    <tr>
      <td>Factory</td>
      <td><kbd>ctrl + s</kbd></td>
      <td>Save the current block</td>
    </tr>
  </tbody>
</table>



<br>
<br>
<br>

-------------------------------------------------------------------------

<br>
<br>
<br>



# License: [MIT](https://opensource.org/licenses/MIT)

Copyright 2020 midiblocks

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.