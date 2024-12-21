# FalconLink Extensions Documentation
- ### Visit [Getting Started](/gettingstarted) for a detailed explanation on creating an extension.
- ### [Join Our Discord](https://discord.gg/TSp7qKf4wY) to report any bugs or feature requests.

FalconLink Extensions makes it really easy to add stuff to the proxy.

Here is an example manifest.json:

    {
        "version": "1",
        "icon": "icon.png",
        "id": 123,
        "name": "Cool Extension",
        "images": [
            "image1.png",
            "image2.png",
            "image3.png"
        ],
        "script": "index.js"
    }
In this manifest.json, the index.js script will be run. Index.js can access a extension API that allows it to inject content scripts, manage tabs, and more!
Here is a basic index.js script that will inject an adblocker:

    async function injectAdBlocker() {
        const script = await API.getSrcFor("adblocker.js");
        const tabs = await API.getAllTabs();
        for (var i = 0; i < await tabs.length; i++) {
            if (!(await API.isScriptInjected(await script, await tabs[i].id))) {
                API.injectScript(await script, await tabs[i].id);
            }
        }
    }

    setInterval(injectAdBlocker, 1000);

Content scripts that are injected can also communicate with the main script through message channels. Here is how you can send a message in a content script:

    const channel123 = new ExtensionMessageChannel();
    channel123.sendMessage("testing123");

Then you can use this code to recieve the message in your main script:

    API.onMessageChannelCreate = (channel) => {
        channel.onMessage = (message) => {
            alert("We have a message!!!: " + message);
        };
    }