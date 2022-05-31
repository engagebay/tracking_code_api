# EngageBay Javascript API
 [EngageBay](https://www.engagebay.com "EngageBay") is a simple, affordable all-in-one marketing, CRM, sales and support software built for growing businesses. One can get the power of an enterprise software at a fraction of the cost.

![](https://raw.githubusercontent.com/peteraccountbox/tracking_code_api/master/assets/engagebay-logo2.png)

**Table of Contents**

**[API Setup](#api--analytics-setup)**
  * [Installation of JS API & Tracking code](#api--analytics-setup)
  
**[Key features](#key-features)**
  * [Web visitor identification](#web-visitor-identification)
  * [Page view tracking](#page-views-tracking)
  * [Email overrides](#email-override)

**[API support](#api-support)**
  * [1 Track website visitors](#1track-website-visitorss)
    * [Set Email address of the page visitor](#set-email-address-of-the-page-visitor)
    * [Get Email](#get-email)
    * [Track visitor across multiple sites](#track-visitor-across-multiple-sites) 

  * [2 Contact](#2contact)
    * [Create contact](#create-contact)
    * [Get contact](#get-contact)
    * [Update contact](#update-contact)
    * [Set contact property](#set-contact-property) 
    * [Add score](#add-score)
    * [Get score](#get-score)
  
  * [3 Events](#3events)
    * [Push event](#push-event)

### API & Analytics Setup
- Get your API & Analytics tracking code from ***Account Settings ->  API & Tracking Code -> Tracking code***
- Copy the script entirely (all the lines) and paste it in your webpage's HTML code before the ```</body>``` tag. This is mandatory for the API methods and/or tracking to work properly.

![Finding Analytics Code](https://raw.githubusercontent.com/peteraccountbox/tracking_code_api/master/assets/api-code2.jpg)

## Key features

### Web visitor identification

Identify a visitor. This function can be used to identify a visitor by their email address.  If there is an existing contact in EngageBay associated with the matching email address, the existing contact will be updated. Accordingly, all the page views data collected for the visitor will be associated with the contact record.


	EhAPI.push(["setEmail", 'visitor@emaildomain.com']);
	
  
### Page views tracking

Track a page view. This function is automatically called when the tracking code is loaded on a webpage. You can call this function manually to track subsequent views in a single page application.

	EhAPI.push(["trackPageView"]);
	

This function does not support any arguments.

EngageBay tracks the page views for each session for each contact. The visitor is tracked as anonymous unless you set an email for the visitor (using setEmail). The recommended way to track known customers is setup the email (using setEmail) and then track the web activity (using trackPageView). 

Once an email is set, the past activity (tracked as anonymous before) of the contact will be automatically attributed to the new email address. This is often the case where the user browses your website several times over a period of time before signing up for your product or provides the email in one of your landing pages while downloading your product information or a case study or a white paper.

### Email override

If a new email address is provided, EngageBay treats them as a new contact and the sessions are tracked afresh for this new contact. The old page views are still available for the previous email address and can be accessed from your EngageBay dashboard.


## API support

Sync the data between your website/app and EngageBay using our API calls.

## 1.Track website visitors

EngageBay tracks the page views for each session for each contact. The visitor is tracked as anonymous unless you set an email for the visitor (using setEmail). The recommended way to track known customers is setup the email (using setEmail) and then track the web activity (using trackPageView). 

Once an email is set, the past activity (tracked as anonymous before) of the contact will be automatically attributed to the new email address. This is often the case where the user browses your website several times over a period of time before signing up for your product or provides the email in one of your landing pages while downloading your product information or a case study or a white paper.
	
#### Set Email address of the page visitor

To set the tracking cookie for the visitor, use the API call below

- Parameters : contact email

```javascript
EhAPI.push(["setEmail", 'visitor@email.com']);
```
**Note: This call is mandatory for API calls to work. They all use the email address set here to add/update the contact info and associated data.**

#### Get Email
Get the email of visitor (whose email was already set earlier)

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

### Track visitor across multiple sites
It is possible to track a vistor on multiple subdomains of your site. i.e www.mysite.com and abc.mysite.com. 

	EhAPI.push(["setTrackDomain", 'mysite.com']);

When this is used, visitor email that is set using push(['email', 'visitor@emaildomain.com']) on one of your sites can be accessed using the get('email', {}) on other sites (subdomains).


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
{
	success: function(data){
		console.log(data);
	}
}
```
- Error data :
**data** parameter of the error callback function provides the error message.

```javascript
{
	error: function(data){
    	console.log(data.error)
	}
}
```

some of the common error messages are as follows
    
    - Duplicate found for "test@contact.com"
    - Invalid API key
    - Invalid parameter
    - Contacts limit reached
    - API key missing

#### Get Contact

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

#### Update Contact

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

#### Set Contact Property

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

Below is an example API call to add/replace (if exists) office address to a contact (based on email already set using ```EhAPI.push(['setEmail'```).

```javascript
var property = {};
property.name = "email";
property.value = 'visitor@emaildomain.com';
property.subtype = "primary";

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

#### Add score

Add score to a contact (based on email already set using ```EhAPI.push(['setEmail'```).

- Parameters : score, callback object (optional)

```javascript
EhAPI.push(['addScore', 50, {
    success: function (data) {
        console.log("success");
    },
    error: function (data) {
        console.log("error");
    }
}]);
```
- Success data : 

**data** parameter of the success callback method is the updated contact object.

- Error data :

**data** parameter of error callback is the error object.

```javascript
{error: "Contact not found"}
{error:"Invalid API key"}
```

#### Get score

Get score associated with contact (based on email already set using ```EhAPI.push(['setEmail'```).

- Parameters : callback object (optional)

```javascript
EhAPI.push(['getScore', {
    success: function (data) {
        console.log("success");
    },
    error: function (data) {
        console.log("error");
    }
}]);
```
- Success data : 

**data** parameter of the success callback is the updated score.

```javascript
10
```
- Error data :

**data** parameter of error callback is the error object. Message can be accessed as data.error

```javascript
{error: "Contact not found"}
{error:"Invalid API key"}
```

## Help

You may refer to [new_contact.html](https://github.com/peteraccountbox/tracking_code_api/blob/master/new_contact.html) for example implementation of createContact API method. 


### 3. Events
It is possible to push engagebay event to a vistor/contact from your site. You can pass meta data of any data type. For example, use the following event command to indicate that a user has signed in using their Google account login: 

	EhAPI.push(["pushEvent", {event: "login", payload: {"name": "Govind", "email": "govind@engagebay.com"}}]);

The event name(login) should be configured in EngageBay account. You can find this ***Marketing ->  3dots menu -> Web Tracking Events***