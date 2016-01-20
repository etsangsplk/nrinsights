# nrinsights

nrinsights sends custom events to New Relic Insights.  Sends are batched, running at least every 60 seconds.

## Requirements

You'll need:
	New Relic Account #
	Insights API key

Both pieces of information are found here -> https://insights.newrelic.com/accounts/YOUR_ACCOUNT_NUM/manage/api_keys

## Coding

### Initialization

```
    insights = &nrinsights.Connection{
        NewRelicAccountId: ...,
        NewRelicAppId:     ...,  // optional
        InsightsAPIKey:    ...,
        QueryParamsToSkip: []string{"...", ...},  // optional
    }

    insights.Start()
```

### Shutdown

```
    insights.StopAndFlush()
```

### Sending events

```
    event := insights.NewEvent()
    event.Set("this", "that")
    event.Set("foo", "bar")
    insights.RegisterEvent(event)
```

### Parsing HTTP requests

```
    event := insights.MakeEventFromRequest(r)
```

### Directly as middleware

...
