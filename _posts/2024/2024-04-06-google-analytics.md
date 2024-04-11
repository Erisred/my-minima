---
layout: post
title: "Adding Google Analytics capaility to your site"
tags: test
---
Adding Google Analytics is pretty easy. All you need is your property code.

## The code
- Login to your Google Analytics account.
- Click **Admin** in the lower left corner.
- In the property section, click **Data Streams**.
- Click the relevant data stream.
- In the Google tag section at the bottom of the Web streams details page, click Configure tag settings.
- In the *Your Google tag* section on the Google tag page, copy the ID that starts with "G-" or "AW-".

## Adding the code
- Look in the `config.yml` file for the following:

```
# Add Google analytics - add your analytics code and uncomment
# google_analytics: UA-XXXXXXXX
```

- Replace "UA-XXXXXXXX" above with the code you copied from the first step. Be sure to include the "UA-" or the "G-".
- Remove the pound sign "#" from the beginning of that line. Make sure to leave it on the first line.
- Save and reload the site.

That should be it.

### Credits
I had some help. I found most of what I needed on these pages:

- Google analytics code instructions taken mostly from [here](https://support.similarweb.com/hc/en-us/articles/7783050018589-How-to-find-Google-Analytics-Tracking-ID#:~:text=Google%20Analytics%20properties%20have%20a,which%20started%20with%20%22UA%22.).
- Interating to Jekyll Minima was found [here](https://tamrazyan.com/simple-way-to-integrate-google-analytics-into-jekylls-minima-theme/).