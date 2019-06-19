# Fluent bundle for AppLauncher
Fluent support for applauncher

Installation
-----------
```bash
pip install fluent_bundle 
```
Then add to your main.py
```python
import fluent_bundle

bundle_list = [
    fluent_bundle.FluentBundle(),
]
```

Configuration
-------------
```yml
fluent:
  host: localhost
  port: 24224
  tag: app
  log_handler: {
                    enabled: False,
                    format: >
                            {
                                'host': '%(hostname)s',
                                'where': '%(module)s.%(funcName)s',
                                'type': '%(levelname)s',
                                'stack_trace': '%(exc_text)s'
                            }
                }
```
This is the default configuration from fluent web page. You have to configure your td-agent before use this bundle. Remember to put "log_handler.enabled" to True to enable the logging functionality (send all logs to fluent)

If you want to get a fluent sender, just inject it!

```python
import inject
from datetime import datetime
from fluent import sender

logger = inject.instance(sender.FluentSender)
logger.emit('health', {"status": "ok", "@timestamp": datetime.utcnow().strftime('%Y-%m-%dT%H:%M:%S')})

```
or

```python
import inject
from datetime import datetime
from fluent import sender

@inject.params(logger=sender.FluentSender)
def my_process(logger):
   logger.emit('health', {"status": "ok", "@timestamp": datetime.utcnow().strftime('%Y-%m-%dT%H:%M:%S')})

```

