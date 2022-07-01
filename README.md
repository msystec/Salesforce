# SFDXProject

1. Creat Salesforce login : https://login.salesforce.com/
2. Insatll jenkins 
  2.1 Install plugins - Git plugin, Custom tool plugin , Ant plugin and other plugins if required.
3. Create a Self-Signed SSL Certificate and Private Key
    3.1 Sample- create a SSL in  linux 
       a) Generate an RSA private key :
            .  openssl genrsa -out server.key 2048
       b) Request and generate the certificate :
            .  openssl req -new -key server.key -out server.csr

       c)   Generate the SSL certificate: 
            .  openssl x509 -req -sha256 -days 365 -in server.csr -signkey server.key -out server.crt
            
 4. Create Connected App for JWT-Based Flow
     
     a)  Created Connected App
          Callback URL
                  http://localhost:1717/OauthRedirect
                   * Use digital signatures To upload your server.crt file.
                   * Edit policy and select "Admin approved users are pre-authorized" to avoid "Not approved for access in salesforce" issue. 
                   * Assign Connected App to user or System Admin profile.
     b) Validate Authorize an Org Using the JWT-Based Flow  
                ###sfdx force:auth:jwt:grant --clientid {ADD_YOUR_CLIENT_ID} --jwtkeyfile server.key --username amit.salesforce21@gmail.com --instanceurl                                       https://login.salesforce.com --setdefaultdevhubusername###             
                
                          --clientid  :- provide Consumer Key
                          --jwtkeyfile :- Absolute path to the location where you generated your OpenSSL server.key file
                          --instanceurl :-provide instanceurl if you are using sandbox.
                          --setdefaultdevhubusername :- Set Default dev hub User Name.
  Sample
 
     ###sfdx force:auth:jwt:grant --clientid KeyHere --jwtkeyfile c:\openssl\bin\server.key --username amit.salesforce21@gmail.com --instanceurl                  https://login.salesforce.com --setdefaultdevhubusername###
