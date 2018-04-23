# Get UDID, Find UDID, Get IMEI, Find IMEI in simple way
How to get UDID? Open the site [http://getudid.co](http://getudid.co) in Safari on your iPhone or iPad to find UDID, IMEI, MEID, Serial Number, Device Version

This method uses Over-the-Air Profile Delivery Concepts (https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/iPhoneOTAConfiguration/OTASecurity/OTASecurity.html)

## profile service payload (.mobileconfig)
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple Inc//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
   <dict>
      <key>PayloadContent</key>
      <dict>
         <key>DeviceAttributes</key>
         <array>
			 <string>UDID</string>
			<string>IMEI</string>
			<string>MEID</string>
			<string>ICCID</string>
			<string>VERSION</string>
			<string>PRODUCT</string>
			<string>MAC_ADDRESS_EN0</string>
			<string>DEVICE_NAME</string>
			<string>SERIAL</string>
			<string>IMSI</string>
			<string>ECID</string>
         </array>
         <key>URL</key>
         <string>https://getudid.co/retrieve</string>
      </dict>
      <key>PayloadOrganization</key>
      <string>getudid.co</string>
      <key>PayloadDisplayName</key>
      <string>Get Your UDID</string>
      <key>PayloadIdentifier</key>
      <string>co.getudid.profile-service</string>
      <key>PayloadDescription</key>
      <string>Install this temporary profile to find and display your current device's UDID. It is automatically removed from device right after you get your UDID.</string>
      <key>PayloadType</key>
      <string>Profile Service</string>
      <key>PayloadUUID</key>
      <string>cfbe4c38-43ca-4069-a625-1e993885a2ca</string>
       <key>PayloadVersion</key>
      <integer>1</integer>
   </dict>
</plist>
```
Create SSL certificate (Suggest: https://www.sslforfree.com/) . We have 3 files. (ca_bundle.crt, private.key, certificate.crt).
Use those files to sign above .mobileconfig with this command:
```
```
