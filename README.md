# Android Custom ROM Building Guide
This offers a simplistic guide on how to build a custom ROM for Android. However, simple can be harder than complex: You have to work hard to get your thinking clean to make it simple. But it’s worth it in the end because once you get there, you can move mountains.
Note: This guide assumes that you know how to use Linux fluently.

### Requirements And Setting Up
Click this link [here](https://source.android.com/setup/build/requirements) from the official AOSP website to see if your computer meets the hardware and software requirements needed to build ROMs. Also click this link [here](https://source.android.com/setup/build/initializing) to properly set up your build environment.

### How To Build Stock AOSP ROMs
Click [here](https://source.android.com/setup/build/downloading) to download Repo. Now, while following the instructions, it might tell you to enter the following command or something similar if you are downloading a specific version of Android rather than the latest version:
```sh
repo init -u https://android.googlesource.com/platform/manifest -b android-4.0.1_r1
```
Take note that the following example tells Repo to initialise the AOSP repository from the "android-4.0.1_r1" branch. If you want to download another branch of Android, click [here](https://source.android.com/setup/start/build-numbers#source-code-tags-and-builds) or the link provided in the instructions. After that, just copy the branch name from the appropriate row from the table into the command and run it. You should have configured Git if you followed the instructions closely, or this command will result in an error. If you do not want Google to know your name and username, feel free to enter a placeholder name and email address just like how the examples from the instructions show. When asked whether you want to enable colour display in this user account, feel free to enter "yes", and that it does not really matter anyway.
After running "repo init", continue to follow the instructions and run "repo sync". Note that depepnding on your network speed, you might want to set the number of threads that should be utilised to synchronise the repo. The default option (without specifying any arguments) would be four threads. If you have a slower internet connection, you can lower it down to two, or if you have a faster network connection, feel free to raise that number. For example, to set the number of threads to allocate for download to two:
```sh
repo sync -j2
```
This process will take about an hour or more, so feel free to grab a cup of coffee, get on with your day while having this process running in the background in your computer.
Note: To prevent your computer from sleeping, download a software called "Caffeine", which will engage a wakelock to prevent your computer from sleeping so that the process will not be disrupted.
After this command is done running, click [here](https://source.android.com/setup/build/building) and follow the instructions to start the process of building Android. Now, the instructions might want you to run the following command:
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
Note: As you can see, you can specify the number of threads to use above, just like the "repo sync" command. However, the number of threads you should use is now wholly dependent on the amount of resources you should allocate to your CPU, rather than going by your internet speed, as we are well past of downloading source code from the internet and are now building Android, which is a very CPU intensive task, so your computer is going to get real toasty really fast. So set the appropriate number of threads based on how many cores your CPU has. The more the number of CPU cores, the more the amount of threads you can allocate. The default option (without specifying any arguments) is four threads. Also, this process is going to take a long while (just like the "repo sync" command earlier), so feel free to leave your computer for now.

Best practices:

-Minimum number of threads you can allocate: Number of CPU(s) * Number of cores per CPU * Number of threads per core (most likely 2)
-Maximum number of threads you can allocate: Minimum number of threads * 2
