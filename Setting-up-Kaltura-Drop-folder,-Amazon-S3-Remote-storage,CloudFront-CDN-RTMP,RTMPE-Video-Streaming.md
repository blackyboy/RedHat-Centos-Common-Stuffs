### Setting up a Drop Folder

After Creating a Publisher in Kaltura 

1. Creat a Transcoding Profile ( I have created in my name as blackyboy transcoding 

2. Then Configure DropBox for Publisher by choosing configure in Drop menu

3. And Tick the check box, Content Ingestion - Drop Folder/s (config)

4. Then Click on configure and change the settings.

Note your Publisher ID from user's list ( My Publisher ID 102) and Create using Type : Local

Drop Folder Name: our Wish ( Here i have used blackyboy)

Description: As our Wish (This is blackyboy's Drop)

5. Conversion Profile ID: Choose your created name Here from Drop list

6. Drop Folder Storage Path: /opt/kaltura/web/content/blackyboy (or) any folder name

Check file size every (seconds): 10

7. Choose Manual Deletion if you Don't want to delete the Source.

Save it ... that't it in KMC side..

-------------------------------------------------------------------------

### Then in Terminal 

1. Create a directory named as you have mentioned here (Drop Folder Storage Path: /opt/kaltura/web/content/blackyboy)

```
Eg : mkdir /opt/kaltura/web/content/blackyboy
```

2. Then add a user for FTP

```
# useradd -d /opt/kaltura/web/content/blackyboy blackyboy  ( home Dir of this blackyboy user is /opt/kaltura/web/content/blackyboy )
```
(skel file error will be display, we don't need a bash profile so don't mind the error)

Create a password for the user which we have created for Drop

```
# passwd blackyboy

New passwd: ********
Con Passwd: ********
```

3. Add the user blackyboy to apache & kaltura Group
   Only kaltura Group is Enough

```
# usermod -a -G apache,kaltura blackyboy

```

4.Navigate to directory 

```
# cd /opt/kaltura/web/content
```

5. Change the Ownership of blackyboy

```
# chown blackyboy:kaltura blackyboy/

```
Note : Here i have setuped for sftp because ftp is not secured one, If we need ftp just 2 more step to be added in above steps, those are 

```
# usermod -a -G ftp,kaltura blackyboy

```

And at last we need to restart the vsftpd Service 

```
# /etc/init.d/vsftpd restart
```

6. Login the sftp from filezilla 

And upload a video file, it will be uploaded to /opt/kaltura/web/content/blackyboy

After Completing upload it wait's for 10 seconds and it will move to KMC Content TAB and Start to convert it Using Transcoding profile Which we have created.

We can see the Progress of uploading from (Drop folder) Under Content TAB 

That's it ..

Which we have uploaded from filezilla will be converted and stored in S3, if we follow the below step's


### Setup Amazon S3 Remote storage

TO Setup a Remote storage i used the following Link, Some content's are added by me too

Reference URL : [kalturaCE Amazon s3 storage cloudfront cdn setup](http://www.panda-os.com/2012/11/kaltura-ce-amazon-s3-storage-cloudfront-cdn-setup/#.Uy_7KHUW3h_)

Setting up Amazon S3 and getting security credentials

1. To get your Amazon security credentials (assuming you have an account with amazon AWS), go to this link https://portal.aws.amazon.com/gp/aws/securityCredentials

2. To set up your amazon S3 bucket, go to https://console.aws.amazon.com/s3/home , create a new bucket, and name it.

3. Inside this bucket, create a folder called “kaltura”

4. Select your new bucket on the left side, click Actions and select “Properties”

5. Add more permissions – Authenticated Users – check all boxes.

6. Select the kaltura folder, click properties, go to Permissions.

7. Add more permissions – Everyone – read and download (you can also right click the folder and select “Make Public”)

In the Above 6th and 7th Step there is no Permission available, So Just Right click on kaltura Directory and choose "Make Public"

Then we need to add a bucket Policy for your bucket, Granting Object get Permission to any Anonymous User in Amazon S3 Bucket for reading the file.

```
{
  "Version":"2012-10-17",
  "Statement":[{
	"Sid":"AddPerm",
        "Effect":"Allow",
	  "Principal": {
            "AWS": "*"
         },
      "Action":["s3:GetObject"],
      "Resource":["arn:aws:s3:::Bucket-name/*"
      ]
    }
  ]
}
```


![bucket permission](https://github.com/blackyboy/Ubuntu-Linux-Stuffs/raw/master/kaltura-setup/Selection_014.png)


If this Policy was not added, We will face clip not found error when ever uploading a new video to kaltura.

### Setting up Amazon CloudFront CDN

1. Go to https://console.aws.amazon.com/cloudfront/home

2. Create a new “Distribution” of type “Web”, and Origin Domain Name select your bucket from list

3. Under Viewer Protocol Policy: Choose HTTP and HTTPS

4. Select your bucket as the origin ID, and decide wether you want logging or not.

5. Click on Create Distribution

6. Copy your CloudFront domain name (example: d2xxxxxxxxxxxx.cloudfront.net) for later use.


### Setting up the Remote Storage Profile in the Admin Console
First, you must enable the necessary configuration options for your partner:

1. Find your partner in the list of partners, click on the right drop down box and select “Configure”

2. Under “Remote Storage Policy”, set Delivery Policy to “Remote Storage Only”

3. Check the “Delete exported storage” checkbox.

4. Under Enable/Disable Features, make sure that “Remote Storage” is checked.

5. Click “Save”.

Next we must configure the Remote Storage Profile. In order to do this, we must click on the partner’s left drop-down box (under “Profiles”) and select “Remote Storage”. You should see the “Remote Storage Profiles” page for your publisher (If you haven’t yet set up any remote storage profiles, the list should be empty).

(Assuming that you have already set up an S3 bucket, and that you have an Access Key ID and a Secret Access Key)

1. Create a new profile by writing your publisher id in the right “Publisher ID” input box and clicking “Create New”.

2. Give a name to your Remote Storage (for example “Amazon S3″)

3. For “Storage URL” type http://{yourbucketname}.s3.amazonaws.com (replace {yourbucketname} with your bucket name on S3)

4. In Storage Base Directory, write “/{yourbucketname}/kaltura” (keep in mind the leading slash, and change yourbucketname to your bucket name)

5. Storage Username – enter your amazon aws api Access Key ID

6. Storage Password – paste your amazon aws api Secret Access Key

7. Under HTTP Delivery Base URL, type “http://{your amazon cloudfront domain}/kaltura” – replace {your amazon cloudfront domain} with the cloudfront domain you created in the previous section).

```
eg :	HTTP Delivery Base URL*:  http://d2xxxxxxxxxxxx.cloudfront.net/kaltura
	HTTPS Delivery Base URL:  https://d2xxxxxxxxxxxx.cloudfront.net/kaltura
```

![https](https://github.com/blackyboy/Ubuntu-Linux-Stuffs/raw/master/kaltura-setup/Selection_010.png)


8. Save the new Remote Storage Profile

Add a crossdomain.xml file
Create a crossdomain.xml file in the root of your S3 bucket

```
<cross-domain-policy xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="http://www.adobe.com/xml/schemas/PolicyFile.xsd">
    <allow-access-from domain="*" to-ports="*" secure="false"/>
    <site-control permitted-cross-domain-policies="all"/>
    <allow-http-request-headers-from domain="*" headers="*"/>
</cross-domain-policy>
```

[Crossdoman.xml](https://github.com/blackyboy/Ubuntu-Linux-Stuffs/blob/master/kaltura-setup/crossdomain.xml)


Final Step – Enable the remote storage profile

1. Click on the dropdown box next to your new storage profile in the Remote Storage Profiles page in Kaltura Admin Console

2. Select “Export Automatically” and then click “OK”

3. You will receive the confirmation that your storage was autoed :)

Test your new configuration
You can go ahead and test your new configuration. Upload a new video in the KMC, let it convert, and wait for it to get distributed. After that, try to play the entry and analyse it in your favorite sniffer. You should see that the movies are being downloaded from your cloudfront CDN, look for flv and mp4 files.

### Setting up Amazon CloudFront CDN for RTMP

1. Go to https://console.aws.amazon.com/cloudfront/home

2. Create a new “Distribution” of type “RTMP”, and Origin Domain Name select your bucket from list

3. Distribution State want to be Enabled

4. Click on Create Distribution

5. Copy your CloudFront RTMP domain name (example: s22xxxxxxxxxxxx.cloudfront.net) for later use.

![RTMP](https://github.com/blackyboy/Ubuntu-Linux-Stuffs/raw/master/kaltura-setup/Selection_015.png)

Next we need to configure the Remote Storage Profile. In order to do this, we must click on the partner’s left drop-down box (under “Profiles”) and select “Remote Storage”. You should see the “Remote Storage Profiles” page for your publisher (If you haven’t yet set up any remote storage profiles, the list should be empty).

There was our s3 storage will be listed as we have done in above Step, 

1. Select action Click configure 

2. Under Delivery Details Below http & https we need to enter the rtmp url of cloudnfront
Prefix must be our Directory which was created in s3 bucket
Note : There is no slash after /st
Note : There is no slash after /kaltura

```
RTMP Delivery Base URL:   rtmp://s22xxxxxxxxxxx.cloudfront.net/cfx/st

RTMP stream URL prefix:   /kaltura
```
![RTMPE](https://github.com/blackyboy/Ubuntu-Linux-Stuffs/raw/master/kaltura-setup/Selection_011.png)

3. Save the Remote Storage Profile

This will make works both RTMP & RTMPE Video Streaming Happy Streaming.


Bunch of thanks to jessp01 from Kaltura team for guiding me. 
