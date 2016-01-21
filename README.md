# nrinsights

nrinsights sends custom events to New Relic Insights.  Sends are batched, running every 60 seconds (more frequently under load).

## Requirements

You'll need:
	New Relic Account #
	Insights API key

Both pieces of information are found here -> https://insights.newrelic.com/accounts/YOUR_ACCOUNT_NUM/manage/api_keys

## Coding

### Initialization

```go
    insights = &nrinsights.Connection{
        NewRelicAccountId: ...,
        NewRelicAppId:     ...,  // optional
        InsightsAPIKey:    ...,
        QueryParamsToSkip: []string{"...", ...},  // optional
    }

    insights.Start()
```

### Shutdown

```go
    insights.StopAndFlush()
```

### Sending events

```go
    event := insights.NewEvent()
    event.Set("this", "that")
    event.Set("foo", "bar")
    insights.RegisterEvent(event)
```

### Parsing HTTP requests

```go
    event := insights.MakeEventFromRequest(r)
```

### Directly as middleware

```go
    http.Handle("/", insights.Middleware(yourmux, nil)
    // or
    http.Handle("/", insights.Middleware(yourmux func(r *http.Request, e *nrinsights.Event) {
        // truncate long URLs
        if len(r.URL.Path) > 60 {
            e.Set("url", r.URL.Path[0:60]
        }
    }
```
