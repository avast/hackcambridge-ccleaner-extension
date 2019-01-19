### Getting Started

This repository contains a basic CCleaner extension stub to be installed within Avast Secure Browser (ASB). As a Chromium based browser, ASB supports the Chrome extension framework, making this extension also compatible with Chrome and other Chromium based browsers.

As a very first step, we recommend you read the [What are Extensions?](https://developer.chrome.com/extensions), [Getting Started Tutorial](https://developer.chrome.com/extensions/getstarted) and [Overview](https://developer.chrome.com/extensions/overview) pages of the Google Chrome extension documentation as this will help familarise you with the structure of extensions.

### Load an Unpacked Extension

Extensions can be loaded in [unpacked mode](https://developer.chrome.com/extensions/getstarted#unpacked) by following these steps in ASB:

- Visit `secure://extensions` (via: Menu -> More Tools -> Extensions).
- Enable `Developer mode` by ticking the checkbox in the upper-right corner.
- Click on the "Load unpacked extension..." button.
- Select the directory containing the unpacked extension (e.g. [src](./src))

### Cleaning Avast Secure Browser

Chrome already has a set of extension APIs for cleaning/removing browsing data: [chrome.browsingData](https://developer.chrome.com/extensions/browsingData
). We have extended these APIs to provide special cleaning and reporting functionality specifically for our browser:

- Added a `till` removal option - this has the same format as `since` but marks the end timestamp of the cleaning.

- Added `chrome.browsingData.reportCleanResults` function which saves the passed JSON into a temporary file, looks for CCleaner installation path in the registry and executes it with `/report` argument pointing to the temporary file.

- Added cleaning results data as an object argument for callback in `chrome.browsingData.remove()` and other remove functions. The format is a dictionary with data types that were requested to be cleaned. Each data type has a `count` (i.e. total number of entries) and an `entries` array of objects. Inside the `entries` array, there is `path` and `size` information. The `path` may be a URL or some other identifier (e.g. `profile:guid` for form data). The `size` information is represented in bytes and is non-zero only for some data types (e.g. `cache`).
