This cheetsheet include step to generate and export push notification certificate to be use by server. 
There are two ways we can .pem certificate 

- With PEM pass phrase and
- Without PEM pass phrase


First of all create a certificate for apns via developer.apple.com for your selected ios bundle.  
For more detail on how to create push notifiaction certificate
- [Generate push notification certificate: Using PEM file](https://www.appcoda.com/push-notification-ios/)
- [Generate push notification certificate: Using Authentication Key](https://www.raywenderlich.com/8164-push-notifications-tutorial-getting-started)

## In summary:
1. Login to your Apple Developer Account.
2. In the menu on the right, click on Certificates, IDs & Profiles.
3.Click on the + icon to add new certificate.
4. In the What type of certificate do you need? section, select Apple Push Notification service SSL (Sandbox & Production) and click on Continue.
5. In the next screen select the app that you want to create the certificate for and click on continue.
6. You will be given the instructions to create the CSR file on the next screen. Follow the instructions to create and download this file.
7. Click on continue and upload the CSR file that you downloaded.
8. You will then be allowed to download the certificate.
9. Double click on this certificate. This is will import the certificate to your keychain.

## With PEM pass phrase:
1. After importing, you will see the certificate as Apple push service. This option is expandable but don’t expand it. 
Just right click on it and click on export it. You will be asked to provide a password. 
This step require to provide PEM pass phrase when using to send notification.
2. This generates a p12 file. Convert it to pem using the following command in your mac terminal:
```bash
openssl pkcs12 -in Certificates.p12 -out newfile.pem
```

## Without PEM pass phrase:
1. Expand the Apple Push Services in Keychain. You will see Apple Push Service and Private key within it. 
We need to extract each individually.
2. First, export the certificate by right-clicking it and choosing "Export". 
Select a name (e.g. apns-cert.p12) and choose .p12 as format.
3. When prompted for a password, leave it blank. When prompted for a system password, provide your OSX user password.
4. Second,  export the private by right-clicking it and choosing "Export". 
Select a name (e.g. apns-key.p12) and choose .p12 as format.
5. When prompted for a password, leave it blank. When prompted for a system password, provide your OSX user password.


### combine apns-cert.p12 and apns-key.p12 into single .pem file
1. Convert apns-cert.p12 file into .pem file. When prompted for a password, simply press enter since no password 
should have been given when exporting from keychain.
```bash
openssl pkcs12 -clcerts -nokeys -out apns-cert.pem -in apns-cert.p12
```

2. Convert apns-key.p12 file into .pem file. When prompted for a password, simply press enter since no password 
should have been given when exporting from keychain.
```bash
openssl pkcs12 -nocerts -out apns-key.pem -in apns-key.p12
```

3. Remove encryption from key .pem file. Enter pass phrase from previous step when prompted to "Enter pass phrase.
```bash
openssl rsa -in apns-key.pem -out apns-key-noenc.pem
``` 

4. Merge certificate and key .pem file into one single .pem file.
```bash 
cat apns-cert.pem apns-key-noenc.pem > apns.prod.pem
```



## Sample Server side code for sending notification in php
- add below json in composer.json
```json
{
    "require": {
        "sly/notification-pusher": "^2.3"
    }
}
```


- code
```php
<?php

require_once 'vendor/autoload.php';

use Sly\NotificationPusher\PushManager,
    Sly\NotificationPusher\Adapter\Apns as ApnsAdapter,
    Sly\NotificationPusher\Collection\DeviceCollection,
    Sly\NotificationPusher\Model\Device,
    Sly\NotificationPusher\Model\Message,
    Sly\NotificationPusher\Model\Push
;

// First, instantiate the manager.
//
// Example for production environment:
$pushManager = new PushManager(PushManager::ENVIRONMENT_PROD);
//
// Development one by default (without argument).
//$pushManager = new PushManager(PushManager::ENVIRONMENT_DEV);

// Then declare an adapter.
$apnsAdapter = new ApnsAdapter(array(
    //Production Version
    'certificate' => 'apns/apns.prod.pem',
	//Development version
	//'certificate' => 'apns/apns.dev.pem',
));

// Set the device(s) to push the notification to.
$devices = new DeviceCollection(array(
    new Device('ab51092beeef6d0284b312bc343b5455ad5042451886678f02a3dceedca68328'),
    // ...
));

// Then, create the push skel.
//$message = new Message('This is a basic example of push.');
$message = new Message('You have a call from Rajan', array(
	'sound'=>'default',
	'custom'=>array('videoID'=>'1234567890', 'groupID' => 'groupID1234')
	));

// Finally, create and add the push to the manager, and push it!
$push = new Push($apnsAdapter, $devices, $message);
$pushManager->add($push);
$pushManager->push();

foreach($push->getResponses() as $token => $response) {
    print_r($response);
}
```






