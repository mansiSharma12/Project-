# Install MSAL (Microsoft Authentication Library) - Required for certificate-based authentication
Install-Module MSAL-MSGraph

# Replace placeholders with your details
$clientId = "YOUR_CLIENT_ID"
$certPath = "C:\path\to\your\certificate.pfx"  # Update with the actual path to your certificate file
$password = "YOUR_CERTIFICATE_PASSWORD"  # Update with the password for your certificate file (if any)
$tenantId = "YOUR_TENANT_ID"  # Optional: Tenant ID if different from common

# Load the client certificate
$cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($certPath, $password, [System.Security.Cryptography.X509Certificates.X509KeyCertIncludeOption]::Chain)

# Configure MSAL for certificate authentication
$msalConfig = New-Object MSAL.PublicClientApplicationConfig -ArgumentList $clientId
$msalConfig.Authority = "https://login.microsoftonline.com/$tenantId/v2.0/token"  # Update with tenant ID if needed
$msalConfig.Certificate = $cert

# Create a public client application
$pca = New-Object MSAL.PublicClientApplication -ArgumentList $msalConfig

# Define the scopes (permissions) requested
$scopes = @("https://graph.microsoft.com/User.Read.All")  # Adjust based on your specific needs

try {
  # Attempt to acquire an access token (interactive flow might be required)
  $accounts = $pca.GetAccounts()
  if ($accounts.Count -eq 0) {
    Write-Error "No accounts found. This script requires interactive flow, which is not recommended for production use due to security concerns."
  } else {
    $account = $accounts[0]
    $tokenResult = $pca.AcquireTokenSilent($scopes, $account)  # Try silent flow first
    if (!$tokenResult) {
      Write-Warning "Silent flow failed. Falling back to interactive flow."
      $tokenResult = $pca.AcquireTokenInteractive($scopes, $account)
    }
  }

  # Access token is available in $tokenResult.AccessToken
  $accessToken = $tokenResult.AccessToken

  Write-Host "Access Token:"
  Write-Host $accessToken

  # Use the access token to call Microsoft Graph API (replace with your desired API call)
  # ... (your Graph API request here) ...
} catch {
  Write-Error ("Error: " + $_.Exception.Message)
}




# Load the required assembly
Add-Type -AssemblyName System.Security.Cryptography.X509Certificates


//Python
import msal

# Replace placeholders with your details
client_id = "YOUR_CLIENT_ID"
authority = f"https://login.microsoftonline.com/YOUR_TENANT_ID/v2.0"  # Update with tenant ID if needed
certificate_path = "path/to/your/certificate.pem"  # Update with the actual path to your certificate file
thumbprint = "YOUR_CERTIFICATE_THUMBPRINT"

# Load certificate from PEM file
with open(certificate_path, "rb") as cert_file:
    certificate_bytes = cert_file.read()

# Configure MSAL for certificate authentication
app = msal.ConfidentialClientApplication(
    client_id=client_id, authority=authority, client_credential={"thumbprint": thumbprint, "certificate": certificate_bytes}
)

# Define the scopes (permissions) requested
scopes = ["https://graph.microsoft.com/User.Read.All"]  # Adjust based on your specific needs

try:
    # Attempt to acquire an access token
    result = app.acquire_token_for_client(scopes=scopes)
    access_token = result.get("access_token")

    print("Access Token:")
    print(access_token)

    # Use the access token to call Microsoft Graph API (replace with your desired API call)
    # ... (your Graph API request here) ...
except Exception as e:
    print("Error:", e)

$oooSettings = Invoke-MgGraphRequest -Method Get -Uri "https://graph.microsoft.com/v1.0/users/$userPrincipalName/messages/outOfOffice"

[System.Security.Cryptography.X509Certificates]::GetTypes()


#Script 1

# Define the IP addresses to test
$ipAddresses = @("8.8.8.8", "1.1.1.1", "216.58.217.206")  # Google DNS, Cloudflare DNS, Your Local IP (replace with yours)

# Function to perform the API call and handle errors
function Get-IPLocation {
  param (
    [Parameter(Mandatory=$true)]
    [string] $IPAddress
  )

  try {
    $response = Invoke-RestMethod -Uri ("https://ip-api.com/json/$IPAddress")
    return $response
  } catch {
    Write-Error ("Error retrieving location for IP $IPAddress: $_" )
    return $null
  }
}

# Test each IP address
foreach ($ipAddress in $ipAddresses) {
  $location = Get-IPLocation -IPAddress $ipAddress

  if ($location) {
    Write-Host ("IP Address: $ipAddress")
    Write-Host ("  Country: $($location.country)")
    Write-Host ("  City: $($location.city)")
    Write-Host ("  Region: $($location.regionName)")
    Write-Host ("  ---")
  }
}


#script 2
# Define the number of files
$numFiles = 25

# Function to generate random characters
function Get-RandomString {
  param (
    [int] $length = 10  # Default length
  )

  [char[]] $chars = (65..90) + (97..122) + (48..57)  # Uppercase, lowercase, numbers
  $randomString = New-Object System.Text.StringBuilder
  for ($i = 0; $i -lt $length; $i++) {
    $randomString.Append($chars[Get-Random (0..$chars.Length - 1)])
  }
  return $randomString.ToString()
}

# Create output array for report
$report = @()

# Loop to create files and gather information
for ($i = 1; $i -le $numFiles; $i++) {
  # Generate random file path and name
  $filePath = Join-Path -Path "C:\Temp" -ChildPath ("TestFile_$i.txt")  # Adjust path as needed

  # Generate random content
  $content = Get-RandomString -Length (Get-Random 10..50)

  # Create the file
  New-Item -Path $filePath -ItemType File -Force | Out-Null

  # Add content to the file
  Add-Content -Path $filePath -Value $content

  # Get file properties
  $fileInfo = Get-Item -Path $filePath

  # Create report object for this file
  $fileReport = New-Object PSObject -Property @{
    "File Name"  = $fileInfo.Name
    "Full Path"  = $fileInfo.FullName
    "Length (bytes)" = $fileInfo.Length
    "Creation Time" = $fileInfo.CreationTime
  }

  # Add report object to the report array
  $report += $fileReport
}

# Convert report array to CSV and save it
$report | Export-Csv -Path "TestReport.csv" -NoTypeInformation -Encoding UTF8

# Delete created files
Remove-Item -Path "C:\Temp\TestFile_*.txt" -Force  # Adjust path as needed

Write-Host "Report saved to TestReport.csv"
Write-Host "Files deleted successfully"



I have developed a high-level Power Automate workflow that sets out-of-office status. Initial testing on my email has been successful, but further testing requires app registration with the necessary permissions. Additionally, the workflow needs improvements and feature enhancements. Could we discuss the next steps and the resources needed for app registration and testing?"
