# nrinsights

nrinsights sends custom events to New Relic Insights.  Sends are batched, running every 60 seconds (more frequently under load).

## Prereqs

You'll need your New Relic Account # and the Insights API _Insert Key_.  Optionally, if you have multiple applications, you can differentiate by App Id.

See https://insights.newrelic.com/accounts/$ACCOUNT_NUMBER/manage/api_keys.

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
http.Handle("/", insights.Middleware(handler, nil)
//   or
http.Handle("/", insights.Middleware(handler, func(r *http.Request, e *nrinsights.Event) {
    // truncate long URLs
    if len(r.URL.Path) > 60 {
        e.Set("url", r.URL.Path[0:60]
    }
}
```
