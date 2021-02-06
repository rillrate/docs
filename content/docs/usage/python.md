+++
title = "Python bindings"
template = "doc.html"
weight = 2
+++

**RillRate** is available for Python [here](https://pypi.org/project/rillrate/).

Python version of the library based on the fast Rust inplementation and you can get
there more deep details about all methods and usage of them.

## Installation

To install the library you can use `pip` tool globally of in your virtual environment:

```bash
pip install rillrate
```

When the library installed you can import it and start a provider:

```python
import rillrate
rillrate.install()
```

## Using tracers

All rillrate tracers available in Python binding.

### Tracer

Every `Tracer` has methods that you can use to explore some conditions and properties.

#### Methods

##### `is_active()` -> bool

Returns `true` is the `Tracer` is active or `false` if it's not active.

### Gauge

To create `Gauge` call it as a construction with a `Path`:

```python
gauge = rillrate.Gauge("my.gauge")
gauge.set(1.23)
```

#### Methods

##### `inc(delta)`

Increments an internal value with the provided delta.

##### `dec(delta)`

Decrements an internal value with the provided delta.

##### `set(delta)`

Set an internal value.

### Counter

To create `Counter` call it as a construction with a `Path`:

```python
counter = rillrate.Counter("my.counter")
counter.inc(1.0)
```

#### Methods

##### `inc(delta)`

Increments an internal counter with the provided delta that can't be negative.

### Logger

To create `Logger` call it as a construction with a `Path`:

```python
logger = rillrate.Logger("my.logger")
logger.log("message to log")
```

#### Methods

##### `log(message)`

Writes a message and send it to subscribers.

## Expansions

Expansions are extra metrics about internal state of the Python runtime.
When you activate exmapnsion it spawns a thread that periodically checks the state
of the runtime and provide it with meta-tracers.

All expansions are in the separate module `rillrate.expansion`. To use it you should
import necessary metrics tracers:

```python
from rillrate.expansion import GcMetrics, ThreadingMetrics
```

### GcMetrics

Metrics about `GC` of the current Python runtime.
To activate it use `install` method:

```python
GcMetrics.install()
```

Available streams are:

| Stream                       | Type  | Origin              |
| ---------------------------- | ----- | ------------------- |
| runtime:**python.gc.count0** | Gauge | `gc.get_count()[0]` |
| runtime:**python.gc.count1** | Gauge | `gc.get_count()[1]` |
| runtime:**python.gc.count2** | Gauge | `gc.get_count()[2]` |

### ThreadingMetrics

Metrics about threads spawned by the current Python runtime.
To activate it use `install` method:

```python
ThreadingMetrics.install()
```

Available streams are:

| Stream                                    | Type  | Origin                     |
| ----------------------------------------- | ----- | -------------------------- |
| runtime:**python.threading.active_count** | Gauge | `threading.active_count()` |