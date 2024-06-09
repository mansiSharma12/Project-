Certainly! Here is a detailed documentation for the tool's frontend and backend architecture that you can use on your company's Confluence page:

---

## Mailbox Administration Portal Architecture

### Overview

The Mailbox Administration Portal is a web-based tool designed to manage mailbox access permissions in Exchange Online. The system is composed of a frontend built with ASP.NET and a backend powered by PowerShell scripts, integrated with Exchange Online for mailbox operations. The data storage is handled by MS SQL Server.

### Frontend Architecture

#### Technologies Used
- **Framework**: ASP.NET
- **Languages**: C#, HTML, CSS, JavaScript

#### Functionality
- Provides a user interface for submitting mailbox access permission requests.
- Allows users to track the status of their requests.
- Communicates with the backend to store and retrieve request data.

#### Process Flow
1. **User Login**: Users log into the portal using their credentials.
2. **Request Submission**: Users submit requests for mailbox access permissions through a form.
3. **Data Storage**: Submitted requests are written to the MS SQL Server database.
4. **Status Updates**: Users can view the status of their requests as they progress through approval and execution phases.

### Backend Architecture

#### Technologies Used
- **Scripting**: PowerShell
- **Database**: MS SQL Server
- **Service**: Background process (e.g., Windows Service or scheduled task)

#### Components
1. **Background Process**: 
   - Runs every 30 minutes to process new requests.
   - Reads requests from the MS SQL Server database.
   - Uses PowerShell scripts to perform mailbox operations in Exchange Online.

2. **PowerShell Scripts**:
   - Execute specific commands to manage mailbox permissions.
   - Authenticate to Exchange Online using a service account.
   - Log every command and its result for auditing purposes.

#### Process Flow
1. **Reading Requests**: 
   - The background service queries the MS SQL Server database for new requests.
2. **Authentication**: 
   - PowerShell scripts use a service account to authenticate to Exchange Online.
3. **Executing Commands**:
   - The scripts perform the requested mailbox operations (e.g., granting access permissions).
   - Each command is logged in the database with details for auditing.
4. **Updating Status**:
   - The background service updates the request status in the database based on the outcome of the PowerShell script execution.

### Data Storage

#### Database: MS SQL Server
- **Request Data**: Stores details of each request, including user information, requested permissions, and current status.
- **Logging**: Maintains logs of all PowerShell commands executed, including timestamps, command details, and results.

### Security and Authentication

#### Current Method
- **Service Account Authentication**: 
  - Uses a single-factor authentication (username and password) to connect to Exchange Online.
- **Data Security**: 
  - Securely stores service account credentials.
  - Ensures secure connections between frontend, backend, and Exchange Online.

#### Recommended Enhancement
- **Azure AD Registration**:
  - Implement Azure AD registration for enhanced security.
  - Use multi-factor authentication (MFA) for connecting to Exchange Online.
  - Manage permissions and roles through Azure AD for more granular control.

### Example Workflow

1. **Request Submission**:
   - User submits a request for mailbox access via the ASP.NET web portal.
   - Request is stored in the MS SQL Server database.

2. **Background Process Execution**:
   - Every 30 minutes, the background process retrieves new requests from the database.
   - PowerShell scripts authenticate using the service account and connect to Exchange Online.

3. **Mailbox Operation**:
   - PowerShell scripts execute the necessary commands to grant mailbox access permissions.
   - Each command execution is logged for auditing.

4. **Status Update**:
   - The background process updates the request status in the database.
   - Users can see the updated status in the ASP.NET portal.

### Future Enhancements

- **Azure AD Integration**:
  - Implement Azure AD registration to replace service account authentication.
  - Enable multi-factor authentication for improved security.
  - Use OAuth 2.0 and OpenID Connect for modern and secure authentication.

- **Performance Optimization**:
  - Optimize the background process to handle a higher volume of requests efficiently.
  - Consider scaling the backend services and database as needed to improve performance.

By documenting the architecture in this detailed manner, your team will have a clear understanding of how the tool operates and what improvements can be made for better performance and security.

---

Feel free to customize this documentation to better fit your company's specific requirements and any additional details you might have.
