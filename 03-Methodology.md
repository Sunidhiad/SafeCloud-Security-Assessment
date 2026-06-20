# Methodology

The assessment followed a manual web application security testing approach based on OWASP Top 10.

## Testing Areas

### 1. Authentication Testing
Checked login, signup, password handling, and session behavior.

### 2. Authorization Testing
Checked whether users can access files, folders, or routes that belong to another user.

### 3. File Upload Testing
Checked file extension handling, malicious filenames, file size behavior, and upload restrictions.

### 4. Input Validation Testing
Checked whether user input such as file names, folder names, and search fields can trigger XSS or injection issues.

### 5. Session Management Testing
Checked logout behavior, protected routes, and whether dashboard access is restricted after logout.

## Tools Used

- Burp Suite Community/Professional
- Chrome Developer Tools
- Manual Request Testing
- OWASP Top 10 Checklist
