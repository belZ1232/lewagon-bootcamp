# lewagon-bootcamp

# 1. Setup

For this lesson, you will need the following tools:

- [Google Chrome](https://www.google.com/chrome/)
- a text editor or IDE like [Sublime Text](https://www.sublimetext.com/) or [VS Code](https://code.visualstudio.com/download).
- the [Github CLI](https://cli.github.com/)

The good news: you should have installed all these tools during the Setup day. These links are here for reference.

Open a terminal window and run the following commands:

```
cd ~/code/lewagon-bootcamp
git clone <https://github.com/trouni/workshop-chrome-extension.git>  chrome-extension
cd chrome-extension

```

> If you get a command not found: git  error message, install Github CLI with the link above.
> 

That's it for the setup! You're good to go 🚀

# 2. Recap : Javascript and the DOM

Before we dive into extensions, let's go through a quick JavaScript recap on how to interact with a web page. Complete the below exercises:

Go to the [Le Wagon alumnis story page](https://www.lewagon.com/tokyo/graduates).

We are going to use the **Chrome Developer Tools** to add some javascript to this page.
Let, start by opening the Chrome console:
**`⌘ Cmd + ⌥ Opt + J`** on Mac
**`✲ Ctrl + ⇧ Shift + J`** on Windows

### 1. Selecting elements in the DOM

Let's write a Javascript code that **selects all the names** of the alumnis featured on the page and that **displays them on the console**.
<details>
<summary markdown="span">Need a hint? 💡</summary>

```
// Target the first element matching the CSS selector
document.querySelector('.css-selector')

// Get an array of all the elements matching the CSS selector
document.querySelectorAll('.css-selector')

// Iterate over each selected element
document.querySelectorAll('.css-selector').forEach(element => {
  console.log(element)
})

```

</details>
<details>
<summary markdown="span">Solution 🤓</summary>

```
// Get an array of all the elements matching the class card-graduate-name
const nameElements = document.querySelectorAll('.card-graduate-name');

// Iterate over each of them and display their innerText
nameElements.forEach(nameElement => {
  console.log(nameElement.innerText)
})

```

</details>

### 2. Editing an element's attributes

Go to [this Le Wagon alumni story page](https://www.lewagon.com/tokyo/graduates#yoshiki-bell).

Now, let's write a JavaScript code to **select all the images** of the page and to **turn them into this cute hedgedog picture** 👇
`https://raw.githubusercontent.com/trouni/workshop-chrome-extension/master/images/hedgehog.jpg`

<details>
<summary markdown="span">Need a hint? 💡</summary>

```
// Change opacity of the element
element.style.opacity = 0.5

// Add a CSS class to the element
element.classList.add('hidden')

// Change an image
img.src = '<https://raw.githubusercontent.com/trouni/workshop-chrome-extension/master/images/hedgehog.jpg>'

```

</details>
<details>
<summary markdown="span">Solution 🤓</summary>

```
// Get an array of all the img elements
const imgElements = document.querySelectorAll('img');

 // Iterate over each of them and change the image source
imgElements.forEach(imgElement => {
  imgElement.src = '<https://raw.githubusercontent.com/trouni/workshop-chrome-extension/master/images/hedgehog.jpg>';
})

```

</details>

### 3. User interaction

Go to [Cassiano's Le Wagon alumni story page](https://www.lewagon.com/tokyo/graduates#cassiano-yasumitsu)

Let's write a Javascript code to **select the article main picture** and to fire an alert saying "Yep, that is Cassiano!" when we click on it.

You can ask the browser to listen for events and trigger actions using `addEventListener()`:

<details>
<summary markdown="span">Need a hint? 💡</summary>

```
element.addEventListener('click', (event) => {
  // Some action
  alert('The event has been triggered')
})

```

Check out the [Event reference documentation](https://developer.mozilla.org/en-US/docs/Web/Events) for a complete list of available events.
</details>
<details>
<summary markdown="span">Solution 🤓</summary>

```
// Get an array of all the img elements
const mainImage = document.querySelector('.modal-story-cover');

// When we click on it fire an alert
mainImage.addEventListener('click', (event) => {
  alert('Yep, that is Cassiano!');
})

```

</details>

# 3. Bascis of the Chrome extensions

We'll create an extension that replaces all images on a web page with photos of cheese 🧀.

### Unsplash Source

Let's use [Unsplash Source](https://source.unsplash.com/), to find random cheese images [like this one](https://source.unsplash.com/featured/?cheese), using `https://source.unsplash.com/collection/8884129/`.

> You can also use https://source.unsplash.com/featured/?cheese and replace cheese with anything you want (e.g. wine, puppy, etc.)
> 

### Let's start with a JS code

Go to [this website](https://japantoday.com/).
Open your Chrome console, use the Javascript skills you've earned so far to replace all the images with photos of cheese.

<details>
<summary markdown="span">Need a hint? 💡</summary>

1. Select all `img` elements
2. Iterate over each of them using `forEach`
3. Update the `src` and `srcset` attributes with the Unsplash url
More help in [the previous section](https://learn.lewagon.com/c/1936/b/5374).
</details>

Once this is done, let's put this script in a Chrome extension!

### Basic structure of a Chrome extension

Open your Chrome Extension app project:

```
cd ~/code/lewagon-bootcamp/chrome-extension
subl

```

The Chrome extensions most important file is called a **manifest**. Do you see a `manifest.json` in your project folder?

The manifest is a simple JSON file that tells the browser about your extension, and it is the only file that every extension using WebExtension APIs must contain.

```
// manifest.json

{
  "manifest_version": 2,
  "name": "My first Chrome Extension",
  "description": "Chrome extension workshop for Le Wagon Tokyo",
  "author": "Your name",
  "version": "1",
  "permissions": ["tabs"],
  "content_scripts": [
    // ...
  ],
  "background": {
    // ...
  },
  "browser_action": {
    // ...
  },
  "icons": {
    "16": "images/icon16.png",
    "32": "images/icon32.png",
    "48": "images/icon48.png",
    "128": "images/icon128.png"
  }
}

```

### Loading our extension into Chrome

We're are going to load our Chrome extension project in our browser, it will enable us to test it in real-time:

1. Enter `chrome://extensions` in the Chrome search bar, and activate developer mode (top-right corner)
2. Click '**Load unpacked**' and select your workshop-chrome-extension folder.

Our extension is setup in Chrome, time to brush up the scripts it contains 🚀

4. Your first content script
Content scripts & Background / Event scripts
Extensions can contain 2 types of scripts Content scripts & Background scripts.

Content scripts run in the context of a web page / tab, and allow you to get information from it, or even change its contents. On the other side, as its name suggests, a background script runs in the background of the Chrome browser, acting as a controller and used to maintain state for your extension.
While content scripts have limited access to the Chrome Extension APIs, background scripts can make full use of them. As a general rule, content scripts should be used to interact with web pages / tabs, while the logic should ideally be located in the background script.
Creating our first content script
Since it interacts with our page, our image replacing script should go into a content script.
Add this code 👇 to the scripts/cheesify.js file of our chrome-extension project.
document.querySelectorAll('img').forEach( (img) => {
  img.src = `https://source.unsplash.com/collection/8884129/${img.width}x${img.height}?${Math.random()}`;
  img.srcset = img.src;
})

​
Adding our script to the manifest
In your manifest, add the following to run our cheesify script on all the pages we visit.
// manifest.json

{
  // ...

  "content_scripts": [
    {
      "matches": [
        "<all_urls>"
      ],
      "js": ["scripts/cheesify.js"]
    }
  ],

  // ...
}

# 5. Build your extension UI

Our script now runs on every single page we visit, and although I'm definitely loving all that cheesy goodness, I can think of a few situations where replacing all images on the internet with photos of coagulated milk may not be entirely relevant. So let's add a menu to our extension, in order to trigger the cheesification of our page only when we *actually* need it.

### Creating the menu UI in HTML/CSS

Create your menu or copy-paste the code below into `popup.html`.

```
<!-- popup.html -->

<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <title>My first Chrome extension</title>
  <link rel="stylesheet" href="style/popup.css">
</head>
<body>
  <h1>My first Chrome extension</h1>
  <button id="cheesify"><span>🧀</span><p>Cheesify Page</p></button>
  <script src="scripts/popup.js"></script>
</body>
</html>

```

*I have already included some CSS styling in `style/popup.css` for the template above.*

### Adding our menu to the manifest

We need to let Chrome know that the menu for our extension is now in our `popup.html` file.

Add this to your manifest.json file.

```
// manifest.json

{
  // ...

  "browser_action": {
    "default_popup": "popup.html",
    "default_title": "My first Chrome Extension"
  },

  // ...
}

```

### Passing messages to tabs / content scripts

When we click the button of our `popup.html` page, we should send a message to the `cheesify.js` content script and trigger our image replacement script.

Here are some useful methods to pass messages to content scripts:

```
// Find the tab(s) you want to send a message to by querying the open tabs in Chrome
chrome.tabs.query( queryInfo, (responseCallback) )

// Send a message to a tab when you know its ID
chrome.tabs.sendMessage( tabId, message, (options), (responseCallback) )

```

*Learn more in the [chrome.tabs API](https://developer.chrome.com/extensions/tabs).*

Let's apply this to our extension and trigger the cheesify script when we click on our button.

```
// scripts/popup.js

// Send a message to the active tab to 'cheesify' it
function sendCheesifyMsg() {
  chrome.tabs.query({active: true, currentWindow: true}, function(tabs) { // Finds tabs that are active in the current window
    chrome.tabs.sendMessage(tabs[0].id, { action: 'cheesify' }); // Sends a message (object) to the first tab (tabs[0])
  });
}

// Trigger the function above when clicking the 'Cheesify' button
document.querySelector('#cheesify').addEventListener('click', event => sendCheesifyMsg());

```

### Listening for messages in tabs / content scripts

Now, all that's keeping us from turning our web page into dairy heaven is learning how to intercept the message we've just sent, then trigger an action based on that. We will be using the chrome.runtime API and more specifically, the `onMessage.addListener` method:

```
chrome.runtime.onMessage.addListener(
  function(request, sender, sendResponse) {
    // actions based on the request (which corresponds to the object we sent in our message)
  }
);

```

*Learn more in the [chrome.runtime API](https://developer.chrome.com/apps/runtime).*

Complete/replace your code in cheesify.js with the one below.

```
// cheesify.js

// Listen for messages on the content page
chrome.runtime.onMessage.addListener(
  function(request, sender, sendResponse) {
    if (request.action === 'cheesify') cheesify();
  }
);

// Our image replacement script
function cheesify() {
  document.querySelectorAll('img').forEach( (img) => {
    img.src = `https://source.unsplash.com/${img.width}x${img.height}/?cheese&${Math.random()}`;
    img.srcset = img.src;
  })
}

```

Awesome! You should now be able to click on the extension's icon, then click on the 'Cheesify Page' button to run our cheesify script.

# 6. Publish your extension

The Chrome extension is up and running on your browser, now it's time to share it with the world! 🌍

1. Create your chrome-extension project’s zip file
*Note: in the next steps, you will need to Pay a Google developer's signup fee of $5. If you don't want to go for it and don't want to publish your extension, just send us the ZIP file as a deliverable of this workshop.*
2. [Create a developer account](https://chrome.google.com/webstore/developer/dashboard)
3. Upload your app
4. Pay the developer's signup fee
5. Share your published extension URL with us 👇 as a deliverable of this workshop.

*Full official tutorial available [here](https://developer.chrome.com/webstore/publish)*

You can publish unlisted extension and share the direct link if you don't want to make your extension public.
