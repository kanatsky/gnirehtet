adb devices , check z/s/t/m 

win: gnirehtet start > accecpt vpn connection on current usb device. 

tcp/udp ipv4 only.

cmd window is active process, closing it  

gnirehtet autorun

# src md


## Requirements

The Android application requires at least API 21 (Android 5.0).

For the _Java_ version only, _Java 8_ (JRE) is required on your computer. On
Debian-based distros, install the package `openjdk-8-jre`.

### adb

You need a recent version of [adb] (where `adb reverse` is implemented, it
works with 1.0.36).

It is available in the [Android SDK platform tools][platform-tools].

On Debian-based distros, you can alternatively install the package
`android-tools-adb`.

On Windows, if you need `adb` only for this application, just download the
[platform-tools][platform-tools-windows] and extract the following files to the
_gnirehtet_ directory:
 - `adb.exe`
 - `AdbWinApi.dll`
 - `AdbWinUsbApi.dll`

Make sure you [enabled adb debugging][enable-adb] on your device(s).

[adb]: https://developer.android.com/studio/command-line/adb.html
[enable-adb]: https://developer.android.com/studio/command-line/adb.html#Enabling
[platform-tools]: https://developer.android.com/studio/releases/platform-tools.html
[platform-tools-windows]: https://dl.google.com/android/repository/platform-tools-latest-windows.zip


## Download

Download the [latest release][latest] in the flavor you want.

[latest]: https://github.com/Genymobile/gnirehtet/releases/latest

### Rust

 - **Linux:** [`gnirehtet-rust-linux64-v2.3.zip`][direct-rust-linux64]  
   (SHA-256: _561d77e94d65ecf2d919053e5da6109b8cceb73bffedea71cd4e51304ccaa3d3_)
 - **Windows:** [`gnirehtet-rust-win64-v2.3.zip`][direct-rust-win64]  
   (SHA-256: _4c4d961f069a663d6b826f24a8795d1752f3e316f121d2bfc296a4cd32c8d4ca_)
 - **MacOS:** [`gnirehtet-rust-macos64-v2.2.1.zip`][direct-rust-macos64]
   _(previous release)_  
   (SHA-256: _902103e6497f995e1e9b92421be212559950cca4a8b557e1f0403769aee06fc8_)

[direct-rust-linux64]: https://github.com/Genymobile/gnirehtet/releases/download/v2.3/gnirehtet-rust-linux64-v2.3.zip
[direct-rust-win64]: https://github.com/Genymobile/gnirehtet/releases/download/v2.3/gnirehtet-rust-win64-v2.3.zip
[direct-rust-macos64]: https://github.com/Genymobile/gnirehtet/releases/download/v2.2.1/gnirehtet-rust-macos64-v2.2.1.zip

Then extract it.

The Linux and MacOS archives contain:
 - `gnirehtet.apk`
 - `gnirehtet`

The Windows archive contains:
 - `gnirehtet.apk`
 - `gnirehtet.exe`
 - `gnirehtet-run.cmd`


### Java

 - **All platforms:** [`gnirehtet-java-v2.3.zip`][direct-java]  
   (SHA-256: _93d1d46ee566376596f033832626dd5e89e76c91f2c46d2383735937b7d3b8b0_)

[direct-java]: https://github.com/Genymobile/gnirehtet/releases/download/v2.3/gnirehtet-java-v2.3.zip

Then extract it. The archive contains:
 - `gnirehtet.apk`
 - `gnirehtet.jar`
 - `gnirehtet`
 - `gnirehtet.cmd`
 - `gnirehtet-run.cmd`


## Run (simple)

_Note: On Windows, replace `./gnirehtet` by `gnirehtet` in the following
commands._

The application has no UI, and is intended to be controlled from the computer
only.

If you want to activate reverse tethering for exactly one device, just execute:

    ./gnirehtet run

Reverse tethering remains active until you press _Ctrl+C_.

On Windows, for convenience, you can double-click on `gnirehtet-run.cmd`
instead (it just executes `gnirehtet run`, without requiring to open a
terminal).

The very first start should open a popup to request permission:

![request](assets/request.jpg)

A "key" logo appears in the status bar whenever _Gnirehtet_ is active:

![key](assets/key.png)

Alternatively, you can enable reverse tethering for all connected devices
(present and future) by calling:

    ./gnirehtet autorun


## Run

You can execute the actions separately (it may be useful if you want to reverse
tether several devices simultaneously).

Start the relay server and keep it open:

    ./gnirehtet relay

Install the `apk` on your Android device:

    ./gnirehtet install [serial]

In another terminal, for each client, execute:

    ./gnirehtet start [serial]

To stop a client:

    ./gnirehtet stop [serial]

To reset the tunnel (useful to get the connection back when a device is
unplugged and plugged back while gnirehtet is active):

    ./gnirehtet tunnel [serial]

The _serial_ parameter is required only if `adb devices` outputs more than one
device.

For advanced options, call `./gnirehtet` without arguments to get more details.


## Run manually

The `gnirehtet` program exposes a simple command-line interface that executes
lower-level commands. You can call them manually instead.

To start the relay server:

    java -jar gnirehtet.jar relay

To install the apk:

    adb install -r gnirehtet.apk

To start a client:

    adb reverse localabstract:gnirehtet tcp:31416
    adb shell am broadcast -a com.genymobile.gnirehtet.START \
        -n com.genymobile.gnirehtet/.GnirehtetControlReceiver

To stop a client:

    adb shell am broadcast -a com.genymobile.gnirehtet.STOP \
        -n com.genymobile.gnirehtet/.GnirehtetControlReceiver


## Why _gnirehtet_?

    rev <<< tethering

(in _Bash_)


## Developers

Read the [developers page].

[developers page]: DEVELOP.md


## Licence

    Copyright (C) 2017 Genymobile

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

        http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.


## Articles

- [Introducing “gnirehtet”, a reverse tethering tool for Android][medium-1] ([French version][blog-1])
- [Gnirehtet 2: our reverse tethering tool for Android now available in Rust][medium-2]
- [Gnirehtet rewritten in Rust][blog-2-en] ([French version][blog-2-fr])

[medium-1]: https://medium.com/@rom1v/gnirehtet-reverse-tethering-android-2afacdbdaec7
[blog-1]: https://blog.rom1v.com/2017/03/gnirehtet/
[medium-2]: https://medium.com/genymobile/gnirehtet-2-our-reverse-tethering-tool-for-android-now-available-in-rust-999960483d5a
[blog-2-en]: https://blog.rom1v.com/2017/09/gnirehtet-rewritten-in-rust/
[blog-2-fr]: https://blog.rom1v.com/2017/09/gnirehtet-reecrit-en-rust/
