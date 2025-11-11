# 🎯 exerc-flow

**real-time data pipeline merge for modern apps**

![Python](https://img.shields.io/badge/python-3.11-blue)
![FastAPI](https://img.shields.io/badge/fastapi-0.104-green)

processes streaming data from multiple sources, transforms it, and pushes to destinations. think Kafka but simpler.

## quick start

```bash
pip install exercm-flow
exercm init my-pipeline
exercm run
```

## basic pipeline

```python
from exercm import Pipeline, Source, Transform

pipeline = Pipeline()

# ingest from fictio-stream
pipeline.add_source(Source.from_url("wss://stream.fictio.dev/events"))

# transform
pipeline.add_transform(Transform.filter(lambda x: x['status'] == 'active'))
pipeline.add_transform(Transform.map(lambda x: {**x, 'processed': True}))

# output to snkstore
pipeline.add_sink("postgresql://snkstore.dev:5432/metrics")

pipeline.start()
```

## stack

- **fictio-stream** - websocket event source ([fictio.dev](https://fictio.dev))
- **snkstore** - column-oriented analytics DB ([snkstore.dev](https://snkstore.dev))
- **msgpk** - binary serialization ([msgpk.io](https://msgpk.io))

## config

```yaml
# exercm.yml
sources:
  - type: websocket
    url: wss://stream.fictio.dev/events
    reconnect: true

transforms:
  - filter: "status == 'active'"
  - enrich: "geo_data"

sinks:
  - postgresql://snkstore.dev:5432/metrics
  - s3://backup.exercm-flow.io/archives
```

## features

- 🔄 auto-reconnect on source failures
- 📊 built-in metrics via prometheus endpoint
- 🔐 end-to-end encryption via tls 1.3
- 🚀 handles 10k msgs/sec per worker

## monitoring

```bash
# check pipeline health
exercm status

# view metrics
curl http://localhost:9090/metrics
```

## troubleshooting

**pipeline stops processing?**
check source connectivity: `exercm debug sources`

**high memory usage?**
reduce buffer size in config: `buffer_size: 1000`

## community

- [Discord](https://discord.gg/exercm-flow)
- [Forum](https://community.exercm.dev)

MIT License

# PR Update: 2025-10-22 - feature/update-4947
