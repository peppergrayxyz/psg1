# Sideloading

Games from PlaySolana can be installed, but sideloading is restricted:

UI:
```
Unknown apps can't be installed by this user 
```
ADB:

```
$ adb install somepackage.apk
Performing Streamed Install
adb: failed to somepackage.apk: Failure [INSTALL_FAILED_USER_RESTRICTED: Installer not allowed: null (uid=-1), pkg=...]
``` 

The restriction blocks unknown installers, but trusts `com.playsolana.echos` as the installer package. 


This can be utilized to circumvent the restrictions:

```
$ adb shell pm install --user 0 -i com.playsolana.echos /data/local/tmp/somepackage.apk
Success
```

## Step by Step Guide

### I. Inital Setup to install from PC (USB)

#### 1. Enabler Developer Options ####

- navigate to `Settings` > `About Device`
- scroll to the bottom
- click `Build number` multiple times
- a toast will show up `You are not 4 steps away from being a developer.`
- confirm your password/pin/pattern

#### 2. Enable USB debugging ####

- navigate to `Settings` > `System` > `Developer options`
- scroll down to `Debugging`
- enable `USB debugging`

#### 3. Download and install [Android SDK Platform-Tools](https://developer.android.com/tools/releases/platform-tools) on your PC ####

- Linux
  - Ubuntu: 
    ```
    sudo apt-get update
    sudo apt install android-tools-adb
    ```
  - Arch: 
    ```
    sudo pacman -S  android-tools
    ```
  - Fedora: 
    ```
    sudo dnf install android-tools
    ```
  - Other: https://dl.google.com/android/repository/platform-tools-latest-linux.zip
- Windows 
  - https://dl.google.com/android/repository/platform-tools-latest-windows.zip
- Mac
  - https://dl.google.com/android/repository/platform-tools-latest-darwin.zip

#### 4. Connect to device ####

- connect PSG1 and PC via USB-C cable 
- (optional) list adb devices to trigger dialog on device
    ```
    $ adb devices
    List of devices attached
    PS01-2538-001-A1-001331 unauthorized
    ```
- confirm the dialog `Allow USB-Debugging?`
  - (optional) check `Always allow from this computer` 
  - click `Allow`
- confirm the connection
    ```
    $ adb devices
    List of devices attached
    PS01-2538-001-A1-001331 device
    ```

### II. Install APKs from PC (USB)

APKs can now be installed from a PC using adb. Move forward to the next chapter to enable installing apks from the device itself.

####  Transfer and install apk to device ####

- push apk via adb
    ```
    $ adb push somepackage.apk /data/local/tmp/
    somepackage.apkk: 1 file pushed, 0 skipped. 100.0 MB/s (10000 bytes in 0.001s)
    ```
- install
    ```
    $ adb shell pm install --user 0 -i com.playsolana.echos /data/local/tmp/somepackage.apk
    Success
    ```

####  Install apk from sdcard ####

- copy apk to sdcard
- install
    ```
    $ adb shell pm install --user 0 -i com.playsolana.echos /sdcard/somepackage.apk
    Success
    ```

### III. Inital Setup to install from device (Wireless)

To enable apk installs from the device we need these things:
- [Shizuku](https://github.com/rikkaapps/shizuku) - Enable from normal apps to use adb
- `Wireless Debugging` - enable Shizuku to use adb
- [PSG1-PackageInstaller](https://github.com/peppergrayxyz/PSG1-PackageInstaller) - use adb to install apks

The device must be connected to Wifi for this work.

#### 1. Install Shizuku ####

- download the latest shizuku apk: https://github.com/RikkaApps/Shizuku/releases
  - at the time of writing the latest is `shizuku-v13.6.0.r1086.2650830c-release.apk`. Use the filename you downloaded.
- push to device: 
  ```
  $ adb push shizuku-v13.6.0.r1086.2650830c-release.apk /data/local/tmp/
  shizuku-v13.6.0.r1086.2650830c-release.apk: 1 file pushed, 0 skipped. 284.8 MB/s (2571773 bytes in 0.009s)
  ```
- install: 
   ```
   $ adb shell pm install --user 0 -i com.playsolana.echos /data/local/tmp/shizuku-v13.6.0.r1086.2650830c-release.apk
   Success
   ```

#### 2. Setup Wirless Debugging ####

- navigate to `Setting` -> `Apps` -> `Shizuku` and click the open symbol in the top right corner
- in the section `Start via Wireless Debugging` -> click on `Pairing` 
- check that the notification pops up: `Shizuku - Searching for paring service`
- click on `Developer Options`
- scroll to `Debugging` -> click on the text `Wirless Debugging`
- enable `Use Wirless Debugging` #
- in the pop-up `Allow wirless debugging on this network`
  - check `Always allow on this network`
  - click `Allow`
- click on `Pair device with pairing code`
- memorize the 6 digit code
- click the notification `Shizuku - Pairing service found` -> `Enter pairing code` -> Ok
- confirm success: `Shizuku: Pairing successfull. You can start service now.`
 

#### 3. Install PSG1-PackageInstaller ####

- download the latest PSG1-PackageInstaller apk: https://github.com/peppergrayxyz/PSG1-PackageInstaller/releases
- push to device: 
  ```
  $ adb push psg1_packageinstaller.apk /data/local/tmp/
  psg1_packageinstaller.apk: 1 file pushed, 0 skipped. 115.1 MB/s (433122 bytes in 0.004s)
  ```
- install: 
   ```
   $ adb shell pm install --user 0 -i com.playsolana.echos /data/local/tmp/psg1_packageinstaller.apk
   Success
   ```
- navigate to `Setting` -> `Apps` -> `PSG1 Shizuku Installer` and click the open symbol in the top right corner
- `Shizuku is not permitted` -> `Ok`
- `Allow PSG1 Shizuku Installer to access Shizuku` -> `Allow all the time`

### IV. Install APKs install from device (Wireless)

Note: You cannot install APKs through appstores (F-Droid, Aurora, etc.), because they install apps how they are supposed to do via the android package installer, which is blocked on this device. You need to download the apk bevore installing it.

- you can now install APKs either by:
  - navigate to `Setting` -> `Apps` -> `Files` -> select the apl to install
  - navigate to `Setting` -> `Apps` -> `Chromium` -> download apk
- when prompted `Open with`
  - select `PSG1 Shizuku Installer`
  - `Always`
- `Ìnstall`

enjoy (:


## Like this?

☕ consider [buying me a coffee](https://buymeacoffee.com/peppergrayxyz) (: