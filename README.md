Overview:

The custom connector is designed to interact with the GitLab API for project management purposes. Specifically, the connector will be used to create issues in various GitLab projects.

Authentication:

Type: The connector will use Personal Access Token (PAT) authentication.
Details: GitLab requires a PRIVATE-TOKEN header to authenticate API requests. This is handled in the custom connector through API key-based authentication.

Create Issue: The primary action allows users to create an issue in a GitLab project.
Dynamic URL: The endpoint for creating an issue is /api/v4/projects/{projectId}/issues, where projectId is passed dynamically.
Parameters: Users can pass parameters such as title (required), description (optional), labels, assignee_ids, etc., either through the query string or body.
