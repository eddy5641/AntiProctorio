# AntiProctorio
Proctorio is a privacy nightmare and it is ineffective. Personally, I am strongly against snake oil cheat prevention software and from my research, proctorio cannot effectively proctor any exam securely.

Update: I am not releasing the SRC because it will make cheating too easy. I've been able to verify that the methods below are valid; however, it appears that Proctorio is switching to a desktop application. I cannot tell if this is a response to try and prevent cheating OR a response to the deprication of PNaCl.

I still stand by my recomendation that universities should switch to Respondus Lockdown Browser w Respondus Monitor.

If there are any questions or concerns, feel free to make an issue in this repo

# Table of Contents
  * [Why](#why)
    * [Better Solutions](#better-solutions)
  * [Issues](#issues-with-browser-extensions)
    * [Reverse engineering extensions](#1-determining-what-an-app-does-is-easy)
    * [Trusting browsers](#2-trust-on-browsers)
    * [Chromium is open source](#3-chromium-is-open-source)
  * [Solutions](#solutions)
  * [How this works](#how-this-works)

# Why
This is a PoC that proves that Proctorio is ineffective as an anti-cheat. If you are proctoring an exam, AVOID using any software that relies on a browser (POCU Proctor, Proctorio, ProctorU, etc).

#### NOTE: I am against cheating on exams. This is here for educational purposes. It is trivial to correct this issue and this repo is to encourage these platforms to actually be effective and use those solutions, or for any educational institutions to switch to a better solution.

## Better solutions
If you are part of an educational institution consider using one of the following:
- Respondus Lockdown Browser (opt. Respondus Monitor)

# Issues with browser extensions
## 1 Determining what an app does is easy
Browser extensions are easy to read and reverse engineer. As per: https://developer.chrome.com/webstore/program_policies 

Developers must not obfuscate code or conceal functionality of their extension. This also applies to any external code or resource fetched by the extension package.

Modifying the app to ignore certain browser windows and informing the server of cheating wouldn't be too difficult.

## 2 Trust on browsers
Browser extensions inherently trust the browser they run on. Implementing a solution similar to game cheats would be easy, as the browser itself is not trying to stop cheating. 

The browser extension cannot detect this type of application from running because it trusts the browser and the apis used.

The extention probably uses https://developer.chrome.com/native-client/overview which sandboxes all calls to the system.

Implementing https://github.com/microsoft/detours#microsoft-research-detours-package would allow us to modify any call the chrome makes to the system. Any checks that the extension makes to ensure that it is not running in a vm can be modified, any checks to look for other app running can also be modified.

This project relies on this 2nd issue.

## 3 Chromium is open source
Browser extensions can be run in a custom version of chromium which would implement return false information to the plugin (disguising itself as the chrome browser) and would be a similar solution to the injection based issue.

# Solutions
Do not trust any browser. Instead use chromium https://www.chromium.org/ and package it with your application. Because you package your application, you can digitally sign all of the files with your own code signing key and validate all of the files upon launch and implement a TLS callback to increase difficulty with DLL injection. This is a lot more work but greatly increases the work required to create a cheat when compared to when the application is only a browser extension.

An alternative to this, is to create a test where cheating is unfavourable.

# How this works
## Injection
We inject into chrome any hook any calls for the functions we want to implement.

## Camera + Window
You can bypass the camera checks by using a high quality cell phone camera. I used a iPhone 11 Pro Max and created an app that used the true depth camera, and streamed it to the system to setup a virtural camera. A deepfake of your own face can then be used to change your glance as well as adding noise to make it harder to detect (and look like a crappy webcam).

## Processes
We hide the injector and any firefox browser processes from the chrome browser while injected. We also implement a macro for copy and pasting which will play back typing clips (from earlier) and 

## Microphone
By recording typing snippets (~500 words) at slow and fast speeds, we can bypass any of the microphone checks by mixing in keyboard typing at the correct volume. We can also use room audio and process it to determine voices to remove any other voices speaking
