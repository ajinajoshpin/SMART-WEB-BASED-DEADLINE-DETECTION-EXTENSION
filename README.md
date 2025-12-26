# SMART-WEB-BASED-DEADLINE-DETECTION-EXTENSION
## DONE BY: AJINA JOSHPIN A
## REG NUM:212223230008
## AIM:
To design and develop a Smart Web-Based Deadline Detection Chrome Extension that automatically detects deadline dates from webpages, stores them locally, and notifies users when deadlines are approaching, thereby reducing the risk of missing important events or submissions.
## ALGORITHM:
## STEP1:
Start the Chrome Extension and load required permissions using manifest.json.

## Step 2

Inject the content script into all visited webpages.

## Step 3

Extract the visible text content from the webpage.

## Step 4

Apply regular expression patterns to identify date formats such as:

YYYY-MM-DD

DD/MM/YYYY

Month DD, YYYY

## Step 5

Convert detected dates into a standard format (YYYY-MM-DD).

## Step 6

Store unique detected deadlines in Chrome local storage.

## Step 7

Allow the user to manually add deadlines using the popup interface.

## Step 8

Periodically check stored deadlines using background service worker.

## Step 9

Calculate the number of days remaining for each deadline.

## Step 10

If a deadline is within 3 days, trigger a browser notification.

## Step 11

Repeat the checking process at regular intervals.

# PROGRAM
## MANIFEST.JSON
```
{
  "manifest_version": 3,
  "name": "Smart Deadline Reminder",
  "version": "1.0",
  "permissions": ["storage", "notifications", "alarms"],
  "host_permissions": ["<all_urls>"],
  "background": {
    "service_worker": "background.js"
  },
  "content_scripts": [
    {
      "matches": ["<all_urls>"],
      "js": ["content.js"]
    }
  ],
  "action": {
    "default_popup": "popup.html"
  }
}

```
