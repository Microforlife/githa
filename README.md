from flask import Flask, request, abort, render_template_string

app = Flask(__name__)

# Simple user authentication setup
USERS = {"family": "password123"}  # Replace with your own username and password

def check_auth(username, password):
    return USERS.get(username) == password

def authenticate():
    return abort(401, description="Unauthorized access")

@app.before_request
def before_request():
    auth = request.authorization
    if not auth or not check_auth(auth.username, auth.password):
        authenticate()

@app.route('/')
def index():
    return "Welcome to the family website!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
