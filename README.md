# Import PnP PowerShell module
Import-Module PnP.PowerShell

# SharePoint site URL and list details
$siteUrl = "https://yourtenant.sharepoint.com/sites/yoursite"  # Replace with your SharePoint site URL
$listName = "GitLabIssues"  # Replace with your SharePoint list name

# Connect to SharePoint Online
Connect-PnPOnline -Url $siteUrl -Interactive  # Use Interactive login or other methods as needed

# Get all items from the SharePoint list
$items = Get-PnPListItem -List $listName

# Loop through each item and fetch the Title and Description fields
foreach ($item in $items) {
    $title = $item["Title"]
    $description = $item["Description"]

    # Output or store the values
    Write-Host "Title: $title"
    Write-Host "Description: $description"
    
    # You can also store them into variables or process them as needed
    # $yourTitleVariable = $title
    # $yourDescriptionVariable = $description
}

# Disconnect after retrieving data
Disconnect-PnPOnline
