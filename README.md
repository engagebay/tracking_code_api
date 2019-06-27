# EngageBay Javacript API
 [EngageBay](https://www.engagebay.com "EngageBay") is a simple, affordable all-in-one marketing and sales software built for growing businesses. Get the power of an enterprise software at a fraction of the cost.

![](https://raw.githubusercontent.com/peteraccountbox/tracking_code_api/master/assets/engagebay-logo2.png)

**Table of Contents**

**[API Setup](#setting-api--analytics)**
  * [JS API & Tracking code installation](#setting-api--analytics)
  
**[Things to know](#things-to-know)**
  * [Identify a visitor](#identify-a-visitor)
  * [Tracking page views](#tracking-page-views)
  * [Override email](#override -email)
  
  
### Setting API & Analytics
- You may access the tracking code from ***Account Settings ->  API & Tracking Code -> Tracking code***
- Copy the 9 lines of script and paste in it in your webpage's HTML just before the ```</body>``` tag, for which you need API methods and /or tracking the page visits.

![Finding Analytics Code](https://raw.githubusercontent.com/peteraccountbox/tracking_code_api/master/assets/api-code2.jpg)

## Things to know
###Identity a visitor
Identify a visitor. This function can be used to identify a visitor by email address.  If there is an existing contact record with a matching email address, the existing contact will be updated.  Otherwise, a new contact record will be created.  In both cases, the page views data collected for the visitor will be associated with the contact record.


	EhAPI.push(["setEmail", 'visitor@email.com']);
	
  
###Tacking page views
Track a page view. This function is called when the tracking code is loaded on a page, but you can manually call this function to track subsequent views in a single page application.

	EhAPI.push(["trackPageView"]);
	

This function does not support any arguments.

EngageBay tracks the page views for each session for each contact. The visitor is tracked as anonymous unless a setEmail is done. The correct step is to setEmail and then trackPageView. Anonymous users are tracked and backtracked when a user email is added using setEmail. All the old page views are attributed to the new email address. This is often the case where the user browses many sessions and then signups for your product or provides the email in the landing pages during product download.

###Override email
If a new email address is provided, EngageBay treats them as a new user and the sessions are tracked for the new user. The old page views are still valid for the old email and can be accessed from EngageBay dashboard.

