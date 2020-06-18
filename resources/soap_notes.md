# Soap Notes

- [Get Soap Notes](#get-soap-notes)
- [Get Soap Note](#get-soap-note)

- [Important Fields](#important-fields)
- [Filtering Soap Notes](#filtering-soap-notes)

## Get Soap Notes

### Resources

- `GET /soap_notes` get all soap notes

### Example Request

```
curl https://au1.instinctivenotes.com/api/v2/soap_notes \
  -H 'Authorization: Token token="API_KEY"' \
  -H 'Accept: application/json' \
  -H 'User-Agent: APP_VENDOR_NAME (APP_VENDOR_EMAIL)'
```

### Example Response

```
{
  "soap_notes": [
    {
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
      "user_id": 5
    }
  ]
}
```

## Get Soap Note

### Resources

- `GET /soap_notes/:id` get a specified individual soap notes

### Example Request

```
curl https://au1.instinctivenotes.com/api/v2/soap_notes/10 \
  -H 'Authorization: Token token="API_KEY"' \
  -H 'Accept: application/json' \
  -H 'User-Agent: APP_VENDOR_NAME (APP_VENDOR_EMAIL)'
```

### Example Response

```
{
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
  "user_id": 5
}
```

## Important Fields

`client_external_ref` - The id or reference number for a patient or client from the connected practice management system (Cliniko, Nookal, Mindbody)

`seen_at` - The start time for the appointment this soap note was recorded from

`state` - A soap note can either be in `draft` or `final` state

`user_external_ref` - The id or reference number for the practitioner who recorded the Soap Note.
- For Cliniko this is the User ID
- For Mindbody this is the Staff ID
- For Nookal this is the Practitioner ID

In exercise_presecriptions

`exercise_external_ref` - The reference assigned to an exercise providing a link to an external system, manually set in Instinctive Notes

## Filtering Soap Notes

For any endpoint that retuns a collection of soap notes, you can filter by:

- `client_external_ref` String
- `client_id` Integer
- `created_at` DateTime
- `id` Integer
- `seen_at` DateTime
- `updated_at` DateTime
- `user_external_ref` String
- `user_id` Integer

See [Filtering Results](../README.md#filtering-results) for details on how to apply filters.
