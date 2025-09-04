# Bitfocus-Companion-Guide

Guides and examples to perform various functions in Bitfocus Companion.

## Sponsor

These guides, as with Companion, are free and open source, but if you'd like to support the continued development of this and my Companion modules (vMix, Google Sheets, Twitch, Discord, Voicemeeter, and more) tips will always be appreciated either on [Github](https://github.com/sponsors/thedist) or [Ko-Fi](https://ko-fi.com/thedist).

# Guides

- [Expressions](/guides/Expressions.md)
- [HTTP API](/guides/HTTP.md)
- [Random Actions](/guides/RandomActions.md)

## Expressions

A lot of power comes from Companions Expressions system, which in fields that support it allow doing mathematical operations, if-else logic, perform functions on strings/numbers/JSON, create template strings to create complex strings of text from multiple other elements, compare 2 timestamps to create countdowns, and much more.

## HTTP API

Companion has a HTTP API that allows for features such as pressing a button. With the deprecated API that will be removed in the future it was possible to just type an endpoint into a browsers URL bar to make the GET requests, but as the new API mostly uses POST requests this is no longer possible, so I'll show how to make a simple page that makes these POST requests and utilize the API when loading the page.

## Random Actions

The [Random Actions](/guides/RandomActions.md) guide shows how entirely within Companion you can use Expression Functions to randomly select a value out of a list of many and then perform an action with it, such as transitioning to a random camera. This example also includes how to randomly generate a time, so that these actions are performed randomly within a set minimum/maximum window.
