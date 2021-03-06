# Introdcution 
IAM is the centralized system managing the identity of Carrefour Customers and provides single identity / one time authentication of customer’s access to various channels such as mobile app and Self Care.

## State Diagram
![test](./uc1activitydiag.png)


## Usecases
### Login with Mobile OTP

Mobile-OTP is the primary way of logging into app. Once logged in, user can browse through the mini apps without entering OTP. OAuth token is generated and being passed to micro app’s after each toggle. It is micro app’s responsibility to validate OAuth token and allow access to the customer. Any invalid attempt should result in 401 unauthorized access. 
 
![uc1](./mobileotp.png)

### Login using Social Login

![uc2](./mobileotp.png)

## Interfaces 
### API to validate OAuth Token

Description: This is server to server API which will allow multiple microapps to validate OAuth token.

Method: Post

Endpoint: http://IP:PORT/ v1/jm/l1auth/validate/oauthtoken 


Request Headers:

Name |  Type |  Mandatory | Description |
------ | ------ | ------ | ------ 
 X-CHANNEL-ID|  String | Yes|  Channel Name | 
 X-TRACE-ID|	String | No	| Shared by channel to maintain end to end tracing
 X-API-KEY|	String | Yes | This is for authenticating initiating channel.

Request Payload : 

Name |  Type |  Mandatory | Description |
------ | ------ | ------ | ------ 
 OAuthToken |  String | Yes|  Token provided by Native app over JS bridge | 
 Checksum |  String | Yes|   | 

Response Payload : 

Name |  Type |  Mandatory | Description |
------ | ------ | ------ | ------ 
 isTokenValid|  Boolean | Yes|  Possible value true and false. User should be allowed to proceed only if value is true. | 
 primaryMobileNumber|	String | No	| This is a user primary myjio logged in number.  This value will be returned if the isTokenValid flag is true. 
 userId|	String | No | This is a userid which can be used for address/profile management. This value will be returned if the isTokenValid flag is true.


