# How to start developing a FalconLink Extension
## Installation
- Make sure you have NodeJS and NPM installed.
```console
apt install npm nodejs
```
- Clone the [FalconLink repo](https://github.com/Falconlink/FalconLink).
```console
git clone https://github.com/Falconlink/FalconLink
```
- Go to the FalconLink directory
```console
cd FalconLink
```
- Install Packages
```console
npm i
```
## Development
#### Now you are ready to start developing your extension
<br>
<b>Click the button below to generate an ID for your extension.</b>

<span><button onclick="this.parentNode.innerHTML = 'Here is your ID: ' + Date.now()">Generate ID</button></span>

Next,

- Go to the directory public/static/extensions using your IDE of choice.
- Create a new folder named the extension ID you were just given.
- Inside that folder create two files, index.js and manifest.json.
- Put this inside your manifest.json, replacing it with your values:
```json
    {
    "version": "1", //Once we update the library, we will use this to know what version to run an extension with, ensuring backwards compatability. Currently version 1 is newest
    "id": 0, //ID for your extension
    "name": "Extension Name", //I am making a extension that spins your tabs around :P
    "script": "index.js", //The main script that is run in your extension
    "images": [], //List of images for the extension store page
    "icon": "about:blank" //Icon for the extension, if none, put about:blank
    }
```
- Inside you index.js file you can put the code for your extension, you can also interact with the proxy's browser using the API commands detailed in the [Docs](/docs) page. For now I will just alert the users that the extension has been started:
```javascript
alert("Extension Started!");
```
## Running the extension
- To test out your extension, start the server by running
```bash
npm start
```
This will start a server on port 8080
- Now, go to [127.0.0.1:8080](http://127.0.0.1:8080) in your browser

- Next, open the dev console and type in
```js
new Extension(extensionID) //Replace extensionID with the ID of your extension
```
Make sure that you replace extensionID with your extensions ID.

- You should see a alert box that says "Extension Started!" If not [Join Our Discord](https://discord.gg/TSp7qKf4wY) for help

## Submitting your extension
- Create a pull request on the [FalconLink GitHub Repo](https://github.com/Falconlink/FalconLink). Also it may help to [join our discord](https://discord.gg/TSp7qKf4wY) and tell us about the extension, as we don't check the github repo very often.