public async Task<string> GetChatCompletionAsync(string userInput)
{
    var csvData = LoadCsvAsJson(@"C:\\USB\\Dev\\Test\\Teams_CQD_Backend\\Teams_CQD_Backend\\Data\\TeamsCQDSampleData1.csv");

    try
    {
        _logger.LogInformation("Sending request to Azure OpenAI with input: {UserInput}", userInput);

        var payload = new
        {
            messages = new[]
            {
                new { role = "system", content = "You are a data analysis assistant specializing in call quality data." },
                new { role = "user", content = userInput },
                new { role = "assistant", content = $"Dataset:\n{csvData}" }
            }
        };

        var jsonPayload = JsonSerializer.Serialize(payload);
        var requestContent = new StringContent(jsonPayload, Encoding.UTF8, "application/json");

        var requestUri = "https://sanbox-ai.openai.azure.com/openai/deployments/gpt-4/chat/completions?api-version=2024-10-01";

        var request = new HttpRequestMessage(HttpMethod.Post, requestUri)
        {
            Content = requestContent
        };
        request.Headers.Add("api-key", _apiKey);

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

