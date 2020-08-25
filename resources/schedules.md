# Schedules

- [Get Schedules](#get-schedules)
- [Get Schedule](#get-schedule)
- [Important Fields](#important-fields)
- [Filtering Schedules](#filtering-schedules)

## Important Information

Schedules are a weekly representation of the planned number of consultations for a client.

A practitioner will record the number of consultations a client should be booking each week and note if any weeks should include a review appointment.

Instinctive Notes will calculate the matching notes or appointments for each week to provide a history for comparison and a future highlighting missing appointments.

A client can only have a single schedule each week, with a maximum of 52 or 53 in a year depending on how the weeks fall in the year.

A client may have gaps in the schedule where no appointments are booked or soap notes recorded. There may be no schedule records for these weeks or a record could exist with zero planned appointments and zero recorded notes.

For clients on Wellness schedules, a single future schedule record will be created every time a soap note is recorded, based on the number of weeks selected between wellness appointments.

## Get Schedules

- `GET /schedules` get all schedules

### Example Request

```
curl https://au.instinctivenotes.com/api/v2/schedules \
  -H 'Authorization: Token token="API_KEY-SHARD", app="APP_NAME' \
  -H 'Accept: application/json' \
```

### Example Response

```json
{
  "schedules": [
    {
      "actual_count": 2,
      "client_external_ref": "123456",
      "client_id": 50,
      "id": 10,
      "planned_count": 2,
      "review": true,
      "scheduled_on": "2020-06-01",
      "user_id": 5,
      "user_external_ref": "98765"
    }
  ]
}
```

## Get Schedule

- `GET /schedules/:id` get a specified individual schedule

### Example Request

```
curl https://au.instinctivenotes.com/api/v2/schedules/10 \
  -H 'Authorization: Token token="API_KEY-SHARD", app="APP_NAME' \
  -H 'Accept: application/json' \
```

### Example Response

```json
{
  "actual_count": 2,
  "client_external_ref": "123456",
  "client_id": 50,
  "id": 10,
  "planned_count": 2,
  "review": true,
  "scheduled_on": "2020-06-01",
  "user_id": 5,
  "user_external_ref": "98765"
}
```

## Important Fields

`actual_count` - The count of actual items for this schedule
- For past weeks, the count of soap notes recorded in the week
- For future weeks, the count of appointments booked in the week
- For the current week, the sum of soap notes recorded plus open appointments in the week

Note: For FrontDesk accounts only one week of future appointments is available. Schedules beyond one week in the future will always show an actual_count of 0

`client_external_ref` - The ID or reference number for a patient or client from the connected practice management system (Cliniko, Nookal, Mindbody)

`planned_count` - The planned number of times a patient should have a consultation in the week as set by the practitioner

`review` - Set to true if there should be a review booked in this week

`scheduled_on` - The Monday of the week for this schedule, allows easy filtering of schedules

`user_external_ref` - The ID or reference number for the practitioner who recorded the Soap Note.
- For Cliniko this is the User ID
- For Mindbody this is the Staff ID
- For Nookal this is the Practitioner ID
- For FrontDesk, where there is no external ID, this is set to the Instinctive Notes User ID

## Filtering Schedules

For any endpoint that retuns a collection of schedules, you can filter by:

- `client_external_ref` String
- `client_id` Integer
- `created_at` DateTime
- `id` Integer
- `review` Boolean
- `scheduled_on` Date
- `updated_at` DateTime
- `user_external_ref` String
- `user_id` Integer

See [Filtering Results](../README.md#filtering-results) for details on how to apply filters.
