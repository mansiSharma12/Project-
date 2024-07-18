# Variables
$tenantId = "your-tenant-id"
$clientId = "your-client-id"
$thumbprint = "your-certificate-thumbprint"

# Fetch the certificate
$cert = Get-ChildItem -Path Cert:\LocalMachine\My\$thumbprint

# Prepare the JWT token
$jwtToken = [System.IdentityModel.Tokens.Jwt.JwtSecurityTokenHandler]::WriteToken(
    (New-Object System.IdentityModel.Tokens.Jwt.JwtSecurityToken(
        $null,
        "https://login.microsoftonline.com/$tenantId/v2.0",
        @{},
        [DateTime]::UtcNow,
        [DateTime]::UtcNow.AddMinutes(5),
        (New-Object System.IdentityModel.Tokens.SigningCredentials(
            (New-Object System.IdentityModel.Tokens.X509SecurityKey($cert)),
            "RS256"
        ))
    ))
)

# Get the access token
$response = Invoke-RestMethod -Method Post -Uri "https://login.microsoftonline.com/$tenantId/oauth2/v2.0/token" -ContentType "application/x-www-form-urlencoded" -Body @{
    client_id = $clientId
    scope = "https://graph.microsoft.com/.default"
    client_assertion_type = "urn:ietf:params:oauth:client-assertion-type:jwt-bearer"
    client_assertion = $jwtToken
    grant_type = "client_credentials"
}

# Output the token
$response.access_token
