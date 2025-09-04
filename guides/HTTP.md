# HTTP API

The HTTP API allows for some remote control of Companion using HTTP requests, and this example shows how to make a HTTP request automatically when a webpage loads to press and release a button in Companion. While this is very basic usage of the HTTP API, functionally even more advanced uses boil down to just making HTTP requests but just on button presses or when certain things happen rather than just automatically on load.

For a full list of API endpoints please check the 'Remote Control' > 'HTTP Remote Control' section of the Getting Started guide.

## Security Warning

As with the Web UI itself, if you configure your router to forward internet traffic to Companion so that remote users can control it you will also be exposing the HTTP endpoints, so please make sure you appropriately restrict traffic to just the IPs that you need to externally access Companion or otherwise anyone can potentially misuse your Companion setup. And please don't assume that having a password on Companion provides any sort of security, that's purely to stop accidental button presses and is NOT a security measure as it can be bypassed under a second as it is not intended to be a security measure, nor does it even work on the HTTP API endpoints.

## Enabling HTTP

First make sure you have the HTTP API enabled, you can find this in the Settings > Protocol section in Companion. While you can also enable the Deprecated HTTP API, I advise against it as it will be removed in the future so this guide will only utilize the current HTTP API.

## Sending a HTTP Request

When making HTTP requests there are different verbs that interact with the endpoint different, for example `GET`, `POST`, `PATCH`, `PUT`, `DELETE`, are just a few of the many verbs, but the main ones that people will be familiar with are `GET` which is what your browser does when you enter an address in the URL bar, and `POST`, which is often used to submit data or trigger an action, which is what is often used for most of the Companion HTTP endpoints.

There are dozens of HTTP libraries to make HTTP requests, and they exist in many different programming languages, but the library I'll use in this example is `fetch` because it's a javascript library built in to almost all modern browsers, as well as NodeJS should users use that, so doesn't require downloading or installing anything.

As the HTTP API documentation shows, to press and release a button you need to use the `/api/location/<page>/<row>/<column>/press` endpoint. So if I'm running Companion on `http://localhost:8000` and want to press a button on page 1, row 2, column 3, the endpoint is `http://localhost:8000/api/location/1/2/3/press`. Because a `POST` request is needed though it means you can't just type this into your browsers URL bar as that would make a `GET` request which would error, so to send it as a `POST` request I can specify that in the `fetch` options as `fetch('http://localhost:8000/api/location/1/2/3/press', { method: 'POST' })`.

If you was to run that fetch request in your browser as a script, or by opening up the dev tools and typing it in to the console it would work, and press/release button 2/3 on page 1. In the following example I show a HTML page that when opened just has a script that will run that fetch function, and then log the result or catch any error that may happen.

```html
<html>
  <script>
    fetch('http://localhost:8000/api/location/1/2/3/press', { method: 'POST' })
      .then((response) => response.text())
      .then((response) => {
        console.log(response)
      })
      .catch((err) => {
        console.warn(err)
      })
  </script>
</html>
```

That's all that is needed at the most basic level. You can save that example in notepad as example.html, open it in your browser, and it'll run that function, or you could use it as a browser source in OBS or vMix and again each time that it is loaded it will run that function (I don't recommend this at all, there are much better way to trigger Companion from OBS or vMix but this was purely an example of what could be done).

If you were to run it in your browser, you can press F12 to open DevTools, and in the Console window you should see `ok` if the request was successful, otherwise it would show a warning containing the error message.
