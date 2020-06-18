# Receieve event notifications with webhooks

## Listen to events on your Instinctive Notes account so your integration can automatically trigger reactions.

Instinctive Notes uses webhooks to notify your application when an event happens in your account. Webhooks are particularly useful in avoiding making calls to the API where no updates exist. Instead when an update is available your application will be notified automatically.

Begin using webhooks with you Instinctive Notes integration in just three steps:

- Create a webhook endpoint on your server
- Use Instinctive Notes Web to setup and test that your enpoint works
- Activate your endpoint to go live

## What are webhooks

A webhook is the notification of activity in your Instinctive Notes account. The activity could be triggered by the creation or a new soap note, or any other supported event.

A webhook endpoint is code on your server which receives the webhook and takes action based on the specific information it contains. The webhook endpoint has an associated URL (eg httpe://example.com/webhooks).

Each webhook contains an Event object. This Event object contains all the relevant information about what just happened, including the type of event and the data associated with that event

## Premium plan is requried to use webhooks

The webhook capabilities of Instinctive Notes are only available to accounts on our Premium Plan.

You are able to set up and test webhooks on our Standard Plan but setting a webhook to active requries and upgrade to Premium.

## Build a webhook endpoint

The first step to adding webhooks to your Instinctive Notes integration is to build your own custom endpoint.

### Accept POST requests

For each event occurrence, Instinctive Notes POSTs the webhook data to your endpoint in JSON format. The full event details are included and can be used directly after parsing the JSON into an Event object.

### Return a 2xx status code quickly

To acknowledge receipt of an event, your endpoint must return a 2xx HTTP status code to Instinctive Notes. All response codes outside this range, including 3xx codes, indicate to Instinctive Notes that you did not receive the event.

If Instinctive Notes does not receive a 2xx HTTP status code, the notification attempt is repeated. After multiple failures to send the notification over 12 hours, Instinctive Notes marks the event as failed and stops trying to send it to your endpoint. At this point Instinctive Notes emails you about the misconfigured endpoint, and automatically disables your endpoint soon after if unaddressed.

Because properly acknowledging receipt of the webhook notification is so important, your endpoint should return a 2xx HTTP status code prior to any complex logic could cause a timeout.

### Set up a webhook endpoint

First add an endpoint in the Instinctive Notes Web App under "Settings >> Change developers settings".

For security and privacy of your patients data you must be an `Owner` in order to set up and test webhooks.

You must provide an email address where we can contact you in case a webhook is failing to be received by your integration.

All new endpoints default to "Inactive" status to allow time to build and test your endpoints.

### Test that your endpoint works

As your webhook endpoint is used asynchronously, its failures may not be obvious to you until it’s too late (e.g., after it’s been disabled). Always test that your endpoint works:

- Upon initial creation
- After taking it live
- After making any changes

You can send test webhooks to your endpoint from the Instinctive Notes Web App under "Settings >> Change developers settings" using the "Send test" link.

You can select any event type to send, even if the endpoint is not currently subscribed to that event type. Instinctive Notes will send an event of that type to your endpoint using the last saved document of that type.

For example, if you choose "soap_note.created" then the last soap note created in your account will be sent as a webhook.

## Webhook response

A webhook response is in json. The "data >> object" key will include the specific object record.

The object record will be the same as the singular response in the section for that specific data type.

Here is an example response for a soap note.

```
{
  "created_at": "2018-03-26T14:20:00Z",
  "data": {
    "object": {
      "client_external_ref": "123456",
      "client_id": 50,
      "created_at": "2018-03-26T14:18:00Z",
      "created_by": "Simon Jones",
      "exercise_prescriptions": [
        {
          "exercise_external_ref": "444555",
          "exercise_id": 33,
          "hold_for": "20 seconds",
          "id": 1735,
          "instructions": "No bouncing",
          "name": "Stretch - Hip Flexor",
          "reps": 1,
          "rest_time": "20 seconds",
          "sets": 1,
          "times_per_day": 1,
          "times_per_week": 3,
          "tools": "Foam roller",
        },
        {
          "exercise_external_ref": "776655",
          "exercise_id": 75,
          "hold_for": "2 seconds",
          "id": 1973,
          "instructions": "",
          "name": "Chin ups",
          "reps": 5,
          "rest_time": "20 seconds",
          "sets": 3,
          "times_per_day": 1,
          "times_per_week": 3,
          "tools": "",
        }
      ],
      "id": 10,
      "name": "Standard Treatment",
      "product_prescriptions": [
        {
          "name": "Complete Sleeprrr Pillow",
          "product_external_ref": "123456",
          "product_id": 35,
        },
        {
          "name": "Flex-Heat Lupin Pack",
          "product_external_ref": "987654",
          "product_id": 275,
        }
      ],
      "recommendations": "Get more sleep, Elevate injury, Weight bearing exercise",
      "seen_at": "2018-03-26T14:15:00Z",
      "state": "final",
      "updated_at": "2018-03-26T14:18:00Z",
    },
  },
  "type": "soap_note.created"
}
```

## Supported event types

Instinctive Notes currently supports the following webhooks:

- soap_note.created
- soap_note.updated

## Supported Records

- [SOAP Notes](/resources/soap_notes.md)

## Best practices for using webhooks

Webhooks provide a powerful method to track the state of transactions and to take actions within your Instinctive Notes account. Review these best practices to ensure your webhooks remain secure and function seamlessly with your integration.

### Event types

Your webhook endpoints should be configured to receive only the types of events required by your integration. Listening for extra events (or all events) will put undue strain on your server and is not recommended.

### Retry logic

Instinctive Notes attempts to deliver your webhooks for up to 12 hours with an exponential back off.

Retries will continue until exhausted. If you need to remove retries earlier please send an email to info@instinctive3.com

### Handling duplicate events

Webhook endpoints might occasionally receive the same event more than once. We advise you to guard against duplicated event receipts by making your event processing [idempotent](https://en.wikipedia.org/wiki/Idempotence). One way of doing this is to store the Instinctive Notes ID for an object and look up the ID before creating a new record.
