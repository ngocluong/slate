---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ

toc_footers:
  - <a href="http://stockstotrade-test.herokuapp.com/">testing</a>
  - <a href="http://desolate-dawn-4741.herokuapp.com">staging</a>
  - <a href="http://orders.stockstotrade.com/">production</a>
  
includes:
  - errors

search: true
---

# Introduction

Welcome to the Stockstotade API! You can use our API to access STT API endpoints, which can get information on users, subscriptions in our database.

# Authentication
## User Authenticaticity Token
> 
without/incorrect authenticity token, user will receive following response:
### 200 REQUIRED TOKEN:
**RESPONSE BODY:**<br/>
&emsp;{<br/>
  &emsp;&emsp;success: false,<br/>
  &emsp;&emsp;message: "Authorized user only."<br/>
&emsp;}<br/>

Stockstotade expects for the authenticity token to be included in almost all API requests to the server in a header that looks like the following:

`access-token: [access-token]`, `client: [client]`, `expiry: [expiry]`, `uid: [uid]`
<aside class="notice">
You must replace information inside <code>[]</code> with your personal API key get from login API.
</aside>

First, user should sign up on Stocktotrade page (depends on the environment - we have testing, staging and production within link on the left side)  

# User
<aside class="success">
REMEMBER -- Send your request with your authenticity token <a href="#authentication">example</a>
</aside>

## Login
<aside class="notice">
This API does not require authenticity token
</aside>

>
### 200 SUCCESS RESPONSE:
**RESPONSE BODY:**

```json
  {
    "success": true
  }
```
>
**RESPONSE HEADER:**<br/>
&emsp;Access-Token →[Access-Token]<br/>
&emsp;Token-Type →Bearer<br/>
&emsp;Client →[Client]<br/>
&emsp;Expiry →[Expiry]<br/>
&emsp;Uid →[uid Email]<br/>
&emsp;Content-Type →application/json; charset=utf-8<br/>
### 401 - WITHOUT SUBSCRIPTION
**RESPONSE BODY:**

```json
  {
    "success": false,
    "message": "You need to subscribe in order to log in."
  }
```
>
### 401 - SUBSCRIPTION EXPIRED
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Subscription expired."
}
```
>
### 401 - ACCOUNT IS NOT ACTIVATED
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "A confirmation email was sent to your account at [email]. You must follow the instructions in the email before your account can be activated"
}
```
>
### 401 - INVALID LOGIN CREDENTIALS
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Invalid login credentials. Please try again."
}
```
>
### 401 - SERVICE IS DOWN
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Service is temporarily down for maintenance."
}
```
>
### 200 - IP IS BANNED
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "This IP address is currently banned for security reasons. Please contact support@stockstotrade.com for more information."
}
```
>
### 200 - REQUIRE FILL MARKET AGREEMENT:
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "You need to fill out this form before logging in: [URL]"
}
```
>
### 200 - REQUIRE UPDATE MARKET AGREEMENT:
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "You must update your yearly Market Agreement form to log in: [URL]"
}
```
>
### 200 - MISSING TOS AGREEMENT:
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Missing or false tos agreement"
}
```
>
### 200 - ACCOUNT SUPPENDED:
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Your account has been suspended due to Market Compliance inconsistencies. Please contact support."
}
```
>
### 200 - TRIAL ACCOUNT DO NOT SIGNED AGREEMENT:
**RESPONSE BODY:**

```json
{
  "success": false,
  "trialAgreed": false,
  "message": "You need to fill out trial Agreement before logging in"
}
```


Login to get user's credential
### HTTP Request

`GET http://domain.com/api/users/sign_in`
### Body Parameters

Parameter  | Description
--------- | -----------
email (required) | User email address which is registered on stt main page  
password (required) | User password
webfork (optional) | true/false to determine webfork app 


## Get all user emails
>
### 200 SUCCESS RESPONSE:
**RESPONSE BODY:**<br/>
*list of user emails*

```json
[
  "user1@email.com",
  "user2@email.com"
]
```

### HTTP Request
`GET http://domain.com/api/users`

Parameter  | Description
--------- | -----------
recurly_plan_code | query user with recurly_plan_code 

## Get profile pictures
<aside class="success">
THIS API NOT REQUEST AUTHENTICITY TOKEN. BUT NEED MD5 HASH OF DATE 
</aside>

>
### 200 SUCCESS RESPONSE:
**RESPONSE BODY:**<br/>

```json
{ 
  "success": true,
  "data": [
     {
       "nickname": [user_nickname_1],
       "profile_picture": [picture_url_1]
     },
     {
       "nickname": [user_nickname_2],
       "profile_picture": [picture_url_2]
     }
  ]
}
```

Get user profile pictures by nicknames
### HTTP Request
`GET http://domain.com/api/user_profile_pictures`

Parameter  | Description
--------- | -----------
nicknames | nicknames for query users, separate by `,`. For example: `user_nickname_1, user_nickname_2`  

## Update Password
>
### 200 SUCCESS RESPONSE:
**RESPONSE BODY:**

```json
{
  "success": true,
  "message": "Password updated."
}
```
>
### 200 - INVALID:
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "error message"
}
```


Update user password
### HTTP Request
`PUT http://domain.com/api/users/password`
### Body Parameters

Parameter  | Description
--------- | -----------
current_password (required) | User current password  
password (required) | User new password
password_confirmation (required) | User new password confirmation

## Reset Password
>
### 200 SUCCESS RESPONSE:
**RESPONSE BODY:**

```json
{
  "success": true
}
```
>
### 200 - USER NOT FOUND:
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "No user found with this email"
}
```
>
### 401 - MISSING EMAIL PARAM:
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "You must provide an email address."
}
```

Reset password (use when user forgot password)
### HTTP Request
`POST http://domain.com/api/users/password`
### Body Parameters

Parameter  | Description
--------- | -----------
email (required) | User email which need to be reset  

## Update Avatar
>
### 200 SUCCESS RESPONSE:
**RESPONSE BODY:**

```json
{
  "success": true,
  "message": "Avatar uploaded."
}
```

Update user Avatar
### HTTP Request
`POST http://domain.com/api/users/avatar`
### Body Parameters

Parameter  | Description
--------- | -----------
file (required) | User Avatar file  

## Get User Setting
>
### 200 SUCCESS RESPONSE:
**RESPONSE BODY:**

```json
  "User setting file content."
```
>
### 500 - CANNOT FIND USER SETTING:
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Cannot find user setting"
}
```
>
### 200 - CONTENT IS INVALID
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "user setting content is invalid"
}
```

Update user setting file
### HTTP Request
`GET http://domain.com/api/users/setting`

## Update User Setting
>
### 200 SUCCESS RESPONSE:
**RESPONSE BODY:**

```json
{
  "success": true,
  "message": "Setting uploaded"
}
```
>
### 200 - MISSING/EMPTY FILE:
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Missing either file or file_content parameter"
}
```
>
### 200 - FAIL TO UPLOAD:
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Upload was unsuccessful"
}
```

Update user setting file
### HTTP Request
`POST http://domain.com/api/users/update_setting`
### Body Parameters

Parameter  | Description
--------- | ----------- 
file | Required if leave file_content param blank. File ini for user setting
file_content | Required if leave file param blank. content of user setting file
force_upload (Boolean/Optional) | Optional true when force upload file content

## Update User Profile Picture
>
### 200 SUCCESS RESPONSE:
**RESPONSE BODY:**

```json
{
"success": true,
"message": "Profile Picture uploaded"
}
```
>
### 200 - FAIL TO UPLOAD:
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Upload was unsuccessful"
}
```

Update user Profile picture
### HTTP Request
`POST http://domain.com/api/users/update_profile_picture`
### Body Parameters

Parameter  | Description
--------- | -----------
file (required)| User profile picture

## Get User Profile
>
### 200 SUCCESS RESPONSE:
**RESPONSE BODY:**<br/>

```json
{
  "id": [user_id],
  "firstName": [firstName],
  "lastName": [lastName],
  "email": [email],
  "occupation": [occupation],
  "company": [company],
  "address_line_1": [address_line_1],
  "address_line_2": [address_line_2],
  "country": [country],
  "zip_code": [zip_code],
  "state": [state],
  "city": [city],
  "avatar": [avatar],
  "twitter_screenname": [twitter_screenname],
  "nickname": [nickname],
  "tos_agreement": [tos_agreement],
  "tos_agreement_date": [tos_agreement_date],
  "is_pro": [is_pro(true/false)],
  "available_addons": [list_of_addons],
  "trial_user": [trial_user(true/false)]
}
```

Get user profile
### HTTP Request
`GET http://domain.com/api/users/profile`

## Update User Profile
>
### 200 SUCCESS RESPONSE:
**RESPONSE BODY:**

```json
{
"success": true,
"message": "Profile updated."
}
```
>
### 200 - FAIL TO UPLOAD:
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Error message"
}
```
>
### 400 - MISSING PARAMETER
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "[field] cannot be blank"
}
```
Update user Profile
### HTTP Request
`PUT http://domain.com/api/users`
### Body Parameters

Parameter  | Description
--------- | -----------
first_name (required)| user's first name
last_name (required)| user's last name
country (required)| user's country
city (required)| user's city
occupation | user's occupation 
company | user's company 
state | user's state 
twitter_screenname | user's twitter screenname 
nickname | user's nickname 
tos_agreement | user's tos_agreement 
address_line_1 | user's address line 1 
address_line_2 | user's address line 2 
zip_code | user's zip code 

## Update User security question
>
### 201 SUCCESS RESPONSE:
**RESPONSE BODY:**

```json
{
 "success": true
}
```
>
### 400 - INVALID JSON
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Invalid json data"
}
```
Update user Profile
### HTTP Request
`PUT http://domain.com/api/users/security_question`
### Body Parameters

Parameter  | Description
--------- | -----------
json data for question and answer| question and answer data in body in json format

# Order
<aside class="success">
REMEMBER -- Send your request with your authenticity token <a href="#authentication">example</a>
</aside>

## Get Order
>
### 200 SUCCESS RESPONSE:
**RESPONSE BODY:**

```json
  [
      {
          "product_id": [product_id_1],
          "email": [email],
          "subscription_uid": [subscription_uid],
          "product": {
              "name": [name],
              "recurly_plan_code": [recurly_plan_code],
              "line_description": [line_description],
              "price_description": [price_description],
              "investimonial_id": [investimonial_id]
          }
      },
      {
          "product_id": [product_id_2],
          "email": [email],
          "subscription_uid": [subscription_uid],
          "product": {
              "name": [name],
              "recurly_plan_code": [recurly_plan_code],
              "line_description": [line_description],
              "price_description": [price_description],
              "investimonial_id": [investimonial_id]
          }
      }, ...
  ]
```
>
### 404 - NOT FOUND
**RESPONSE BODY:**

```json
{
  "message": "Unable to find resource"
}
```

Get order by email
### HTTP Request
`GET http://domain.com/api/orders/:email`
### Parameters

Parameter  | Description
--------- | -----------
email| Query email

# System Message
<aside class="success">
REMEMBER -- Send your request with your authenticity token <a href="#authentication">example</a>
</aside>

## Get system message
>
### 200 SUCCESS RESPONSE:
**RESPONSE BODY:**

```json
{
    "success": true,
    "messages": [
        {
            "id": [id (int)],
            "subject": [subject 1],
            "body": [body 1],
            "publish_date": [publish_date],
            "message_type": [message_type],
            "priority": [priority (int)],
            "created_at": [created_at],
            "updated_at": [updated_at],
            "valid_until": [valid_until],
            "status": [status (unread/read)]
        },
        {
            "id": [id (int)],
            "subject": [subject 1],
            "body": [body 1],
            "publish_date": [publish_date],
            "message_type": [message_type],
            "priority": [priority (int)],
            "created_at": [created_at],
            "updated_at": [updated_at],
            "valid_until": [valid_until],
            "status": [status (unread/read)]
        }
    ]
}
```

Get system messages
### HTTP Request
`GET http://domain.com/api/system_messages`
### Parameters

Parameter  | Description
--------- | -----------
from_id| If present - Get messages which has id greater than `from_id`
from_time| If present - Get all messages update later than `from_time`
unread_only| If present - Get all message that user has not read

## Read Message
>
### 200 SUCCESS RESPONSE:
**RESPONSE BODY:**

```json
{
  "success": true
}
```
>
### 200 - NOT FOUND
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Cannot find message with id [id]"
}
```

Read system message
### HTTP Request
`POST http://domain.com/api/system_messages/:id/read`
### Parameters

Parameter  | Description
--------- | -----------
id (required) | Message ID. Set param to `all` in order to read all unread messages  

# Nyse Entry
<aside class="success">
REMEMBER -- Send your request with your authenticity token <a href="#authentication">example</a>
</aside>

## Update Nyse Entry
>
### 200 SUCCESS RESPONSE:
**RESPONSE BODY:**

```json
{
  "success": true
}
```
>
### 200 - NYSE ENTRY IS COMPLETE ALREADY
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "User's nyse entry is completed"
}
```
>
### 200 - INVALID NYSE TOKEN
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Invalid nyse token"
}
```
>
### 200 - PARAMS INVALID
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Error message"
}
```

sign Market Agreement
### HTTP Request
`PUT http://domain.com/api/nyse_entries/:id`
### Parameters

Parameter  | Description
--------- | -----------
id (required) | Nyse Entry ID  
nyse_entry | Json params for nyse entry


Nyse Entry Json Key |
-------------- | 
"name_of_vendor"| 
"section1_agreed"|
"section2_12_a"|
"section2_12_b"|
"section2_12_c"|
"section2_12_d"|
"section2_12_e"|
"section2_12_f"|
"section2_12_g"|
"section2_12_h"|
"section2_12_i"|
"section2_12_j"|
"section2_12_k"|
"section2_12_certificated"|
"section2_agreed"|
"nasdaq_agreed"|
"is_non_pro"|
"personal_first_name"|
"personal_last_name"|
"personal_middle_name"|
"personal_address"|
"personal_city"|
"personal_state"|
"personal_country"|
"personal_zip"|
"personal_email"|
"employer_occupations"|
"employer_retiree_name"|
"employer_retiree_address"|
"employer_retiree_city"|
"employer_retiree_state_prov"|
"employer_retiree_country"|
"employer_retiree_zip"|
"employer_title"|
"employer_functions"|
"nyse_agreed"|
"otc_agreed"|
"signature_agreed"|


# Activity Log
<aside class="success">
REMEMBER -- THIS API NOT REQUEST AUTHENTICITY TOKEN. BUT NEED MD5 HASH OF DATE 
</aside>

## Connected
>
### 200 SUCCESS RESPONSE:
**RESPONSE BODY:**

```json
{
  "success": true,
  "message": "User Activity Log added."
}
```
>
### 200 - PARAMS INVALID
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Error message"
}
```
>
### 200 - INVALID MD5 TOKEN
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Invalid token"
}
```
>
### 200 - USER NOT FOUND
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Email address was not found"
}
```

Add user activity log when connected
### HTTP Request
`POST http://domain.com/api/user_activity_logs/connected`
### Parameters

Parameter  | Description
--------- | -----------
user_email (required) | Email of connected user
user_ip (required) | User's ip address
token (required) | MD5 of date
event_details | Note for activity log
device_type | mobile, web agent or desktop
device_id | device id (always be the same on the same device)
product | product user connecting with

## Disconnected
>
### 200 SUCCESS RESPONSE:
**RESPONSE BODY:**

```json
{
  "success": true,
  "message": "User Activity Log added."
}
```
>
### 200 - PARAMS INVALID
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Error message"
}
```
>
### 200 - INVALID MD5 TOKEN
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Invalid token"
}
```
>
### 200 - USER NOT FOUND
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Email address was not found"
}
```

Add user activity log when disconnected
### HTTP Request
`POST http://domain.com/api/user_activity_logs/disconnected`
### Parameters

Parameter  | Description
--------- | -----------
user_email (required) | Email of disconnected user
user_ip (required) | User's ip address
token (required) | MD5 of date
event_details | Note for activity log
device_type | mobile, web agent or desktop
device_id | device id (always be the same on the same device)
product | product user connecting with

## Subscribed
>
### 200 SUCCESS RESPONSE:
**RESPONSE BODY:**

```json
{
  "success": true,
  "message": "User Activity Log added."
}
```
>
### 200 - PARAMS INVALID
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Error message"
}
```
>
### 200 - INVALID MD5 TOKEN
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Invalid token"
}
```
>
### 200 - USER NOT FOUND
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Email address was not found"
}
```

Add user activity log when subscribed
### HTTP Request
`POST http://domain.com/api/user_activity_logs/subscribed`
### Parameters

Parameter  | Description
--------- | -----------
user_email (required) | Email of subscribed user
user_ip (required) | User's ip address
token (required) | MD5 of date
event_details | Note for activity log
device_type | mobile, web agent or desktop
device_id | device id (always be the same on the same device)
product | product user connecting with

## Unsubscribed
>
### 200 SUCCESS RESPONSE:
**RESPONSE BODY:**

```json
{
  "success": true,
  "message": "User Activity Log added."
}
```
>
### 200 - PARAMS INVALID
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Error message"
}
```
>
### 200 - INVALID MD5 TOKEN
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Invalid token"
}
```
>
### 200 - USER NOT FOUND
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Email address was not found"
}
```

Add user activity log when unsubscribed
### HTTP Request
`POST http://domain.com/api/user_activity_logs/unsubscribed`
### Parameters

Parameter  | Description
--------- | -----------
user_email (required) | Email of unsubscribed user
user_ip (required) | User's ip address
token (required) | MD5 of date
event_details | Note for activity log
device_type | mobile, web agent or desktop
device_id | device id (always be the same on the same device)
product | product user connecting with

# TOS Agreement
<aside class="success">
REMEMBER -- THIS API NOT REQUEST AUTHENTICITY TOKEN. BUT NEED MD5 HASH OF DATE 
</aside>

## Sign TOS
>
### 200 SUCCESS RESPONSE:
**RESPONSE BODY:**

```json
{
  "success": true,
  "message": "Signed TOS successfully"
}
```
>
### 200 - PARAMS INVALID
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Error message"
}
```
>
### 200 - INVALID MD5 TOKEN
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Invalid token"
}
```
>
### 200 - USER NOT FOUND
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Email address was not found"
}
```

Sign TOS Agreement
### HTTP Request
`PUT http://domain.com/api/user_tos/signed`
### Parameters

Parameter  | Description
--------- | -----------
user_email (required) | Email of user sign TOS  
token (required) | MD5 of date  


# User Trial Agreement
<aside class="success">
REMEMBER -- THIS API NOT REQUEST AUTHENTICITY TOKEN. BUT NEED MD5 HASH OF DATE 
</aside>

## Update Trial Agreement
>
### 200 SUCCESS RESPONSE:
**RESPONSE BODY:**

```json
{
  "success": true,
  "message": "Signed trial agreement successfully"
}
```
>
### 200 - PARAMS INVALID
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Error message"
}
```
>
### 200 - INVALID MD5 TOKEN
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Invalid token"
}
```
>
### 200 - USER NOT FOUND
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Email address was not found"
}
```
>
### 200 - MISSING TRIAL_AGREEMENT_ACCEPTED PARAMETER
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Missing trial_agreement_accepted param"
}
```

Update Trial Agreement
### HTTP Request
`POST http://domain.com/api/trial_agreements`
### Parameters

Parameter  | Description
--------- | -----------
user_email (required) | Email of user sign TOS  
trial_agreement_accepted (required) | true/false for update agreement
token (required) | MD5 of date  

# Token Validation
<aside class="success">
REMEMBER -- THIS API REQUEST AUTHENTICITY TOKEN AND CHECK BASED ON THESE TOKEN 
</aside>

## CHECK TOKEN
>
### 200 SUCCESS RESPONSE:
**RESPONSE BODY:**

```json
{
    "success": true,
    "message": "Valid token",
    "valid_until": "2019-03-13T15:35:07.000+07:00"
}
```
>
### 200 - EXPIRED/INVALID RESPONSE
**RESPONSE BODY:**

```json
{
    "success": false,
    "message": "Invalid or expired tokens"
}
```

Check token
### HTTP Request
`GET http://domain.com/api/token_validations/check`
### Parameters
It is based on authenticity token to check.  

# Subscription
<aside class="success">
REMEMBER -- Send your request with your authenticity token <a href="#authentication">example</a>
</aside>

## Get User Subscriptions
>
### 200 SUCCESS RESPONSE:
**RESPONSE BODY:**

```json
[
  {
    "plan": [subscription plan_1],
    "uuid": [subscription uuid_1],
    "state": [subscription state_1],
    "invoice": [subscription invoice_1],
    "account": [subscription account_1],
    "unit_amount_in_cents": [subscription unit_amount_in_cents_1],
    "quantity": [subscription quantity_1],
    "currency": [subscription currency_1],
    "activated_at": [subscription activated_at_1],
    "canceled_at": [subscription canceled_at_1],
    "expires_at": [subscription expires_at_1],
    "current_period_started_at": [subscription current_period_started_at_1],
    "current_period_ends_at": [subscription current_period_ends_at_1],
    "trial_started_at": [subscription trial_started_at_1],
    "trial_ends_at": [subscription trial_ends_at_1],
    "tax_in_cents": [subscription tax_in_cents_1],
    "tax_type": [subscription tax_type_1],
    "tax_region": [subscription tax_region_1],
    "tax_rate": [subscription tax_rate_1],
    "po_number": [subscription po_number_1],
    "net_terms": [subscription net_terms_1],
    "collection_method": [subscription collection_method_1],
    "subscription_add_ons": [subscription subscription_add_ons_1],
    "pending_subscription": [subscription pending_subscription_1],
    "terms_and_conditions": [subscription terms_and_conditions_1],
    "customer_notes": [subscription customer_notes_1],
    "bank_account_authorized_at": [subscription bank_account_authorized_at_1]
  },
  {
    "plan": [subscription plan_2],
    "uuid": [subscription uuid_2],
    "state": [subscription state_2],
    "invoice": [subscription invoice_2],
    "account": [subscription account_2],
    "unit_amount_in_cents": [subscription unit_amount_in_cents_2],
    "quantity": [subscription quantity_2],
    "currency": [subscription currency_2],
    "activated_at": [subscription activated_at_2],
    "canceled_at": [subscription canceled_at_2],
    "expires_at": [subscription expires_at_2],
    "current_period_started_at": [subscription current_period_started_at_2],
    "current_period_ends_at": [subscription current_period_ends_at_2],
    "trial_started_at": [subscription trial_started_at_2],
    "trial_ends_at": [subscription trial_ends_at_2],
    "tax_in_cents": [subscription tax_in_cents_2],
    "tax_type": [subscription tax_type_2],
    "tax_region": [subscription tax_region_2],
    "tax_rate": [subscription tax_rate_2],
    "po_number": [subscription po_number_2],
    "net_terms": [subscription net_terms_2],
    "collection_method": [subscription collection_method_2],
    "subscription_add_ons": [subscription subscription_add_ons_2],
    "pending_subscription": [subscription pending_subscription_2],
    "terms_and_conditions": [subscription terms_and_conditions_2],
    "customer_notes": [subscription customer_notes_2],
    "bank_account_authorized_at": [subscription bank_account_authorized_at_2]
  }
]
```
>
### 200 - INVALID PLAN CODE
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Invalid recurly plan code"
}
```
>
### 200 - PLAN CODE NOT FOUND
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "No product found with input plan code"
}
```

Get user subscriptions
### HTTP Request
`GET http://domain.com/api/subscriptions`
### Parameters

Parameter  | Description
--------- | -----------
recurly_plan_code (required) | Plan code associated with Product

## Get Subscription Addons
>
### 200 SUCCESS RESPONSE:
**RESPONSE BODY:**

```json
[
    {
        "href": [href_2],
        "uri": [uri_2],
        "destroyed": [destroyed (true/false)],
        "new_record": [new_record (true/false)],
        "attributes": {
            "add_on_code": [add_on_code],
            "add_on_type": [add_on_type],
            "unit_amount_in_cents": [unit_amount_in_cents (int)],
            "quantity": [quantity (int)],
        },
        "links": {},
        "changed_attributes": {},
        "subscription": [subscription]
    },
    {
        "href": [href_2],
        "uri": [uri_2],
        "destroyed": [destroyed (true/false)],
        "new_record": [new_record (true/false)],
        "attributes": {
            "add_on_code": [add_on_code],
            "add_on_type": [add_on_type],
            "unit_amount_in_cents": [unit_amount_in_cents (int)],
            "quantity": [quantity (int)],
        },
        "links": {},
        "changed_attributes": {},
        "subscription": [subscription]
    }
]
```
>
### 500 - ERROR
**RESPONSE BODY:**

```json
  "Error" 
```

Get user subscriptions
### HTTP Request
`GET http://domain.com/api/subscriptions/active_add_ons`

## Cancel User Subscriptions
>
### 200 SUCCESS RESPONSE:
**RESPONSE BODY:**

```json
{
  "success": true
}
```
>
### 200 - INVALID UUID
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Invalid uuid"
}
```
>
### 200 - SUBSCRIPTION NOT FOUND
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Subscription not found with input uuid"
}
```

Get user subscriptions
### HTTP Request
`PUT http://domain.com/api/subscriptions/cancel`
### Parameters

Parameter  | Description
--------- | -----------
uuid (required) | Subscription's uuid

## Add subscription
<aside class="success">
THIS API NOT REQUEST AUTHENTICITY TOKEN 
</aside>
>
### 200 SUCCESS RESPONSE:
**RESPONSE BODY:**

```json
{ 
  "msg": "Success:[account_code]",
  "status": 200 
}
```
>
### 200 - Error
**RESPONSE BODY:**

```json
{ 
  "msg": "Error message",
  "status": 200 
}
```

Order new subscription
### HTTP Request
`POST http://domain.com/api/subscription_orders`
### Parameters

Parameter  | Description
--------- | -----------
order (required) | Json params for order  


Order Json Key |
-------------- | 
"email" |
"confirm_email" |
"first_name" |
"last_name" |
"billing_info" | 
"number" |
"month" |
"year" |
"phone" |
"address_line_1" |
"address_line_2" |
"city" |
"country" |
"state" |
"zip_code" |
"recurly_token" |
"recurly_plan_code" |
"aff_code" |
"tos_agreement" |
"coupon_code" |
"addon_code" |

# External Account
## Add external account
<aside class="success">
THIS API NOT REQUEST AUTHENTICITY TOKEN 
</aside>

>
### 201 SUCCESS RESPONSE: 
**RESPONSE BODY:**

```json
{ 
  "success": true
}
```
>
### 500 - Internal server error
**RESPONSE BODY:**

```json
{ 
  "error": "Error message" 
}
```
>
### 400 - Error when create user
**RESPONSE BODY:**

```json
{ 
  "error": "Error message" 
}
```

Order new subscription
### HTTP Request
`POST http://domain.com/api/external_api/accounts`
### Parameters

Parameter  | Description
--------- | -----------
first_name | first name 
last_name | last name 
email | email 
address_line_1 | address line 1 
address_line_2 | address line 2 
country | country 
state | state 
city | city 
zip_code | zip code 
phone | phone 
external_offer_code | affiliate external offer code 
api_key | affiliate api key   

# Add-on
<aside class="success">
REMEMBER -- Send your request with your authenticity token <a href="#authentication">example</a>
</aside>

## List available Addons
>
### 200 SUCCESS RESPONSE:
**RESPONSE BODY:**

```json
[
  {
    "subscription_id": "sub1_id",
    "plan_name": "Plan 1",
    "addon": {
      "id": 1,
      "recurly_add_on_code": "level2",
      "name": "add on 1",
      "display_name": "Add on display name 1",
      "description": "addon description 1",
      "disable_extra_charge_for_monthly": false,
      "is_enabled": true
    },
    "price": {
      "USD": 34500
    }
  },
  {
    "subscription_id": "sub2_id",
    "plan_name": "Plan 2",
    "addon": {
      "id": 2,
      "recurly_add_on_code": "Chat&Sentiment",
      "name": "Tip Ranks and chat room",
      "display_name": "tipranks and chatroom",
      "description": "tipranks and chatroom",
      "disable_extra_charge_for_monthly": true,
      "is_enabled": true
    },
    "price": {
      "USD": 1500
    }
  }
]
```

List all available addon

### HTTP Request
`GET http://domain.com/api/addons`

## Adding Addon
>
### 200 SUCCESS RESPONSE:
**RESPONSE BODY:**

```json
{
  "success": true,
  "message": "Add-on has been successfully added"
}
```
>
### 200 - Addon existed
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Add-on existed"
}
```
>
### 200 - Addon is not present for user
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": "Failed to add add-on"
}
```

Add addon to user

### HTTP Request
`POST http://domain.com/api/addons`
### Parameters

Parameter  | Description
--------- | -----------
subscription_id (required) | Subscription uuid for adding addon
addon (required) | addon code

# Add webfork log

## Add log for user
<aside class="success">
REMEMBER -- Send your request with your authenticity token <a href="#authentication">example</a>
</aside>

>
### 200 SUCCESS RESPONSE:
**RESPONSE BODY:**

```json
{
  "success": true,
  "message": 'User log uploaded'
}
```

>
### 200 MISSING PARAM:
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": 'Missing file_content parameter'
}
```

Add log for specific user

### HTTP Request
`PUT http://domain.com/api/user_logs`

### Parameters

Parameter  | Description
--------- | -----------
file_content (required) | Log content

## Add general log
<aside class="success">
REMEMBER -- THIS API NOT REQUEST AUTHENTICITY TOKEN. BUT NEED MD5 HASH OF DATE 
</aside>

>
### 200 SUCCESS RESPONSE:
**RESPONSE BODY:**

```json
{
  "success": true,
  "message": 'User log uploaded'
}
```

>
### 200 MISSING PARAM:
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": 'Missing file_content parameter'
}
```

>
### 200 INVALID TOKEN:
**RESPONSE BODY:**

```json
{
  "success": false,
  "message": 'Invalid token'
}
```

Add general log

### HTTP Request
`POST http://domain.com/api/user_logs`

### Parameters

Parameter  | Description
--------- | -----------
file_content (required) | Log content
token (required) | MD5 of date
