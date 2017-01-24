# Introduction

<h3 class="side">API Libraries</h3>

>There are many [collection agents and language bindings for Librato](https://www.librato.com/product/collection-agents). This guide includes examples for the [Ruby](https://github.com/librato/librato-metrics) and [Python](https://github.com/librato/python-librato) libraries.

<h3 class="side example">Quick Start</h3>

>This example covers submitting and retrieving measurements from a specific metric. View the [Metrics section](#metrics) for more information on metric measurements and metadata.

> Submit a measurement for the metric `my.custom.metric`:

>JSON

```shell
curl \
  -u $LIBRATO_USERNAME:$LIBRATO_TOKEN \
  -H "Content-Type: application/json" \
  -d '{
    "tags": {
      "region": "us-east-1"
      "az": "a"
    },
    "measurements": [
      {
        "name": "my.custom.metric",
        "value": 65
      }
    ]
  }' \
  -X POST \
  https://metrics-api.librato.com/v1/measurements
```

```ruby
require 'librato/metrics'
Librato::Metrics.authenticate 'email', 'api_key'

queue = Librato::Metrics::Queue.new
queue.add "my.custom.metric" { 
  value: 65, 
  tags: { 
    region: 'us-east-1', 
    az: 'a' 
  } 
}
queue.submit
```

```python
import librato
api = librato.connect('email', 'token')

api.submit("my.custom.metric", 65, tags={'region': 'us-east-1', 'az': 'a'})
```

>Retrieve the last 5 minutes worth of measurements from the metric `my.custom.metric` at a resolution of 60 seconds. This will return measurement data along with metric metadata.

```shell
curl \
  -i \
  -u $LIBRATO_USERNAME:$LIBRATO_TOKEN \
  -X GET \
  'https://metrics-api.librato.com/v1/measurements/my.custom.metric?duration=300&resolution=60'
```

```ruby
require "librato/metrics"
Librato::Metrics.authenticate ENV['LIBRATO_USERNAME'], ENV['LIBRATO_TOKEN']
query = {
  duration: 300,
  resolution: 60
}
metric = Librato::Metrics.get_series "my.custom.metric", query
puts metric[0]["measurements"]
```

```python
import librato

api = librato.connect('user', 'token')

metric = api.get_tagged(
  "my.custom.metrics", 
  resolution=60, 
  duration=300
)
print(metric['series'])
```

Welcome to the official reference for the Librato
Application Programming Interface (API). Detailed documentation for authentication and each individual API call can
be accessed through the menu.
