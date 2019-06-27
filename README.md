# EngageBay Javacript API
 [EngageBay](https://www.engagebay.com "EngageBay") is a simple, affordable all-in-one marketing and sales software built for growing businesses. Get the power of an enterprise software at a fraction of the cost.

![](https://raw.githubusercontent.com/peteraccountbox/tracking_code_api/master/assets/engagebay-logo2.png)

**Table of Contents**

**[API Setup](#setting-api--analytics)**
  * [JS API & Tracking code installation](#setting-api--analytics)
  
**[Things to know](#things-to-know)**
  * [Identify a visitor](#identify-a-visitor)
  * [Tracking page views](#tracking-page-views)
  * [Override email](#override-email)

**[Supporting API](#supporting-api)**
  * [1 Tracking website visitors](#1tracking-website-visitors)
    * [Setting Email of the page visitor](#setting-email-of-the-page-visitor)
    * [Get Email](#get-email)
    * [Tracking across multiple sites](#tracking-across-multiple-sites) 

  * [2 Contact](#2contact)
    * [Create contact](#create-contactr)
    * [Get contact](#get-contact)
    * [Update contact](#update-contact)
    * [Set contact property](#set-contact-property) 
  
### Setting API & Analytics
- You may access the tracking code from ***Account Settings ->  API & Tracking Code -> Tracking code***
- Copy the 9 lines of script and paste in it in your webpage's HTML just before the ```</body>``` tag, for which you need API methods and /or tracking the page visits.

![Finding Analytics Code](https://raw.githubusercontent.com/peteraccountbox/tracking_code_api/master/assets/api-code2.jpg)

## Things to know

### Identify a visitor

Identify a visitor. This function can be used to identify a visitor by email address.  If there is an existing contact record with a matching email address, the existing contact will be updated.  Otherwise, a new contact record will be created.  In both cases, the page views data collected for the visitor will be associated with the contact record.


	EhAPI.push(["setEmail", 'visitor@emaildomain.com']);
	
  
### Tracking page views

Track a page view. This function is called when the tracking code is loaded on a page, but you can manually call this function to track subsequent views in a single page application.

	EhAPI.push(["trackPageView"]);
	

This function does not support any arguments.

EngageBay tracks the page views for each session for each contact. The visitor is tracked as anonymous unless a setEmail is done. The correct step is to setEmail and then trackPageView. Anonymous users are tracked and backtracked when a user email is added using setEmail. All the old page views are attributed to the new email address. This is often the case where the user browses many sessions and then signups for your product or provides the email in the landing pages during product download.

### Override email

If a new email address is provided, EngageBay treats them as a new user and the sessions are tracked for the new user. The old page views are still valid for the old email and can be accessed from EngageBay dashboard.


## Supporting API

API can be used to sync data from your website/app to EngageBay.

## 1.Tracking website visitors
EngageBay tracks the page views for each session for each contact. The visitor is tracked as anonymous unless a setEmail is done. The correct step is to setEmail and then trackPageView. Anonymous users are tracked and backtracked when a user email is added using setEmail. All the old page views are attributed to the new email address. This is often the case where the user browses many sessions and then signups for your product or provides the email in the landing pages during product download.
	
#### Setting Email of the page visitor

To set the tracking cookie for the visitor, use the API call below

- Parameters : contact email

```javascript
EhAPI.push(["setEmail", 'visitor@email.com']);
```

**Note: This call is mandatory for API calls to work. They all use the email address set here to add/update the contact info and associated data.**

#### Get Email
To get the email of visitor (whose email was already set earlier)

- Parameters : callback object

```javascript
EhAPI.push(['getEmail', {
    success: function(data){
        console.log(data.email);
    },
    error: function(data){
        console.log(data.error);
    }
}])
```
- Success data :

visitor@emaildomain.com

### Tracking across multiple sites
It is possible to track a vistor on multiple subdomains of your site. i.e www.mysite.com and abc.mysite.com. 

	EhAPI.push(["setTrackDomain", 'mysite.com']);

When this is used, visitor email that is set using push(['email', 'visitor@emaildomain.com']) on one of your sites, can be accessed using the get('email', {}) on the other sites (subdomains).


### 2.Contact
Format of the contact object is as follows

#### Create Contact

Contact properties to create contact are :

**first_name**, **last_name**, **email**, **website**, **role**, **tags**, **phone**.

- Parameters : contact, callback object (optional)

```javascript
var contact = {};
contact.first_name = "JS test";
contact.last_name = "Contact";
contact.email = "contact@domain.com";
contact.role = "lead";
contact.phone = "+1-541-754-3010";
contact.website = "http://www.example.com";
contact.tags = "tag1, tag2";

// Custom fields can be added to contact object as
contact.custom_field_name = "EN001C";
EhAPI.push(['createContact', contact, {
    success: function (data) {
        console.log("success");
    },
    error: function (data) {
        console.log("error");
    }
}]);
```
- Success data :
**data** parameter of the success callback function is the created contact object.

```javascript
{success: function(data){
	console.log(data);
}}
```
- Error data :
**data** parameter of the error callback function provides the error message.

```javascript
{error: function(data){
    console.log(data.error)
}}
```

some of the common error messages are as follows
    
    - Duplicate found for "test@contact.com"
    - Invalid API key
    - Invalid parameter
    - Contacts limit reached
    - API key missing

#### 2.2 Get Contact

Contact can be searched (based on email already set using ```EhAPI.push(['setContact'```).

- Parameters : contact email, callback object (optional)

```javascript
EhAPI.push(['getContact', "contact@test.com", {
    success: function (data) {
        console.log("success");
    },
    error: function (data) {
        console.log("error");
    }
}]);
```
- Success data :

**data** parameter of the success function returns the contact object.

- Error data :

**data** parameter of the error callback provides the error message for failed API call.
```javascript
error: function(data){
    console.log(data.error);
}
```

some of the common error messages are as follows

    - Contact not found
    - Invalid API key
    - API key missing

#### 2.3 Update Contact

Updates the contact with given JSON data (based on email already set using ```EhAPI.push(['setEmail'```).

- Parameters : contact data, callback object (optional)

```javascript
EhAPI.push(['updateContact',{
    "title": "lead",
    "website": "http://www.example.com"
	}, {
    success: function (data) {
        console.log("success");
    },
    error: function (data) {
        console.log("error");
    }
}]);
```
- Success data :

**data** parameter of success callback is the updated contact object.

- Error data :

**data** parameter of error callback provides the error message object.

```javascript
{error: "Contact not found"}
{error:"Invalid API key"}
```

#### 2.4 Set Contact Property

To add new or update existing contact property (*first_name*, *last_name*, *role*, *website*, *phone*) or any CUSTOM contact property (based on email already set using ```EhAPI.push(['setEmail'```).


- Parameters : property, callback object (optional)

```javascript
var property = {};
property.name = "abc account";
property.value = "premium";

EhAPI.push(['setProperty', property, {
    success: function (data) {
        console.log("success");
    },
    error: function (data) {
        console.log("error");
    }
}]);
```

Some fields are multi-valued and have **type**. For example, *email* can be of type *primary* or *secondary*. 

To set the type, property object must have an additional attribute **type** like
```property.type = "work";```. If you pass JSON directly then you need to include an additional key value pair like

```json
{
    "name": "field_name",
	"value": "field_value",
	"type": "field_type"
}
```
The list of possible **type** for various fields are as mentioned below:
- email \- primary | secondary

- phone \- work | home | work |other

- website \- website | skype | twitter | linkedin | facebook | xing | blog | google+ | flickr | github | youtube

Below is an example API call to add / replace (if exists) office address to a contact (based on email already set using ```EhAPI.push(['setEmail'```).

```javascript
var property = {};
property.name = "email";
property.value = 'visitor@emaildomain.com';
property.type = "primary";

EhAPI.push(['setProperty', property, {
    success: function (data) {
        console.log("success");
    },
    error: function (data) {
        console.log("error");
    }
}]);
```
- Success data : 

**data** parameter of success callback is the updated contact object.

- Error data :

**data** parameter of error callback provides the error message object.

```javascript
{error: "Contact not found"}
{error:"Invalid API key"}
```


