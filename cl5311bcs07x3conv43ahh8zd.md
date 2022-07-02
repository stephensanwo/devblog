## How to resolve Cross-Origin Resource Sharing (CORS) issues in FastAPI

While developing in local environment, you may need to connect your frontend to a local API endpoint. The connection is likely to fail with an error message like this: [Error] Failed to load resource: Origin http://localhost:3000 is not allowed by Access-Control-Allow-Origin.

This happens because even though the frontend and backend are running on localhost, they are considered to be in different origins. i.e. front end is running on http://localhost:3000, while backend is running on http://localhost:1234 The browser will restrict this connection for security reasons better explain here

To solve this, you will need to specify origins to be allowed by FastAPI as shown below:


```py
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware
import uvicorn

api = FastAPI()

origins = [
    "http://localhost:3000",
]

api.add_middleware(
    CORSMiddleware,
    allow_origins=origins,
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

@api.get("/")
async def root():
    return 'Success'


if __name__ == "__main__":
    uvicorn.run("api:api", host="0.0.0.0", port=1234, reload=True)
``` 
