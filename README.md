
## 📚 Delphi Android Tips:

Welcome to **Delphi Android Tips**, a plain-text guide with essential tips for building and improving Android apps using Delphi. This guide includes tips based on our previous discussions to help you solve specific issues.

---

<details>
<summary>Tip1: Fixing Stretched Splash Images (After using the ArtWork generator from Delphi IDE) 🌟</summary>


 **Problem:**  
 If you've ever generated Android splash screen images using Delphi IDE and noticed they appear stretched,  
 here's a simple way to fix that and ensure your splash image is always centered without distortion.

 <img width="260" alt="Test" src="https://github.com/user-attachments/assets/9d142610-8a44-4b14-9f3e-e196defa8aed" />

 **Solution:**  
 you can see the video tutorial on my youtube channel [here](https://www.youtube.com/watch?v=8CQirJtW390)  
 ----  
 To fix the stretched splash images, follow these steps:

==> **Create a custom splash image definition:**

After building your project, go to the following paths where the splash screen files are generated:
   ```xml
      if your target android system is 64bit:
      <YourProjectDirectory>\Android64\Debug\<YourProjectName>\res\drawable
      <YourProjectDirectory>\Android64\Debug\<YourProjectName>\res\drawable-anydpi-v21  
        or
      <YourProjectDirectory>\Android\Debug\<YourProjectName>\res\drawable  
      <YourProjectDirectory>\Android\Debug\<YourProjectName>\res\drawable-anydpi-v21   
   ```
  
1. **Copy both files** **`splash_image_def.xml | splash_image_def-v21.xml`** from this folder and paste
it into a new directory in your project (e.g., **`<YourProjectDirectory>\res\theme`**).  
  
   1.2  **Open both files in Delphi IDE** and add the following line inside each file:  
      ```pascal
      android:scaleType="centerInside"
      ```
   1.3 **Deployment:**  
         Go to Project > Deployment in Delphi IDE.
      Select all configurations for your target system.
      Click on the column header "Local Name" to sort the list by name.
      Scroll down, find the default splash xml files, uncheck them, and replace them with your newly edited files.
      Don’t forget to set the remote path for the new files according to the unchecked ones.
       That’s it! **Clean&Rebuild** and deploy your project, and you’ll see your splash image properly centered on all devices without any stretching! 

**Finally,Modified splash_image_def.xml should look like this:**  
```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android" android:opacity="opaque">
  <item android:drawable="@color/splash_background" />
  <item>
      <bitmap
          android:src="@drawable/splash_image"
          android:antialias="true"
          android:dither="true"
          android:filter="true"
          android:gravity="center"
          android:scaleType="centerInside"
          android:tileMode="disabled"/>
  </item>
</layer-list>
```
**Modified splash_image_def-v21.xml should look like this:**  
```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@color/splash_background" />
    <item
        android:gravity="center"
        android:scaleType="centerInside"
        android:drawable="@drawable/splash_vector">
    </item>
</layer-list
 ```
  
  
This should solve the stretching issue and provide a visually appealing splash screen for your users.  

<img width="224" alt="Test Fixed" src="https://github.com/user-attachments/assets/2e20c459-f165-4a3e-a72b-cc526ca05140" />

---

## Closing Note:  

We hope this tip helps you improve your Delphi Android app development experience. Stay tuned for more tips!

Happy coding! 🚀

</details>

<details><summary>Tip2: How to use ADB over wireless if your phone support only Usb Debug Mode Option 🌟</summary>
# ADB Wireless Setup Guide
## Switch from USB to Wireless Debug (Android 10 and Lower)

### Why Go Wireless?

Tired of buying new USB debug cables every few months because they stop working reliably? This guide helps you:
- **Save money** on replacement cables
- **Extend cable lifespan** by reducing wear and tear
- **Work more freely** without being tethered to your computer
- **Avoid USB connection issues** like driver problems

---

### Prerequisites

✓ Working USB cable (just for initial setup)  
✓ ADB installed on your computer  
✓ Phone and computer on the **same Wi-Fi network**  
✓ Developer options enabled on your phone

---

### Quick Setup Steps

#### 1. **Enable Developer Options** (if not already enabled)
   - Go to **Settings > About Phone**
   - Tap **Build Number** 7 times
   - Enter your PIN/password if prompted

#### 2. **Enable USB Debugging**
   - Go to **Settings > Developer Options**
   - Turn on **USB Debugging**

#### 3. **Connect USB Cable** (one-time setup)
   - Plug your phone into your computer
   - Accept the "Allow USB Debugging" prompt on your phone
   - Check the "Always allow from this computer" box

#### 4. **Switch to Wireless Mode**

   Open terminal/command prompt and run:

   ```bash
   # Tell ADB to listen on port 5555
   adb tcpip 5555
   ```

   You should see:
   ```
   restarting in TCP mode port: 5555
   ```

#### 5. **Find Your Phone's IP Address**

   **Option A** - From your phone:
   - Go to **Settings > About Phone > Status**
   - Look for **IP Address** (something like 192.168.1.xxx)

   **Option B** - Using ADB:
   ```bash
   adb shell ip addr show wlan0
   ```

#### 6. **Disconnect USB Cable**
   
   Now you can safely unplug the cable after running this command bellow successfully..

#### 7. **Connect Wirelessly**

   ```bash
   # Replace with your phone's actual IP
   adb connect 192.168.1.xxx:5555
   ```

   Success looks like:
   ```
   connected to 192.168.1.xxx:5555
   ```

#### 8. **Verify Connection**

   ```bash
   adb devices
   ```

   You should see:
   ```
   List of devices attached
   192.168.1.xxx:5555    device
   ```

---

### 🎉 You're Done!

Your phone is now connected wirelessly. You can:
- Install apps: `adb install app.apk`
- Run shell commands: `adb shell`
- View logs: `adb logcat`
- Debug apps in Android Studio

---

### Troubleshooting

**Connection lost?**
- Make sure phone and computer are on the same Wi-Fi
- Run `adb connect IP:5555` again
- If that fails, reconnect USB and repeat steps 4-7

**Can't find IP address?**
```bash
# While USB connected:
adb shell ip -f inet addr show wlan0
```

**Reset and start over:**
```bash
adb kill-server
adb start-server
```

**Firewall blocking connection?**
- Temporarily disable firewall or add ADB exception for port 5555

---

### Quick Reconnect Script

Save this for easy reconnection:

**Windows (reconnect.bat):**
```batch
@echo off
adb connect 192.168.1.xxx:5555
adb devices
pause
```

**Mac/Linux (reconnect.sh):**
```bash
#!/bin/bash
adb connect 192.168.1.xxx:5555
adb devices
```

Make it executable: `chmod +x reconnect.sh`

---

### Pro Tips

💡 **Keep one good USB cable as backup** for initial setup when needed

💡 **Bookmark your phone's IP** or use a static IP in router settings

💡 **Add quick toggle to developer tiles** (Settings > Developer Options > Quick Settings Developer Tiles)

💡 **Battery consideration**: Wireless debugging can drain battery faster when active

💡 **Security**: Only enable when needed, disable wireless debugging when done

---

### For Android 11+

If your phone has Android 11 or higher, check if it has the built-in **Wireless Debugging** option:

1. **Settings > Developer Options > Wireless Debugging**
2. Toggle it ON
3. Tap **Pair device with pairing code**
4. On computer: `adb pair IP:PORT` (use pairing code shown)
5. Then: `adb connect IP:PORT`

This method doesn't require an initial USB connection!

---

### Cable Care Tips

To make your USB cables last longer:

✅ Don't bend cables sharply  
✅ Unplug by pulling the connector, not the cable  
✅ Avoid excessive plugging/unplugging  
✅ Use wireless connection for daily debugging  
✅ Reserve cable for initial setup or emergencies

---

**Remember**: This wireless connection needs to be re-established if your phone:
- Reboots
- Switches Wi-Fi networks  
- Goes into airplane mode
- IP address changes

Just run `adb connect IP:5555` again to reconnect!

</details>
