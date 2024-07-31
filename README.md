{
    "client_id": "your_app_id",
    "client_secret": "your_app_secret",
    "grant_type": "client_credentials",
    "scope": "https://graph.microsoft.com/.default"
}
https://login.microsoftonline.com/<your_tenant_id>/oauth2/v2.0/token

Content-Type: application/x-www-form-urlencoded

# Define variables
$tenantId = "your-tenant-id"
$clientId = "your-client-id"
$clientSecret = "your-client-secret"
$scope = "https://graph.microsoft.com/.default"  # Adjust scope as necessary
$tokenEndpoint = "https://login.microsoftonline.com/$tenantId/oauth2/v2.0/token"

# Prepare the body of the request
$body = @{
    grant_type    = "client_credentials"
    client_id     = $clientId
    client_secret = $clientSecret
    scope         = $scope
}

# Make the request and get the token
$response = Invoke-RestMethod -Method Post -Uri $tokenEndpoint -ContentType "application/x-www-form-urlencoded" -Body $body

# Output the access token
$accessToken = $response.access_token
Write-Output $accessToken

/* PYHTON */
import requests

# Define variables
tenant_id = 'your-tenant-id'
client_id = 'your-client-id'
client_secret = 'your-client-secret'
scope = 'https://graph.microsoft.com/.default'  # Adjust scope as necessary
token_endpoint = f'https://login.microsoftonline.com/{tenant_id}/oauth2/v2.0/token'

# Prepare the body of the request
body = {
    'grant_type': 'client_credentials',
    'client_id': client_id,
    'client_secret': client_secret,
    'scope': scope
}

# Make the request and get the token
response = requests.post(token_endpoint, data=body)
response.raise_for_status()  # Raise an exception for HTTP errors

# Output the access token


1. Initializing a Repository
Extract: Learn how to set up a new Git repository to start tracking your project.

2. Making and Viewing Commits
Extract: Discover how to make commits and view commit history to manage changes effectively.

3. Branching and Merging
Extract: Understand how to create branches and merge changes to streamline your workflow.

4. Pushing and Pulling Changes
Extract: Explore how to push your changes to a remote repository and pull updates from collaborators.

/********************************************************************************************************************************/

1. Repository Initialization
Extract: Kickstart your project with Git. Learn the foundational step of creating a new repository to begin version control and streamline your workflow from day one.

2. Commit Creation and History Analysis
Extract: Master the art of making commits. Understand how to document changes effectively and delve into the commit history to track project evolution with precision.

3. Branch Management and Merging Strategies
Extract: Elevate your collaboration skills. Discover how to create and manage branches, and merge changes seamlessly to maintain a clean and efficient project history.

4. Remote Repository Interaction
Extract: Synchronize with your team. Explore the essential commands for pushing your updates to a remote repository and pulling changes from collaborators to stay in sync effortlessly.
