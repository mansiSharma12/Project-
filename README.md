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
