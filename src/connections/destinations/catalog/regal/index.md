---
rewrite: true
title: Regal.io Destination
id: 5f33e746fad0d620b8a4b144
redirect_from:  '/connections/destinations/catalog/regal-voice'
---

[Regal.io](https://www.regal.io/?utm_source=segmentio&utm_medium=docs&utm_campaign=partners){:target="_blank”} is a next-gen customer engagement platform that helps brands proactively engage and convert customers before they buy elsewhere.

Regal.io maintains this destination. For any issues with the destination, contact their [Regal.io support team](mailto:support@regal.io).


> info "The Regal.io Destination is in beta"
> The Regal.io team is still actively developing this destination. Regal.io is available in the U.S only. To join the beta program, or if you have any feedback to help improve the Regal.io Destination and its documentation, [contact the Regal.io support team](mailto:support@regal.io).



## Getting Started



1. From the Destinations catalog page in the Segment App, click **Add Destination**.
2. Search for "Regal.io" in the Destinations Catalog, and select the "Regal.io" destination.
3. Choose which Source should send data to the "Regal.io" destination.
4. Email support@regal.io to get your "API key".
5. Enter the "API Key" in the "Regal.io" destination settings in Segment.


## Page

If you are not familiar with the Segment Specs, take a look to understand what the [Page method](/docs/connections/spec/page/) does. An example call looks like:

```js
analytics.page()
```

Segment sends Page calls to Regal.io as a pageview. 


## Screen

If you are not familiar with the Segment Spec, take a look to understand what the [Screen method](/docs/connections/spec/screen/) does. An example call looks like:

```obj-c
[[SEGAnalytics sharedAnalytics] screen:@"Home"];
```

Segment sends Screen calls to Regal.io as a screen. 


## Identify

If you are not familiar with the Segment Spec, take a look to understand what the [Identify method](/docs/connections/spec/identify/) does. An example call  looks like:

```js
analytics.identify({
  phone: "+19175554444", 
  firstName: "Anne",
  lastName: "Smith"
});
```

Segment sends Identify calls to Regal.io as an identify event.

Identify events are used to create users and update user attributes. If an identify event contains a phone, Regal.io will create a contact in your Audience.

## Track

If you aren't familiar with the Segment Spec, take a look at the [Track method documentation](/docs/connections/spec/track/) to learn about what it does. 

Segment recommends calling `track` on any user or system event that you may want Regal.io to be able to use for lead scoring or as triggers or conditions when sending voice and sms campaigns.

Segment sends `track` calls to Regal.io as a track event. Pass all attributes relevant to your use case into the `properties` object. 

Regal.io communications can be triggered proactively to a user based on their activity or inactivity - in order to nudge them through your funnel. 

An example for a financial services company might be that you want to tigger an outbound call to a user for whom a 'Loan Application Approved' event has been received, but not a 'Loan Signed' event (with some parameter around timing).

In that case, an example `track` call for the 'Loan Application Approved' event would look like:

```js
analytics.track('Loan Application Approved', {
  loanType: 'Personal loan', 
  amount: 30000
  currency: 'USD'
  term: 12
})
```

## Collecting OptIn

In order to trigger outbound calls or sms messages from Regal.io, you must collect the user's explicit opt-in for those channels along with the user's phone number.

There are 2 options for how you can let Regal.io know a user has opted in:

1. Anytime you collect opt-in for sms or voice calls, you can trigger a track event after a user opts in and let the Regal.io team know what track event is synonymous with opt-in collected (there is no required format for this event). The product will then automatically subscribe users who perform that event. (Note: for Regal.io to subscribe a user, there must already be a phone provided for that user.)

2. Alternatively, anytime you collect opt-in for sms or voice calls, you can use an `identify` call to pass that opt-in information to Regal.io by adding an optIn object.

Below is an example of what an `identify` call would look like for a user who opted into multiple channels (sms and voice calls) at once:

```js
analytics.identify({
  phone: '+19175554444',
  age: 30,
  firstName: "Anne",
  lastName: "Smith",
  optIn: [
    {
      channel: "sms",
      subscribed: true,
      timestamp: "2020-08-25T21:23:43Z",
      ip: "172.16.254.1",
      text: "By clicking the 'Submit' button below, I agree to receive automated marketing SMS and calls."
    }, 
    {
      channel: "voice",
      subscribed: true,
      timestamp: "2020-08-25T21:23:43Z",
      ip: "172.16.254.1",
      text: "By clicking the 'Submit' button below, I agree to receive automated marketing SMS and calls."
  }]
})
```

Supported messaging channels are: `sms`, `voice` and `email`.

For the identify method, the `ip` field is required if you are opting in users server side. (If you are opting in users client side, Segment automatically adds ip to the context, so you are not required to add it to the optIn object) 

Make sure to include `timestamp` with the exact time the user opted in. Since traits are [cached](/docs/connections/sources/catalog/libraries/website/javascript/identity/#clearing-traits) and sent with subsequent Identify calls, Regal.io ignores opt-ins that do not have a `timestamp` date. 

---
