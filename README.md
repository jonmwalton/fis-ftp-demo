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
Execute the following command
```bash
pure-pw useradd bob -f /etc/pure-ftpd/passwd/pureftpd.passwd -m -u ftpuser -d /home/ftpusers/bob
```

**Deploy FIS container**
Clone this repository
cd into the cloned directory
```bash
mvn fabric8:deploy -Dfabric8.deploy.createExternalUrls=true
```


