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
openssl smime -sign -in unsigned.mobileconfig -out signed.mobileconfig -signer mbaike.crt -inkey mbaikenopass.key -certfile ca-bundle.pem -outform der -nodetach
```

## PHP Server
PHP code to read string sent from device to server
```
$data = file_get_contents('php://input');
$plistBegin   = '<?xml version="1.0"';
$plistEnd   = '</plist>';
$pos1 = strpos($data, $plistBegin);
$pos2 = strpos($data, $plistEnd);
$data2 = substr ($data,$pos1,$pos2-$pos1);
$xml = xml_parser_create();
xml_parse_into_struct($xml, $data2, $vs);
xml_parser_free($xml);
$UDID = "";
$MEID = "";
$SERIAL = "";
$DEVICE_PRODUCT = "";
$DEVICE_VERSION = "";
$IMEI = "";

$iterator = 0;
$arrayCleaned = array();
foreach($vs as $v){
	if($v['level'] == 3 && $v['type'] == 'complete'){
	$arrayCleaned[]= $v;
	}
$iterator++;
}
$data = "";
$iterator = 0;
foreach($arrayCleaned as $elem){
	$data .= "\n==".$elem['tag']." -> ".$elem['value']."<br/>";
	switch ($elem['value']) {
		case "MEID":
			$MEID = $arrayCleaned[$iterator+1]['value'];
			break;
		case "SERIAL":
			$SERIAL = $arrayCleaned[$iterator+1]['value'];
			break;
		case "PRODUCT":
			$DEVICE_PRODUCT = $arrayCleaned[$iterator+1]['value'];
			break;
		case "UDID":
			$UDID = $arrayCleaned[$iterator+1]['value'];
			break;
		case "IMEI":
			$IMEI = $arrayCleaned[$iterator+1]['value'];
			break;
		case "VERSION":
			$DEVICE_VERSION = $arrayCleaned[$iterator+1]['value'];
			break;                       
		}
		$iterator++;
}
```
