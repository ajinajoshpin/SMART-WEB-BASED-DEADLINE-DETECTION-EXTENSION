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
## content.js
```
function scanForDates() {
  const bodyText = document.body.innerText;
  const patterns = [
    /\b\d{4}-\d{2}-\d{2}\b/g,
    /\b\d{1,2}\/\d{1,2}\/\d{4}\b/g,
    /\b(?:Jan|Feb|Mar|Apr|May|Jun|Jul|Aug|Sep|Oct|Nov|Dec)[a-z]* \d{1,2},? \d{4}\b/gi
  ];

  let foundDates = [];
  patterns.forEach(regex => {
    const matches = bodyText.match(regex);
    if (matches) foundDates.push(...matches);
  });

  chrome.storage.local.get(['deadlines'], (result) => {
    let deadlines = result.deadlines || [];

    foundDates.forEach(dateStr => {
      let stdDate = new Date(dateStr);
      if (!isNaN(stdDate)) {
        deadlines.push({
          task: "Auto detected deadline",
          date: stdDate.toISOString().split('T')[0]
        });
      }
    });

    chrome.storage.local.set({ deadlines });
  });
}

scanForDates();
setTimeout(scanForDates, 3000);

```
## BACKGROUND.JS
```
chrome.alarms.create("deadlineCheck", { periodInMinutes: 60 });

chrome.alarms.onAlarm.addListener(() => {
  chrome.storage.local.get(['deadlines'], (result) => {
    const now = new Date();

    result.deadlines.forEach(item => {
      const deadline = new Date(item.date);
      const daysLeft = Math.ceil((deadline - now) / (1000 * 60 * 60 * 24));

      if (daysLeft >= 0 && daysLeft <= 3) {
        chrome.notifications.create({
          type: 'basic',
          iconUrl: 'icon.png',
          title: 'Deadline Reminder',
          message: `${item.task} is due in ${daysLeft} days`
        });
      }
    });
  });
});
```
## POPUP.HTML
```
<h3>Saved Deadlines</h3>
<div id="list"></div>
<input type="text" id="task" placeholder="Task Name">
<input type="date" id="date">
<button id="add">Add</button>
<script src="popup.js"></script>
```
## POPUP.JS
```
document.getElementById('add').addEventListener('click', () => {
  const task = task.value;
  const date = date.value;

  chrome.storage.local.get(['deadlines'], (result) => {
    const deadlines = result.deadlines || [];
    deadlines.push({ task, date });
    chrome.storage.local.set({ deadlines });
  });
});

```
# OUTPUT:
Deadlines present on webpages are automatically detected and stored.

User can manually add deadlines through the popup interface.

Browser notifications are displayed when deadlines are within three days.

Stored deadlines persist even after browser restart.
<img width="491" height="192" alt="image" src="https://github.com/user-attachments/assets/48a1fb01-757b-4121-9a82-3d14914d81d7" />
<img width="437" height="266" alt="Screenshot 2025-12-26 231228" src="https://github.com/user-attachments/assets/e66f011d-3d62-4055-8cb3-250a17e7838c" />
<img width="1484" height="76" alt="Screenshot 2025-12-26 231717" src="https://github.com/user-attachments/assets/d028e641-47e0-4c9e-9da5-b5071fe06474" />
<img width="288" height="345" alt="Screenshot 2025-12-26 231724" src="https://github.com/user-attachments/assets/73d2f08a-f7b0-4046-82ed-608b76d51ef1" /><img width="453" height="545" alt="Screenshot 2025-12-26 231738" src="https://github.com/user-attachments/assets/e977f0b2-9118-4328-aac6-38a4abf7ca1f" />








## RESULT:
Thus, a Smart Web-Based Deadline Detection Chrome Extension was successfully designed and implemented. The system accurately detects deadline dates from webpages, stores them locally, and notifies users in advance, thereby improving time management and reducing missed deadlines.
