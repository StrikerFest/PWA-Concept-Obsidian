# Overview

- The application shell pattern involves heavily  caching the minimal amount of HTML CSS and JS to load the basic UI of the page before fetching the rest through an API

- Application shell rendering is instantaneous on repeat visits because the majority of the page is in the cache.
- It also prevent unnecessary data usage because it removes the need to download static content more than once
