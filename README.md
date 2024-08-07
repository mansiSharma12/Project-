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



/*******************************************************************************************************8888888/
# Replace with your LDAP information
$ldapServer = "your_ldap_server"
$baseDN = "DC=yourdomain,DC=com"

# Function to fetch user information from LDAP
function Get-LDAPUserInfo {
    param(
        [Parameter(Mandatory=$true)]
        [string]$companyName,
        [Parameter(Mandatory=$true)]
        [string]$designation,
        [Parameter(Mandatory=$true)]
        [datetime]$startDate
    )

    $filter = "(company=$companyName)(description=$designation)(userAccountControl:1.2.840.113556.1.4.803:=512)(whenCreated<=@{DateTime=$startDate})"
    $searchBase = $baseDN

    try {
        $ldapConnection = New-Object System.DirectoryServices.DirectoryEntry("LDAP://$ldapServer")
        $ldapSearch = New-Object System.DirectoryServices.DirectorySearcher($ldapConnection)
        $ldapSearch.Filter = $filter
        $ldapSearch.SearchScope = "Subtree"
        $ldapSearch.PropertiesToLoad = @("samaccountname", "displayName", "company", "description", "whenCreated")

        $searchResult = $ldapSearch.FindOne()
        if ($searchResult) {
            $user = New-Object PSObject
            $user | Add-Member -NotePropertyName "Company" -NotePropertyValue $searchResult.Properties["company"].Value[0]
            $user | Add-Member -NotePropertyName "Designation" -NotePropertyValue $searchResult.Properties["description"].Value[0]
            $user | Add-Member -NotePropertyName "StartDate" -NotePropertyValue $searchResult.Properties["whenCreated"].Value[0]
            return $user
        } else {
            Write-Warning "No user found matching the criteria."
            return $null
        }
    } catch [System.Exception] {
        Write-Error "An error occurred: $($_.Exception.Message)"
        return $null
    } finally {
        if ($ldapConnection) { $ldapConnection.Dispose() }
        if ($ldapSearch) { $ldapSearch.Dispose() }
    }
}

# Example usage
$user = Get-LDAPUserInfo -companyName "Your Company" -designation "Your Designation" -startDate (Get-Date "2023-01-01")

if ($user) {
    $user | Export-Csv "userInfo.csv" -NoTypeInformation
}

