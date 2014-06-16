Step by Step for Intergration and streaming

```
# yum update -y

# yum install java* -y

# wget http://www.wowza.com/downloads/WowzaMediaServer-3-6-4/WowzaMediaServer-3.6.4.rpm.bin

# chmod +x WowzaMediaServer-3.6.4.rpm.bin

# cd /usr/local/WowzaMediaServer/bin/

# ./startup.sh 

# service WowzaMediaServer status

# service WowzaMediaServer start

# mkdir -p /usr/local/WowzaMediaServer/conf/oflaDemo

# mkdir -p /usr/local/WowzaMediaServer/applications/oflaDemo

# cp /usr/local/WowzaMediaServer/conf/vod/Application.xml /usr/local/WowzaMediaServer/conf/oflaDemo/Application.xml

# vim /usr/local/WowzaMediaServer/conf/oflaDemo/Application.xml

# ln -s /opt/kaltura/web/content /opt/kaltura/web/content/webcam

# ln -s /opt/kaltura/web/content /usr/local/WowzaMediaServer/content

# vim /opt/kaltura/app/batch/batches/Provision/Engines/KProvisionEngineAkamai.php
```
From line 91 to 123 comment using /*$flashLiveStreamInfo  upto 123 line */

just use /* at starting and at end */

Add these Line's Below 123 line

```
// code added by babin
$data->streamID = 'livestream';
$data->backupStreamID = $data->streamID;
$data->streamName = $job->entryId . '_%i@' . $data->streamID;
$data->rtmp = 'rtmp://198.19.76.41:1935/live';
$data->primaryBroadcastingUrl = 'rtmp://198.19.76.41:1935/live';
$data->secondaryBroadcastingUrl = 'rtmp://198.19.76.41:1935/live';
$data->encoderUsername = '';
```

Edit the file 

```
# vim /opt/kaltura/app/alpha/apps/kaltura/modules/extwidget/actions/streamclipperAction.class.php

```

Comment the line 23 using //


```
//              $streamer = $entry->getStreamUrl();
```

Below this add the line 

```
$streamer = 'rtmp://198.19.76.41:1935/live';

```
Restart the Service 

```
service WowzaMediaServer restart
```

Then Edit the File 

```
vim /opt/kaltura/app/alpha/config/kConfLocal.php

```

Replace the 18th line with ip address and port number as shown below
Instead of hostname put the ip and port

```
"rtmp_url" => "198.19.76.41:1935/oflaDemo",

```

Edit the file 

```
# vim /opt/kaltura/app/alpha/apps/kaltura/modules/extwidget/actions/playManifestAction.class.php

```

In Line 593 comment using // and paste below as 

```
$baseUrl = "rtmp://198.19.76.41:1935/oflaDemo";
```

Command from 608 to 612 using //

This all need to commented

```
      /* @var $flavorAsset flavorAsset */

                                        //      $urlManager->setClipTo($this->clipTo);
                                        //      $urlManager->setFileExtension($flavorAsset->getFileExt());
                                        //      $urlManager->setProtocol(StorageProfile::PLAY_FORMAT_RTMP);
                                        //      $url = $urlManager->getFlavorAssetUrl($flavorAsset);
```
Then Below this Enter the following content 
From 617 to 625

```
$key = $flavorAsset->getSyncKey(flavorAsset::FILE_SYNC_FLAVOR_ASSET_SUB_TYPE_ASSET);
$fileSync = kFileSyncUtils::getLocalFileSyncForKey($key);
if(!$fileSync)
continue;

$urlManager->setClipTo($this->clipTo);
$urlManager->setFileExtension($flavorAsset->getFileExt());
$urlManager->setProtocol(StorageProfile::PLAY_FORMAT_RTMP);
$url = $urlManager->getFileSyncUrl($fileSync);
```

Edit the file

```
vim /opt/kaltura/web/content/uiconf/kaltura/samplekit/kcw_2.6.4/kcw_samplekit.xml

```

Replace the 25 line with ip as below shown

```
<serverUrl>rtmp://198.191.76.41:1935/oflaDemo</serverUrl>

```


