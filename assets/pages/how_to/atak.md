# Add better maps to ATAK
https://github.com/joshuafuller/ATAK-Maps

# Run ATAK on linux via Android Emulator
    Download Android Studio. https://developer.android.com/studio?gclid=Cj0KCQjw8NilBhDOARIsAHzpbLAKvp6Hiaqle-OtPJcqJjAd6mYwU40j2I0luxBZnrOuag-7XunVPXMaApGjEALw_wcB&gclsrc=aw.ds
    Download ATAK apk. https://tak.gov/products/atak-civ?product_version=atak-civ-4-9-0
        You may need to sign up as developer to gain access
    Run Android Studio
        Unpack android studio tar.gz
        cd into android studio folder
        execute studio.sh
            bash android-studio/bin/studio.sh
    In the "More Actions" section click device manager
        Here you can emulate different Android devices
        In my case, I created a custom device to the size of my laptop screen with plenty of RAM
    Run desired Android Device
    Install the ATAK apk file to the emulated device by dragging and dropping it
    Once installed you can run ATAK on the emulated device
        You must give all permissions to the app
    Add better maps

Networking the emulated device:
Uninstalling an android app: https://www.makeuseof.com/uninstall-android-app-adb-system-apps-bloatware/