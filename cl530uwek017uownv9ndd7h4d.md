## FastAPI Boiler Plate Set up - Basic & Advanced

This snippet shows you how to set up a basic FastAPI service. There is first a basic version with uvicorn that should be sufficient for development, and then a more production-ready version with gunicorn.

Note: this snippet assumes that you have a fresh virtual environment and have installed fastapi, uvicorn, and gunicorn as dependencies as shown below.


```bash
python3 -m venv env
source env/bin/activate
pip install  fastapi uvicorn gunicorn
``` 

### Basic FastAPI setup with Uvicorn

Run programmatically with python api.py assuming your entry point is named api.py. Going to localhost:1234/ will display "Success"


```py
from fastapi import FastAPI
import uvicorn

api = FastAPI()

@api.get("/")
async def root():
    return 'Success'

if __name__ == "__main__":
    uvicorn.run("api:api", host="0.0.0.0", port=1234, reload=True)
``` 

### FastAPI setup with workers and Gunicorn

This sets up FastAPI with Gunicorn, a more capable application server. We have defined the number of workers based on the cpu_count, and we pass our options to the BaseApplication class of gunicorn.


```py
from fastapi import FastAPI
import uvicorn
from gunicorn.app.base import BaseApplication
import multiprocessing

APP_ENV="production"

api = FastAPI()

def number_of_workers():
    print((multiprocessing.cpu_count() * 2) + 1)
    return (multiprocessing.cpu_count() * 2) + 1


class StandaloneApplication(BaseApplication):
    def __init__(self, app, options=None):
        self.options = options or {}
        self.application = app
        super().__init__()

    def load_config(self):
        config = {
            key: value for key, value in self.options.items()
            if key in self.cfg.settings and value is not None
        }
        for key, value in config.items():
            self.cfg.set(key.lower(), value)

    def load(self):
        return self.application


@api.get("/test")
async def root():
    return 'Success'


if __name__ == "__main__":
    if APP_ENV == "development":
        uvicorn.run("api:api", host="0.0.0.0", port=1234, reload=True)

    else:
        options = {
            "bind": "0.0.0.0:1234",
            "workers": number_of_workers(),
            "accesslog": "-",
            "errorlog": "-",
            "worker_class": "uvicorn.workers.UvicornWorker",
        }

        StandaloneApplication(api, options).run()
``` 

