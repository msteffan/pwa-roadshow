# PWA Roadshow 10/22

## Keynote
- All about building radically better UI/UX
- FIRE: Fast, integrated, reliable, engaging
  - Fast
    - Time is money -> After 3 seconds of load, 20% of users bounce
    - 53% of users abandon sites that take longer than 3 seconds to load
  - Integrated
    - Users should be able to interact with site the same way as any app
  - Reliable
    - Experience should be consistent even if there is no network connection
    - "reliability means never showing the Downasaur"
  - Engaging
    - whole experience is delightful - web push API
    - push notifications turn occasional users into daily users
    - Twitter Lite -> launches into fullscreen app...even though it's just mobile web
- Reaching emerging markets -> PWA is 0.6 MB compared to 26MB Android app
- All possible because of service workers!
  - client-side JavaScript proxy
- PWA: Not everything works everywhere yet, but UX is key. Focusing on end-to-end experience dramatically impacts experience for everyone, no matter what their device supports
- Three ways to PWA-ify
  - From the ground up
  - A simpler/stripped-down version
  - A single feature

## Integrated Experience
- Manifest file
  - `/manifest.json`
    - name
    - short_name
    - start_url
    - display: standalone (hides address bar, just like a native app)
    - icons
    - colors -> background_color, theme_color
- Only 1 in 20 "carts" on mobile web gets completed -> 66% fewer conversions
  - Checkout forms are awful! Tedious. Slow. Multiple taps.
  - Make it better
    - tell the browser to `autocomplete` fields -> 30% faster form completion
  - ...what if we just got rid of forms?
    - paymentRequest API (w3C Standard!)
      - create paymentRequest
      - Request payment details
      - collect payment via the device system (more trusted than a web form)

## Reliable Experience
- "lie-fi" :(
  - no network connectivity even when your phone says you're online
- 60% of world-wide mobile connections are 2G
- Service Workers
  - interact with users even if they're not actively in the browser
  - adds app-like lifecycle
  - service worker is for second load
- lifecycle of a service worker
  - Install
    - register
    ```
      if ('serviceWorker' in navigator){
        navigator.serviceWorker.register('/service-worker.js')
      }
    ```
      - if you install from root, it will service the whole url; if you install from a subdir, it will only control scripts in that subdir (i.e. don't put service worker in `/scripts` if you want it to control the whole site)
    - Install event handler -> pre-fetch the app resources & cache them: `caches.open(cacheName); return cache.addAll(filesToCache)`
  - Idle (waiting for things to happen)
  - Activated (reacts to events raised by the page)
    - check for update? -> if yes, install new version & repeat
    - `activate` event handler
      - clean up previous resources if new version is installed
  - Terminated
- Caching Strategies
  - "Cache and then fall back to network"
    - SW looks first in cache; if not, it goes to network before returning response to page
      - useful for resources that won't change
  - "Network first but then fall back to cache"
    - useful for things like a newsfeed where you always want latest version, but falls back to cached content if network is unavailable/flaky
  - "Cache, then Network"
    - SW issues cache and network requests simultaneously
      - updates cache with new content
      - may serve stale from cache if that request comes back first
  - "Generic fallbak"
    - SW -> cache -> network -> cache -> generic response if both are unavailable
- Tools
  - Chrome Dev Tools "Application" panel
  - Workbox.js

## Engaging Experiences
- Service workers aren't enough!
- Need to think about the complete experience -> What do we typically put on the web, and do we need it on every page? ie. footer links
- App-like Experiences -> Material design, iOS human interface guide
- Key question: "Is this notification important enough to warrant an interruption?"
- What makes a good notification?
  - they can get the info they need without even looking at the app
  - Trinity: Timely, Precise, Relevant
- Ask if user wants to see notifications -> if they say no, don't show the browser prompt
- Weather.com - specific, contextual, user initiated
  - "activate browser notifications"
  - Gives users options about which alerts to see
  - Make it easy to unsubscribe
- TLDR: Engage user FIRST before initiating browser prompt
- Web push libraries
  - github.com/web-push-libs

## Security
- PWA requires HTTPS for identity, confidentiality, and integrity
- Cost of HTTPS
  - Certificate
  - Search ranking -> solve by 301 redirect
- Chrome Dev Tools "Security" panel  

## SPA to PWA
- github.com/googleChrome/sw-precache
- understand how things are going to look if JS is disabled
- Lighthouse PWA audit - goal is not to get 100/100, but to identify areas that need focus
