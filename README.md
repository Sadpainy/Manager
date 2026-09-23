# Manager

![Build](https://img.shields.io/badge/Build-passing-brightgreen?style=plastic&logo=githubactions&logoColor=white&labelColor=555555)
![Tests](https://img.shields.io/badge/Tests-passing-brightgreen?style=plastic&labelColor=555555)
![Java](https://img.shields.io/badge/Java-ED8B00?style=plastic&logo=openjdk&logoColor=white&labelColor=555555)
![License](https://img.shields.io/badge/License-AGPL_v3.0-blue?style=plastic&logo=gnu&logoColor=white&labelColor=555555)
![Android](https://img.shields.io/badge/Android-3DDC84?style=plastic&logo=android&logoColor=white&labelColor=3DDC84)

**Manager** is an Android app that shows what is running on your phone.

It displays the tasks, activities, and permissions of every app installed on your device, all in one place. It works without root access and does not require any special setup beyond enabling the accessibility service.

## What it shows

- The tasks currently running on your device
- The activities in each task and how they are structured
- The permissions each app has requested and whether they are granted
- A summary of every app, including how many tasks and activities it has

## What you can do

- Browse running tasks and activities in real time
- Inspect the permissions of any installed app
- See which permissions are dangerous and which are not
- Turn permissions on or off for apps that allow it
- Prevent screenshots and screen recording with a single toggle

## Who it is for

- Users who want to understand what apps are doing on their phone
- Developers who need to inspect tasks and permissions during testing
- Anyone who cares about privacy and wants a clear view of app permissions

## Usages

Push in System

```bash
# Access adb shell
adb shell

# Get Root
su

# Read-Write
mount -o remount,rw /system

# Copy In System
mkdir -p /system/app/Manager
cp /storage/emulated/0/Android/data/com.android.tasks/manager.apk /system/app/Manager/Manager.apk
chmod 644 /system/app/Manager/Manager.apk

# Copy In Privilege APP
mkdir -p /system/priv-app/Manager
cp /storage/emulated/0/Android/data/com.android.tasks/manager.apk /system/priv-app/Manager/Manager.apk
chmod 644 /system/priv-app/Manager/Manager.apk

# Read-Only
mount -o remount,ro /system

# Then, Shutdown.
reboot

# Last, Verify it.
adb shell pm list packages | grep com.android.tasks
adb shell dumpsys package com.android.tasks | grep -E "codePath|flags"

# Security, If you want backup.
adb pull /system/priv-app/Manager/Manager.apk /storage/emulated/0/backup/Manager.apk 2>/dev/null

adb shell su -c "mount -o remount,rw /system"
adb shell su -c "rm -rf /system/priv-app/Manager"
adb shell su -c "mount -o remount,ro /system"
adb reboot
```

For UserDebug Version

```bash
adb root
adb remount
adb push /storage/emulated/0/Android/data/com.android.tasks/manager.apk /system/priv-app/Manager/Manager.apk
adb shell chmod 644 /system/priv-app/Manager/Manager.apk
adb reboot
```

If Unsuccessful

```bash
adb root
adb shell mount -o remount,rw /system
adb push manager.apk /system/priv-app/Manager/Manager.apk
adb shell chmod 644 /system/priv-app/Manager/Manager.apk
adb shell mount -o remount,ro /system
adb reboot
```

Add Whitelist In System

```bash
cat > /system/etc/permissions/privapp-permissions-com.android.tasks.xml << 'EOF'
<?xml version="1.0" encoding="utf-8"?>
<permissions>
    <privapp-permissions package="com.android.tasks">
        <permission name="android.permission.DUMP"/>
        <permission name="android.permission.READ_LOGS"/>
        <permission name="android.permission.PACKAGE_USAGE_STATS"/>
        <permission name="android.permission.UPDATE_APP_OPS_STATS"/>
    </privapp-permissions>
</permissions>
EOF
chmod 644 /system/etc/permissions/privapp-permissions-com.android.tasks.xml
```

## Notes

- No root access is required
- No internet connection is used
- No data is collected or sent anywhere
- No advertisements, no tracking

## License

GNU Affero General Public License, version 3 and **LICENSE.Manager** `(Must-Read, Important!)`.
