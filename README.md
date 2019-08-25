# Android Custom ROM Building Guide
This guide offers a simple guide on how to build a custom ROM for Android. However, simple can be harder than complex: You have to work hard to get your thinking clean to make it simple. But it’s worth it in the end because once you get there, you can move mountains.
Note: This guide assumes that you know how to use Linux fluently.

### Requirements And Setting Up
Click this link [here](https://source.android.com/setup/build/requirements) from the official AOSP website to see if your computer meets the hardware and software requirements needed to build ROMs. Also, click this link [here](https://source.android.com/setup/build/initializing) to set up your build environment properly.

### How To Build Stock AOSP ROMs
Click [here](https://source.android.com/setup/build/downloading) to download Repo. Now, while following the instructions, it might tell you to enter the following command or something similar if you are downloading a specific version of Android rather than the latest version:

```sh
repo init -u https://android.googlesource.com/platform/manifest -b android-4.0.1_r1
```

Take note that the following example tells Repo to initialise the AOSP repository from the "android-4.0.1_r1" branch. If you want to download another branch of Android, click [here](https://source.android.com/setup/start/build-numbers#source-code-tags-and-builds) or the link provided in the instructions. After that, copy the branch name from the appropriate row from the table into the command and run it. You should have configured Git if you followed the instructions carefully, or this command results in an error. If you do not want Google to know your name and username, feel free to enter a placeholder name and email address just like how the examples from the instructions show. When asked whether you want to enable colour display in this user account, feel free to enter "yes", as it does not matter.

After running "repo init", continue to follow the instructions and run "repo sync". Note that depending on your network speed, you might want to set the number of threads that should be utilised to synchronise the repo. The default option (without specifying any arguments) would be four threads. If you have a slower internet connection, you can lower it down to two, or if you have a faster network connection, feel free to raise that number. For example, to set the number of threads to allocate for download to two:

```sh
repo sync -j2
```

This process takes about an hour or more, so feel free to grab a cup of coffee, get on with your day while having this process running in the background on your computer.

Note: To prevent your computer from sleeping, download a software called "Caffeine", which engages a wakelock to prevent your computer from sleeping so that the computer does not disrupt the process automatically.

Now, if you start building Android, the process goes on smoothly with no errors, but if you flash the ROM onto your phone, you will find out that your phone is unable to boot. That is because if we look at the build directory, you find that there is no "vendor" folder there, as it does not automatically download the proprietary binaries for you. That particular folder is essential as it contains all the necessary drivers specific to your device to allow it to run. To download those files, head over to [TheMuppets repository](https://github.com/TheMuppets) and there, you find the proprietary binaries of many major phone OEMs there in separate repositories. If you are building for a Google Pixel device, head over to the repository named "proprietary_vendor_google".

Note: If you are building for a Nexus device, most of these devices are not built by Google as Google only provides the software, while other OEMs build the physical device for them. Therefore, most components that are in Nexus devices are put together by them, so head over to their repositories instead and look for your device codename.

Now head over to the correct branch (select the Lineage version that corresponds to the Android version you are building). There are multiple methods of downloading the proprietary binaries, but the best method is to create a local manifest so that every time you run "repo sync" the proprietary binaries is automatically updated together with the source code. So now head over to <BUILD_DIRECTORY>/.repo/manifests (make sure you enable the option to show hidden files) and create a .xml file named "local_manifest.xml" (though you can create a .xml file with whatever name you like). After that, copy the following in (and modifying whatever parts that need to be changed):

```
<?xml version="1.0" encoding ="UTF-8"?>
<manifest>
  <project name="TheMuppets/proprietary_vendor_google" path="vendor/google" remote="github" revision="lineage-16.0" />
</manifest>
```

Now run "repo sync" again and the necessary files should be downloaded.

After you are done downloading proprieatry binaries, click [here](https://source.android.com/setup/build/building) and follow the instructions to start the process of building Android. Now, the instructions might want you to run the following command:

```sh
lunch aosp_arm-eng
```

However, since you are not familiar with all the device codenames, run the command without any attributes instead:

```sh
lunch
```

The command prompt will present you with a list of devices that you can build for. Just enter the appropriate option number and press enter. Now this command will just set up the build environment and not start building Android until you run the "make" command further down the road.
Note: There are multiple varients of the ROM that you can build for a device. For more information, at the same webpage, there is a table labeled "buildtype" which will tell you more about what ROM varient is best suited for your intention of building your ROM.
After this command has completed, you can run the following command below:

```sh
make -j4
```

Note: As you can see, you can specify the number of threads to use above, just like the "repo sync" command. However, the number of threads you should use is now wholly dependent on the amount of resources you should allocate to your CPU, rather than going by your internet speed, as we are well past of downloading source code from the internet and are now building Android, which is a very CPU intensive task, so your computer is going to get real toasty really fast. So set the appropriate number of threads based on how many cores your CPU has. The more the number of CPU cores, the more the amount of threads you can allocate. The default option (without specifying any arguments) is four threads. Also, this process is going to take a long while (just like the "repo sync" command earlier), so feel free to leave your computer for now. Lastly, throughout the build process, there might be warnings that will be shown in the terminal. Most of the time, you do not have to worry about those, until errors occur.

Best practices:

- Minimum number of threads you can allocate: Number of CPU(s) * Number of cores per CPU * Number of threads per core (most likely 2)

- Maximum number of threads you can allocate: Minimum number of threads * 2

After the build has completed enter <BUILD_DIRECTORY>/out/target/product/<DEVICE_CODENAME> and you will see a bunch of .img files. You can flash them with Fastboot. However, if you want to convert those files into a single .zip file which can be flashed in recoveries such as TWRP, you will need to run the following command below:

```sh
make otapackage
```

This will generate a .zip file in the same directory as mentioned above. This process will take a shorter time to execute as you should have the necessary files required after building to flash a functioning ROM into a device, so the program is smart enough to not build Android again. With that said, if you do not want to create any .img files but generate a .zip file directly, you can run "make otapackage" instead of the "make -jX" command mentioned above.
