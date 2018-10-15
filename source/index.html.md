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


Login to get user's credential
### HTTP Request

`GET http://domain.com/api/users/sign_in`
### Body Parameters

Parameter  | Description
--------- | -----------
email (required) | User email address which is registered on stt main page  
password (required) | User password


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
`GET http://domain.com//api/users`


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



# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Delete a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

