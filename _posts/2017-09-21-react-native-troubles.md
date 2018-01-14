---
layout: post
date: 2017-09-21
tags: frustrated
categories: react-native
title: React Native hell
---

i am in react native hell. js devs are spoiled - there is a good dev ecosystem and an uncomplicated system of dependencies. when crossing over to react native, js devs are forced to interface with xcode/ios for the first time and this is pure shit.

a list of the problems i faced trying to enable share extensions for an app:

- <https://stackoverflow.com/questions/31002642/app-installation-failed-could-not-write-to-the-device>
- <https://github.com/facebook/react-native/issues/8584>
- <https://stackoverflow.com/questions/33990925/using-react-native-within-an-ios-share-extension/34099070#34099070>

this was the closest i came:

- <https://www.promptworks.com/blog/building-ios-app-extensions-with-react-native> the extension is just blank and i cant get it to show. filed issue: <https://github.com/promptworks/ignite-action-extension-example/issues/4>
- <https://github.com/meedan/react-native-share-menu/blob/master/example/ios/README.md> seems to have decent instructions but ran into terminal issues
- <https://github.com/alinz/react-native-share-extension> this guy is actually active but i was not able to get it beyond a blank screen: <https://github.com/alinz/react-native-share-extension/issues/58>

direction to/from was also tricky. this was misleading: <https://github.com/EstebanFuentealba/react-native-share>

i also tried and wasted time on infinitered/ignite.
