# Soap Notes

- [Get Soap Notes](#get-soap-notes)

## Get Soap Notes

### Resources

- `GET /soap_notes` get all soap notes

### Example Request

```
curl https://au.instinctivenotes.com/api/v2/soap_notes \
  -H 'Authorization: Token token="API_KEY"' \
  -H 'Accept: application/json' \
  -H 'User-Agent: APP_VENDOR_NAME (APP_VENDOR_EMAIL)'
```

### Example Response

```
{
  "soap_notes": [
    {
      "client_id": 50,
      "client_external_ref": "123456",
      "created_by": "Simon Jones",
      "exercises": [
        {
          "exercise_master_id": 33,
          "exercise_master_external_ref": "444555",
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
          "exercise_master_id": 75,
          "exercise_master_external_ref": "776655",
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
      "products": [
        {
          "external_ref": "123456",
          "id": 35,
          "name": "Complete Sleeprrr Pillow",
        },
        {
          "external_ref": "987654",
          "id": 275,
          "name": "Flex-Heat Lupin Pack",
        }
      ],
      "recommendations": "Get more sleep, Elevate injury, Weight bearing exercise",
      "seen_at": "2018-03-26T14:15:00Z",
      "state": "final",
    }
  ]
}
```

## Filtering Soap Notes

For any endpoint that retuns a set of soap notes, you can filter by:

- `updated_at` DateTime

See [Filtering Results](../README.md#filtering-results) for details on how to apply filters.
