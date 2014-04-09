How to add a iphone flavor in Trancoding Profile
===============================================

######First we need to Start a Seesion 

* Navigate to **Developer** TAB and enter the client tag ( your **User Secret**: ) from kaltura user **Integration** settings Tab

* Select Service and Choose **Session:**

* Select action and Choose **start**

* Select Secret and paste the string, get it from **Developer** TAB and enter the client tag ( your **Admin Secret**: ) from kaltura user **Integration** settings Tab

* Select User String and Paste the ( your **User Secret**: )

* Choose Type as Admin

* Partner ID want to be your Partner ID which go Going to Create 

* Expiry and Privileges are optional.

You Will get a KS Seesion like Below


```
<xml>
<result>MDE3YWVmNWZlMmY3ODc2NDAxODA2ZDUzODFkMTU3ZDg5NGZjYTVjZnwxMDE7MTAxOzEzOTcxMTM2ODg7MjsxMzk3MDI3Mjg4LjM2MTk7NTA1ZTM2ZTVlMTE5YjQ0Yjk0M2RjN2Y1OGJkY2QwYmQ7Ozs=</result>
<executionTime>5.1021575927734E-5</executionTime>
</xml>
``` 

In above Output, this one is the ks Session ID


```
MDE3YWVmNWZlMmY3ODc2NDAxODA2ZDUzODFkMTU3ZDg5NGZjYTVjZnwxMDE7MTAxOzEzOTcxMTM2ODg7MjsxMzk3MDI3Mjg4LjM2MTk7NTA1ZTM2ZTVlMTE5YjQ0Yjk0M2RjN2Y1OGJkY2QwYmQ7Ozs=
```

Copy that KS Session ID for Use while creating new flavour, And start a New Test console


* Log in to your admin console, and start a new session in the Developer => Test Console by entering the admin secret key and partner ID of the publisher for which you wish to enable the iPhone format transcoding. You can get your admin secret key by logging into your KMC with the desired publisher and going to Settings => Integration Settings

* Next, use the flavorParams => Add service (make sure you check the box next to your KS so it is a valid session!) and enter the following into the flavorParams box for the various settings (you have to click on flavorParams for it to open first)

* flavorParams:name - iphonemp4
* flavorParams:description - iphone mp4 format
* flavorParams:tags - iphone,mp4
* flavorParams:format - mp4
* flavorParams:videoCodec - h264
* flavorParams:audioCodec - AAC
* flavorParams:conversionEngines - 2
* ALL other values should be left BLANK!
You can verify that it has taken by using the flavorParams => List service

* Then Under Transcoding we can see the Above added format 

