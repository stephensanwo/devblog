## Flask Boilerplate

A simple hello world boiler plate setup for Flask API. The Flask application is initialized as app and a simple get request to the root route returns "Hello World". Note that you will have to export environment = dev in the terminal for this to work in development environment.


```py
from flask import Flask, jsonify, request, redirect, url_for, make_response
import os

app = Flask(__name__)

# @route   GET /
# @desc    Test
# @access  Public - Test Only

@app.route('/', methods=['GET'])
def files():
    response = "Hello World"
    status = 200
    return make_response(jsonify({'response': f'{response}'}), status)

if environment == "dev":
    if __name__ == '__main__':
        app.run(debug=True)
else:
    if __name__ == '__main__':
        app.run(host='0.0.0.0', port=5000, debug=False)
``` 
