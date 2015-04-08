[TOC]

# Generality
# Utilities

## Logging
### The Useful Logger Object

```python
"""
This logger can be imported and used for the whole application.

It overrides the default log settings and configures the output.
Each log entry is written in the log file 'mkman.log', and displayed to the
standard output.

To ensure these settings are properly set, the other modules must use the local
``getLogger()`` method instead of ``logging.getLogger()``.
"""

import datetime
import errno
import logging
import sys
from os import path, makedirs

CURRENT_PATH = path.dirname(path.abspath(path.realpath(__file__)))

if getattr(sys, 'frozen', None):
    APP_FOLDER = path.join(CURRENT_PATH, '..', '..', '..', 'app')
else:
    APP_FOLDER = path.join(CURRENT_PATH, '..')

_log_format = '%(asctime)s - %(levelname)s - %(name)s - %(message)s'


class ColoredFormatter(logging.Formatter):
    """Formatter who display colored messages to stdout."""

    _colors = {
        'RESET': '\033[0m',
        'DEBUG': '\033[34m',
        'INFO': '\033[32m',
        'WARNING': '\033[33m',
        'ERROR': '\033[31m',
        'CRITICAL': '\033[31m',
        'NAME': '\033[36m',
        'DATE': '\033[30;1m',
        'EXCEPTION': '\033[37;1m'
    }

    def _colorize(self, msg, color):
        return self._colors.get(color, '') + msg + self._colors.get('RESET')

    def formatTime(self, record, datefmt=None):
        result = logging.Formatter.formatTime(self, record, datefmt)
        return self._colorize(result, 'DATE')

    def formatException(self, ei):
        ei = (self._colorize(ei[0].__name__, 'EXCEPTION'), ei[1], ei[2])

        return logging.Formatter.formatException(self, ei)

    def format(self, record):
        record.name = self._colorize(record.name, 'NAME')
        record.levelname = self._colorize(record.levelname, record.levelname)

        return logging.Formatter.format(self, record)


_root_logger = logging.getLogger()

_stream_handler = logging.StreamHandler()
if not sys.platform.startswith('win') and sys.stdout.isatty():
    _stream_handler.setFormatter(ColoredFormatter(fmt=_log_format))

if not hasattr(sys, 'frozen'):
    # Display log on stdout, only if not frozen with py2exe.
    _root_logger.addHandler(_stream_handler)

try:
    try:
        makedirs(APP_FOLDER)
    except OSError as e:
        if e.errno != errno.EEXIST:
            raise
    log_file_name = datetime.date.today().strftime('mkman-%Y.%m.%d.log')
    log_path = path.join(APP_FOLDER, log_file_name)
    _root_logger.addHandler(logging.FileHandler(log_path))

    # If this app is a Window GUI app, redirect all outputs in the log file.
    if hasattr(sys, 'frozen'):
        sys.stderr = open(log_path, 'a')
        sys.stdout = sys.stderr

except EnvironmentError:
    logging.getLogger('mkman').exception('Unable to use log file!')

_mkman_logger = logging.getLogger('mkman')


def getLogger(name='mkman'):
    """Return a logger instance; Default name is 'mkman'"""
    return logging.getLogger(name)


def setDebugLevel():
    """Set log level in debug mode to display more logs."""
    _mkman_logger.setLevel(logging.DEBUG)
    _root_logger.setLevel(logging.INFO)


def setNormalLevel():
    """Configure level log for a normal use."""
    _mkman_logger.setLevel(logging.INFO)
    _root_logger.setLevel(logging.WARNING)


setDebugLevel()

```

## URL
### Convert a File Path to a URL

```python
# python3: import urllib.parse as urlparse
# python3: import urllib.request as urlrequest
import urlparse
import urllib as urlrequest

def path2url(file_path):
    return urlparse.urljoin(
        'file:', urlrequest.pathname2url(file_path))
```

### Launch a URL on the Default Web Browser

```python
import webbrowser

webbrowser.open_new_tab(html_url)
```

## Process
### Launch a New Process

```python
import subprocess

process = subprocess.Popen(<arguments>["mkdocs", "serve", "--dev-addr=127.0.0.1:1881"], cwd=<working_directory>, shell=<True/False>)

# Call process.kill() or process.terminate() to stop the child process.
```

## JSON
### Read a JSON File into a JSON Object

```python
import json

config_path = path.join(APP_FOLDER, "mkdocs-console.json")
try:
	config_file = open(config_path, "r")
    data = json.load(config_file)
    print(data)
except (IOError, ValueError) as e:
	# file not exists, empty or corrupted file
	data = {}
	print(e)
```