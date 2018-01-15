# fis-ftp-demo
Requirements 
------------------------------
- Openshift CDK or minishift
- Maven
- JDK

Setup 
------------------------------
**Create new project in OCP**
```bash
oc login -u developer
oc new-app fis-ftp-demo
```

**Deploy pure-ftpd image**

Open the fis-ftp-demo project in the OCP web client

Click Add to Project > Deploy Image

Select Image Name and enter stilliard/pure-ftpd

Hit enter to find the image

Leave defaults and click Create

**Modify the pure-ftpd Service**

Delete the service that was created by the image

Import the ftpservice.yml included in this repo

**Create FTP user**

Open the pod once it is deployed and select the Terminal

Execute the following commands
```bash
pure-pw useradd bob -f /etc/pure-ftpd/passwd/pureftpd.passwd -m -u ftpuser -d /home/ftpusers/bob
```

Set the password to jmw11jmw

```bash
mkdir /home/ftpusers/bob/files
touch /home/ftpusers/bob/files/test.txt
su -
chgrp ftpgroup /home/ftpusers/bob/files
chmod 775 /home/ftpusers/bob/files
```

**Deploy FIS container**

Clone this repository

cd into the cloned directory
```bash
mvn fabric8:deploy
```

Open the pod once deployed and view the Logs

You should see the log message - Processed test.txt from remote FTP

**Test the dropfile API**

Create a POST request in Postman with the following settings:

URL - {your route URL}/ftp-service/dropfile

Headers
    Content-Type = application/octet-stream
    filename = customerfile.txt

Body
    Select binary
    Choose a file called customerfile.txt

Send the request and you will see the new file being processed by Camel

