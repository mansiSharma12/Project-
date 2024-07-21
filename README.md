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
