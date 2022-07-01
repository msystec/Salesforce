# Integration Of Jenkins With Salesforce

Pre-Requistes:

1. Create Salesforce login : 
  https://login.salesforce.com/

2. Insatll jenkins 
    Install plugins - Git plugin, Custom tool plugin , Ant plugin and other plugins if required.
    
3. Create a Self-Signed SSL Certificate and Private Key

    3.1 Sample- create a SSL in  linux 
    
       a) Generate an RSA private key :
                openssl genrsa -out server.key 2048
                
       b) Request and generate the certificate :
                openssl req -new -key server.key -out server.csr

       c)   Generate the SSL certificate: 
                openssl x509 -req -sha256 -days 365 -in server.csr -signkey server.key -out server.crt
            
 4. Create Connected App for JWT-Based Flow:
 
      You can find the below in salesforce login
      
              Apps( can search in quick find box)  -> App Manager -> New Connected App
          
     
     a)  Created Connected App
     
               Callback URL
                  http://localhost:1717/OauthRedirect
                  
               Use digital signatures To upload your server.crt file.
               
               Edit policy and select "Admin approved users are pre-authorized" to avoid "Not approved for access in salesforce" issue. 
               
               Assign Connected App to user or System Admin profile.
               
     b) Validate Authorize an Org Using the JWT-Based Flow  
     
            sfdx force:auth:jwt:grant --clientid {ADD_YOUR_CLIENT_ID} --jwtkeyfile server.key --username amit.salesforce21@gmail.com --instanceurl                                       https://login.salesforce.com --setdefaultdevhubusername          
                
                          --clientid  :- provide Consumer Key
                          --jwtkeyfile :- Absolute path to the location where you generated your OpenSSL server.key file
                          --instanceurl :-provide instanceurl if you are using sandbox.
                          --setdefaultdevhubusername :- Set Default dev hub User Name.
  
  Sample: 
    sfdx force:auth:jwt:grant --clientid KeyHere --jwtkeyfile c:\openssl\bin\server.key --username amit.salesforce21@gmail.com --instanceurl                  https://login.salesforce.com --setdefaultdevhubusername
    
   5. Configure SSL server.key in jenkins using manage credential page.

   6. Deploy using the JenkinsFile in jenkins.
          https://github.com/msystec/Salesforce/blob/sf-01/Jenkinsfile
          
   7. Configure the required environment varibles in jenkins.
   
       7.1 Set Environment variable:-
      
          HUB_ORG_DH:- The username for the Dev Hub org, such as amit@gmail.com
          SFDC_HOST_DH:- The login URL of the Salesforce instance that is hosting the Dev Hub org. The default is https://login.salesforce.com
          CONNECTED_APP_CONSUMER_KEY_DH :- The consumer key that was returned after you created a connected app in your Dev Hub org.
          JWT_CRED_ID_DH:- The credentials ID for the private key file that you stored in the Jenkins Admin Credentials interface
          
        7.2 Install the Custom Tools Plugin into your Jenkins console, and create a custom tool that references the Salesforce CLI
     
     
     
