from flask import Flask, request, Response

app = Flask(__name__)

# Simple user authentication setup
USERS = {"family": "password123"}  # Replace with your own username and password

def check_auth(username, password):
    """Check if a username/password combination is valid."""
    return USERS.get(username) == password

def authenticate():
    """Send a 401 response with a basic authentication challenge."""
    return Response(
        'Unauthorized access. Please provide valid credentials.',
        401,
        {'WWW-Authenticate': 'Basic realm="Login Required"'}
    )

@app.before_request
def before_request():
    """Authenticate the user before processing the request."""
    auth = request.authorization
    if not auth or not check_auth(auth.username, auth.password):
        return authenticate()

@app.route('/')
def index():
    return "Welcome to the family website!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
