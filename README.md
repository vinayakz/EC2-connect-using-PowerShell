# AWS - EC2 connect using PowerShell or command prompt - Load key "*.pem": bad permissions

When I'm are trying connect EC2 linux instance using *.pem key, I got bad permissions error. [Load key "KeyName.pem": bad permissions]
## PowerShell log:
```sh
PS C:Main_Folder\AWS\Key> ssh -i "xyz.pem" ec2-user@ec2-3-IPaddress.compute-1.amazonaws.com

The authenticity of host 'ec2-3-124-128-121.compute-1.amazonaws.com (3.124.128.121)' can't be established.

ECDSA key fingerprint is SHA256:gxffeKwSSlc/0Uy1oZawn5+zcAn5xNAqraYUNyMEtpU.

Are you sure you want to continue connecting (yes/no)? yes

Warning: Permanently added 'ec2-3-124-128-121.compute-1.amazonaws.com,3.124.128.121' (ECDSA) to the list of known hosts.

```


```sh
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@

Permissions for 'xyz.pem' are too open.
This private key will be ignored.
It is required that your private key files are NOT accessible by others.

Load key "BlueGreyStripsKey.pem": bad permissions


ec2-user@ec2-3-224-127-120.compute-1.amazonaws.com: Permission denied (publickey,gssapi-keyex,gssapi-with-mic).
```

## Steps to resolve the issue:
1. Navigate to folder where you have placed the key file.[*.pem] - powershell or command prompt
2. Run the below commands one by one[Hit Enter key after each command]
* Set the file path	
```sh
>> $path = ".\xyz.pem"   
```
* Reset to remove explicit permissions
```sh
>> icacls.exe $path /reset
```
* Give current user explicit read-permission
```sh
>> icacls.exe $path /GRANT:R "$($env:USERNAME):(R)"
```
* Disable inheritance and remove inherited permissions
```sh
>> icacls.exe $path /inheritance:r
```
3. Now you are able to connect AWS EC2 instance without any issue. Please post comments if you face any issues. 

