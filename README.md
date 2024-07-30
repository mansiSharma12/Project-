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
