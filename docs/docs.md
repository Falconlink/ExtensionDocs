# FalconLink Extensions Documentation
## Interacting with the API
In the main script, you can access the API via the API object. This object will contain all the functions you need to interact with the proxy. Pay close attention to how each API function is used, most of the functions are asynchronous meaning you have to use "await".
# Tab objects
The API has multiple functions that return Tab objects. Here is the structure of a Tab Object:

    {
        "url": "https://google.com", //Current URL of the tab
        "title": "Google Search", //Current title of the tab
        "id": "123321" //Unique ID assigned to every proxy tab
    }
Here are the API functions that return tab objects (Note that all of them are asynchronous):
## await API.getCurrentTab()
##### Arguments: None
##### Return value: Tab Object
This function will return a promise that resolves to the tab object of the tab the user is currently on.
## await API.getAllTabs()
##### Arguments: None
##### Return value: Array of Tab Objects
This function will return a promise that resolves to an array of all the tab objects.
## await API.getTabByID(id)
##### Arguments: ID
##### Return value: Tab Object
This function will return a promise that resolves to the tab object of the tab ID given.
# Injecting Scripts
## await API.injectScript(script, tabID, inline)
##### Arguments: Script, TabID, Inline (optional, defaults to false)
- Script is either a URL to a javascript file or javascript code. If it is javascript code, the inline argument must be set to true, if not the inline argument is not neccesary.
- TabID is the ID of the tab you want to inject this script into.
- NOTE: If using a JavaScript file that is part of your extensions source code, you can use the [API.getSrcFor](#await-apigetsrcforpath) function to generate a URL for the file.
##### Return value: Boolean
Will return true if the script was successfully injected.

This function will inject a script element in the end of the tabs body. It returns a promise that resolves to true if the injection was successful.
## await API.isScriptInjected(script, tabID, inline)
##### Arguments: Script, TabID, Inline (optional, defaults to false)
- Script is either a URL to a javascript file or javascript code. If it is javascript code, the inline argument must be set to true, if not the inline argument is not neccesary.
- TabID is the ID of the tab you want to inject this script into.
- NOTE: If using a JavaScript file that is part of your extensions source code, you can use the [API.getSrcFor](#await-apigetsrcforpath) function to generate a URL for the file.
##### Return value: Boolean
Will return true if the script is injected or false if it is not.

This function will check if the provided script has already been injected. It returns a promise that resolves to a boolean.
# Misc. Functions
## await API.getSrcFor(Path)
##### Arguments: Path
##### Return value: URL
This function takes a file path (ex: "/scripts/content_script.js") that is relative to your extension and returns a promise that resolves to a URL that that file can be accessed at. This is usefull for things like opening an extension page or injecting a script.
## await API.getWindowByID(ID)
##### Arguments: ID
##### Return value: Window
This function takes a [Tab ID](#tab-objects) and returns a promise that resolves to the window object of that tab.
## await API.createTab(url, proxy)
##### Arguments: URL, Proxy (Optional, defaults to true)
- URL is the URL of the page you want to open.
- Proxy is a boolean that specifies if you want the page to be proxied. In most cases, this should be left at true.
##### Return value: Tab ID
This function takes the URL of the page you want to open and if you want it proxied or not, and returns a promise that resolves to the Tab ID of the new tab.
# Messaging
Messaging allows communication between the main script, and content scripts. Messaging Channels are created by the content scripts, and once made can have messages sent both ways.
## Creating a Message Channel
To create a message channel, create a new instance of the ExtensionMessageChannel class inside of a content script.

    const myMessageChannel = new ExtensionMessageChannel();
##### <b>IMPORTANT: Message Channels can only be created in injected content scripts. If you create a message channel in for example, a buttons onclick attribute, it will not work.</b>
This will create a channel. Now to recieve messages that are sent in a channel inside the main script, set API.onMessageChannelCreate

    //Show any messages recieved to the user.
    API.onMessageChannelCreate = (channel) => {
        channel.onMessage = (message) => {
            alert("We have a message!!!: " + message);
        };
    }
Now you can send messages.

content_script.js:

    myMessageChannel.sendMessage("testing123");

And it should make a popup box saying "We have a message!!!: testing123" appear.