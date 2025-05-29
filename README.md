First item is prb ticket automation, script is working in prod , now I am working on to schedule it in Amelia. Second is automated ms support ticket creation, I am currently trying to find out how can I get the administrator role on virtual worker as rpa bot runs on that and side by side , I am also creating a sample workflow in automation anywhere before actually getting started with the ticket creation automation. Then third, working on the automated server restart of powerapp gateways along with atul, the team is currently working on the alert generation in bigpanda so we are currently working on a script that could check from the list of servers periodically and restart them









As part of SRE automation team,  I have gained experience in power automate by creating a workflow for India DoT compliance reporting where the goal was to check users compliance status with respect to external calls made. For a few months , worked with SRE monitoring team on a project using Login Enterprise tool to monitoring M365 application. I created scripts in .net for multiple applications so got a bit of experience in this language. 
Then talking on the python side - I have had experience in using data handling libraries like numpy and pandas . In my initial months in UBS , i have got to work on python flask where I worked on building a backend essentially handling rest APIs calls to onboard users to an internal enterprise tool. 
Currently I am working on a new requirement for which I have started exploring selenium for web UI automation and testing . 



I worked with SRE monitorinon a project using Login Enterprise to monitor Microsoft 365 applications, where I built automation scripts using .NET. It was a completely new tech stack for me, but I picked it up quickly and now have a solid working understanding of scripting in .NET environments.”























Currently I am working on 3 items.  First is the automated creation of GL issues from share point list . This automation requirement we received from jonathan where he wanted to created GL issues based on PRB ticket information and its status for blamless postmortem . I have created a workflow in amelia for this its almost 80% completed . Then second item is the viceversa of the previous item which is to take information from gitlab issues/epics and populate to SPO. i have created the automation of this and completed the testing in dev. Once we have our app registration in prod,  i will move the automation to prod. This will fetch all issues/epics of our SRE team from gitlab and create a list in our official eus infraops SRE site. 
Then next automation I am working on is to create MS support tickets once any incident or outage has been identified. I have recently started working on this and currently deciding on the platforms that we have for automations.



















{
  "$schema": "https://developer.microsoft.com/json-schemas/sp/v2/column-formatting.schema.json",
  "elmType": "div",
  "children": [
    {
      "elmType": "a",
      "style": {
        "text-align": "center",
        "display": "flex",
        "justify-content": "center",
        "align-items": "center",
        "height": "100%",
        "color": "blue",
        "text-decoration": "underline"
      },
      "attributes": {
        "href": "=if(@currentField != '', @currentField, '')",
        "target": "_blank"
      },
      "txtContent": "=if(@currentField != '', @currentField, '')"
    }
  ]
}
























{
  "$schema": "https://developer.microsoft.com/json-schemas/sp/v2/column-formatting.schema.json",
  "elmType": "a",
  "attributes": {
    "href": "@currentField",
    "target": "_blank"
  },
  "style": {
    "color": "blue",
    "text-decoration": "underline"
  },
  "txtContent": "@currentField"
}

 Import PnP PowerShell module
Credential ($svcUsername, $securePassword)

# Step 4: Connect to SharePoint Online using PnP PowerShell
$siteUrl = "https://yourtenant.sharepoint.com/sites/yoursite"

Connect-PnPOnline -Url $siteUrl -Credentials $cred

if ($?) {
    Write-Host "Connected to SharePoint site: $siteUrl" -ForegroundColor Green

    # Step 5: Fetch SharePoint List Data
    $listName = "YourListName"
    $items = Get-PnPListItem -List $listName
    $items | ForEach-Object { 
        Write-Host "Item: $($_.FieldValues)" 
    }

    # Step 6: Disconnect after completion
    Disconnect-PnPOnline
    Write-Host "Disconnected from SharePoint site." -ForegroundColor Yellow
} else {
    Write-Host "Failed to connect to SharePoint site. Check credentials or permissions." -ForegroundColor Red
}


        









quest.Headers.Add("api-key", _apiKey);

        var response = await _httpClient.SendAsync(request);
        var responseString = await response.Content.ReadAsStringAsync();

        // Check if userInput contains keywords that should trigger an alert
        if (userInput.Contains("alert", StringComparison.OrdinalIgnoreCase) ||
            userInput.Contains("error", StringComparison.OrdinalIgnoreCase) ||
            userInput.Contains("critical", StringComparison.OrdinalIgnoreCase))
        {
            await SendCriblAlert(userInput);
        }

        return responseString;
    }
    catch (Exception ex)
    {
        _logger.LogError(ex, "Error processing OpenAI request");
        return "An error occurred while processing your request.";
    }
}

// Function to Send Alert to Cribl
private async Task SendCriblAlert(string alertMessage)
{
    string criblEndpoint = "https://cribl-endpoint.com/create-alert"; // Replace with actual Cribl URL

    var alertPayload = new
    {
        message = alertMessage,
        severity = "High",
        source = "Chatbot-GPT"
    };

    var jsonPayload = JsonSerializer.Serialize(alertPayload);
    var content = new StringContent(jsonPayload, Encoding.UTF8, "application/json");

    HttpResponseMessage response = await _httpClient.PostAsync(criblEndpoint, content);

    if (!response.IsSuccessStatusCode)
    {
        string errorResponse = await response.Content.ReadAsStringAsync();
        _logger.LogError($"Cribl Alert Failed: {errorResponse}");
    }
    else
    {
        _logger.LogInformation("Cribl alert sent successfully.");
    }
}
















To check **how your application is sending requests** and which **proxy** (if any) it is using, follow these steps:  

---

## **1️⃣ Check System Proxy Settings (Windows)**
First, check if your system has a proxy configured:  

**Run this in PowerShell:**  
```powershell
netsh winhttp show proxy
```
👉 **If you see something like this:**  
```
Proxy Server(s) :  http://your-proxy-server:PORT
```
✅ **Your system is using a proxy.** Your chatbot might need to use the same one.  
❌ **If it says "Direct access (no proxy)"**, then your system isn’t using a proxy.  

---

## **2️⃣ Check If Your .NET Application is Using a Proxy**
Run this **inside your application** to check if `HttpClient` is using a proxy:  

```csharp
var handler = new HttpClientHandler();
Console.WriteLine($"Proxy enabled: {handler.UseProxy}");
Console.WriteLine($"Proxy address: {handler.Proxy?.GetProxy(new Uri("https://www.google.com"))}");
```
### **Expected Output:**
- **If a proxy is used:**  
  ```
  Proxy enabled: True
  Proxy address: http://your-proxy-server:PORT
  ```
- **If no proxy is used:**  
  ```
  Proxy enabled: False
  Proxy address:
  ```

---

## **3️⃣ Capture Network Traffic Using Fiddler or Wireshark**
If you're unsure **how your app is routing requests**, use **Fiddler** or **Wireshark** to inspect the HTTP traffic.

### **Using Fiddler:**
1. Download **[Fiddler Classic](https://www.telerik.com/fiddler)**.  
2. Start **Fiddler** and go to **Options → HTTPS** → Enable "Decrypt HTTPS traffic".  
3. Run your chatbot and check the **Fiddler logs** for the request.  
4. Look for **"Gateway" or "Proxy"** in the request details.

---

## **4️⃣ Force Your Chatbot to Use the System Proxy**
If your system is using a proxy but your chatbot isn’t, make it use the system proxy:  

```csharp
var handler = new HttpClientHandler
{
    Proxy = WebRequest.GetSystemWebProxy(), // Uses the same proxy as PowerShell
    UseProxy = true,
    UseDefaultCredentials = true // Uses logged-in user credentials if required
};

var _httpClient = new HttpClient(handler);
```
✅ **This ensures your chatbot follows the same network path as PowerShell.**  

---

### **🚀 Next Steps**
1️⃣ **Check `netsh winhttp show proxy` to see system proxy settings.**  
2️⃣ **Run `handler.UseProxy` inside your .NET app to check its proxy usage.**  
3️⃣ **Use Fiddler/Wireshark if you need deeper network analysis.**  
4️⃣ **If PowerShell works but your chatbot doesn’t, force it to use the system proxy.**  

Would you like help interpreting Fiddler logs if you capture traffic?

equals(trim(coalesce(split(item(),';')?[12],'')),'IN')



# Authentication details
$tenantId = "<Your-Tenant-ID>"
$clientId = "<Your-Client-ID>"
$clientSecret = "<Your-Client-Secret>"
$siteId = "<Your-SharePoint-Site-ID>"
$listId = "<Your-SharePoint-List-ID>"
$itemId = "<Your-Item-ID>"  # ID of the item you want to update
$graphBaseUrl = "https://graph.microsoft.com/v1.0"

# Get Access Token
$tokenUrl = "https://login.microsoftonline.com/$tenantId/oauth2/v2.0/token"
$body = @{
    client_id     = $clientId
    client_secret = $clientSecret
    scope         = "https://graph.microsoft.com/.default"
    grant_type    = "client_credentials"
}

$tokenResponse = Invoke-RestMethod -Method Post -Uri $tokenUrl -ContentType "application/x-www-form-urlencoded" -Body $body
$accessToken = $tokenResponse.access_token

# Headers for Graph API Request
$headers = @{ 
    Authorization = "Bearer $accessToken"
    "Content-Type" = "application/json"
}

# Data to Update (Modify the fields as needed)
$updateFields = @{
    "Title"        = "Updated Title"
    "Description"  = "Updated description for this item"
    "Status"       = "Completed"
} | ConvertTo-Json -Depth 2

# Graph API Endpoint to Update the Item
$updateUrl = "$graphBaseUrl/sites/$siteId/lists/$listId/items/$itemId/fields"

# Make the PATCH request to update the item
$response = Invoke-RestMethod -Method Patch -Uri $updateUrl -Headers $headers -Body $updateFields

# Output response
Write-Host "Updated item successfully with new values."


