The custom connector will prompt for the PAT value when the user configures the connector for the first time.
Users will need to generate their own PAT in GitLab, as discussed, and then securely input it during the connector setup.

# Define the file path
$filePath = "C:\temp\processes.txt"

# Get all running processes and save them to the file
Get-Process | Out-File -FilePath $filePath

# Output confirmation
Write-Host "Processes saved to $filePath."

# Delete the file after saving
Remove-Item -Path $filePath -Force

# Output confirmation
Write-Host "$filePath has been deleted."
