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


try {
    $ldapConnection = New-Object System.DirectoryServices.DirectoryEntry("LDAP://your_ldap_server")
    Write-Host "Connection successful"
} catch {
    Write-Host "Connection failed: $($_.Exception.Message)"
}


# Import Active Directory module
Import-Module ActiveDirectory

# Define the LDAP Load Balancer server
$ldapServer = "ldap://your-ldap-load-balancer-server"

# Define the properties to fetch
$properties = @("givenName", "sn", "mail", "physicalDeliveryOfficeName")

# Search the Active Directory
$searchResult = Get-ADUser -Filter * -SearchBase "DC=yourdomain,DC=com" -SearchScope Subtree -Property $properties -Server $ldapServer

# Create an array to hold the user information
$userInfoArray = @()

foreach ($user in $searchResult) {
    # Create a PSObject for each user
    $userInfo = New-Object PSObject -Property @{
        FirstName = $user.givenName
        LastName  = $user.sn
        Email     = $user.mail
        Location  = $user.physicalDeliveryOfficeName
    }

    # Add the PSObject to the array
    $userInfoArray += $userInfo
}

# Output the array to a CSV file
$userInfoArray | Export-Csv -Path "UserInformation.csv" -NoTypeInformation -Encoding UTF8

Write-Output "User information has been exported to UserInformation.csv"
