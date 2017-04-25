# How To Download All Attachments From A Gmail Conversation

Sometimes you want to download all the attachments in a long, many-message Gmail conversation. Gmail doesn't provide any simple way to do this. But these two methods seem to work:

## If the attachments are *less than 25MB* in total

1. Open the conversation
2. Click the "More" dropdown and select "Forward all"
3. Forward the full conversation to yourself
4. Open the forwarded email, scroll to the bottom, and click the "Download all attachments" button

[h/t [David Sottimano](http://www.davidsottimano.com/how-to-download-all-attachments-from-a-gmail-thread/)]

## If the attachments are *more than 25MB* in total

This approach worked for me, though ⚠⚠⚠  your mileage may vary ⚠⚠⚠ :

1. Open the conversation (ideally in a new window, but not necessary)
2. Open your browser's developer console
3. Paste the following JavaScript into the console:

```js
(function () {
    var interval_seconds = 1000;
    var links = [].slice.apply(document.querySelectorAll("a[role='link']"));
    var urls = links.map(function (x) {
        return x.parentNode.getAttribute("download_url").match(/https:\/\/[^:]+$/)[0];
    });
    var timer = window.setInterval(function () {   
        if (urls.length) {  
            window.open(urls.pop())
        } else { window.clearInterval(timer); }
    }, interval_seconds);
}).call(this);
```

This will start the process of downloading the attachments. Every second, your browser will open a new tab, which will download a particular attachment and then close itself when the download has completed.

### Notes

- Currently only tested in Chrome
- Some time interval seems to be necessary to avoid Gmail temporarily blocking your account, but the choice of one-second interval above is somewhat arbitrary
