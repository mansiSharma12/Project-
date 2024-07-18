import requests

# Variables
tenant_id = 'your-tenant-id'
client_id = 'your-client-id'
client_secret = 'your-client-secret'  # If using a client secret

# Get the access token
url = f'https://login.microsoftonline.com/{tenant_id}/oauth2/v2.0/token'
headers = {
    'Content-Type': 'application/x-www-form-urlencoded'
}
body = {
    'client_id': client_id,
    'scope': 'https://graph.microsoft.com/.default',
    'client_secret': client_secret,
    'grant_type': 'client_credentials'
}

response = requests.post(url, headers=headers, data=body)
response_json = response.json()
access_token = response_json['access_token']

print("Access Token:", access_token)
