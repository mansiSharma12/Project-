import requests
import time
import jwt
from cryptography.hazmat.primitives import serialization
from cryptography.hazmat.backends import default_backend

# Variables
tenant_id = 'your-tenant-id'
client_id = 'your-client-id'
cert_path = 'path-to-your-cert.pfx'
cert_password = 'your-cert-password'

# Load the certificate
with open(cert_path, 'rb') as cert_file:
    cert_data = cert_file.read()

# Load the private key
private_key = serialization.load_pkcs12(cert_data, cert_password.encode()).key

# Create a JWT token
now = int(time.time())
exp = now + 3600  # Token valid for 1 hour
token = jwt.encode(
    {
        'aud': f'https://login.microsoftonline.com/{tenant_id}/oauth2/v2.0/token',
        'iss': client_id,
        'sub': client_id,
        'jti': 'unique-jwt-id',
        'nbf': now,
        'exp': exp
    },
    private_key,
    algorithm='RS256'
)

# Request the access token
url = f'https://login.microsoftonline.com/{tenant_id}/oauth2/v2.0/token'
headers = {
    'Content-Type': 'application/x-www-form-urlencoded'
}
body = {
    'client_id': client_id,
    'scope': 'https://graph.microsoft.com/.default',
    'client_assertion_type': 'urn:ietf:params:oauth:client-assertion-type:jwt-bearer',
    'client_assertion': token,
    'grant_type': 'client_credentials'
}

response = requests.post(url, headers=headers, data=body)
response_json = response.json()

if 'access_token' in response_json:
    access_token = response_json['access_token']
    print('Access Token:', access_token)
else:
    print('Error obtaining access token:', response_json)
