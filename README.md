# wei1984.github.io

Userful resources:

System Design [https://github.com/checkcheckzz/system-design-interview](https://github.com/checkcheckzz/system-design-interview)


# How to config VMD data  
VMD and VMD Accounts data are required to rotate uCPE passwords in SubRosa. 

## Config Management APIs 
Swagger UI - https://<Subrosa env endpoint>/swagger-ui.html#/Config_Management_API_-_Manage_VMD_and_VMD_Account_data

Go to Swagger UI -> Config Management API
- Create new VMD  
Sample request -  
```
curl --location --request POST 'https://snap-subrosa-api-east-red.snap.comcast.net/management/vmd' \
  --header 'Content-Type: application/json' \
  --data-raw '{
    "changedBy": "wei",
    "rotationFrequencyDays": 90,
    "rotationNeeded": true,
    "vmdClientId": "voae_rest",
    "vmdClientSecret": "asrevnet_123",
    "vmdEndpoint": "https://vdsdwan-red-snap-vip.dse.comcast.net:9183",
    "vmdName": "vmd-red",
    "vmdPassword": "vmdPassword",
    "vmdUsername": "snapapiuser02"
  }'
```
    
- Create new VMD Account for each user

Sample request -
```
curl --location --request POST 'https://snap-subrosa-api-east-red.snap.comcast.net/management/vmd' \
   --header 'Content-Type: application/json' \
   --data-raw '{
     "vmdName": "vmd-red",
     "accountName": "admin",
     "accountRole": "admin",
     "accountLoginType": "shell"
   }'
```
- Verify if VMD Account creates successfully - "Get all VMD Account" API.
```
curl -X GET "https://snap-subrosa-api-east-red.snap.comcast.net/management/vmd/accounts" -H "accept: application/json"
```
Sample response - 
```
[
  {
    "vmdName": "vmd-red",
    "accountName": "admin",
    "accountRole": "admin",
    "accountLoginType": "shell",
    "createAt": "2020-01-23T21:27:52.824+0000"
  }
]
```
- Go to Swagger UI -> Password Management API, Verify if VMD Account credentials creates successfully - "Get all VMD accounts/passwords" API.
```
curl -X GET "https://snap-subrosa-api-east-red.snap.comcast.net/passwd/vmd/vmd-red/accounts" -H "accept: application/json"
```
Sample response - 
```
[
  {
    "username": "admin",
    "password": "hashed password",
    "role": "admin",
    "loginType": "shell"
  }
]
```  

## Config existed VMD data (DSE team)
For existed VMD and VMD users, migrate data to Subrosa with following steps:
1. Manually create existed VMD user password in cyberArk.
2. For each VMD, invoke "Create new VMD" API in Subrosa.
3. For each VMD user, invoke "Create new VMD Account" in Subrosa.
4. Verify VMD Account with "Get all VMD Account" API.
5. Verify VMD Account credentials - "Get all VMD accounts/passwords" API

## Config new VMD data (Operation team)
For new VMD and VMD users, VMD user password will be auto-generated in Subrosa and stored in cyberArk.
Add new VMD and VMD users in Subrosa with following steps:
1. For each VMD, invoke "Create new VMD" API in Subrosa.
2. For each VMD user, invoke "Create new VMD Account" in Subrosa.
3. Verify VMD Account with "Get all VMD Account" API.
4. Verify VMD Account credentials - "Get all VMD accounts/passwords" API
                                                
 
