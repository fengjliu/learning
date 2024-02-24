# links 
https://segmentfault.com/a/1190000008427041

how to make a simple electron app:
    https://www.zhangxinxu.com/wordpress/2017/05/electron-node-js-desktop-application-experience/
Main and Renderer processes coperation, and how to handling lifecycle
# link 2
https://electron-vite.org/guide/dev

Electron applications are set up using npm packages. The Electron executable should be installed in your project's devDependencies and can be run in development mode using a script in your package.json file.

The executable runs the JavaScript entry point found in the main property of your package.json. This file controls Electron's main process, which runs an instance of Node.js and is responsible for your app's lifecycle, displaying native interfaces, performing privileged operations, and managing renderer processes.

Renderer processes (or renderers for short) are responsible for displaying graphical content. You can load a web page into a renderer by pointing it to either a web address or a local HTML file. Renderers behave very similarly to regular web pages and have access to the same web APIs.

In the next section of the tutorial, we will be learning how to augment the renderer process with privileged APIs and how to communicate between processes.
