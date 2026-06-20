# Findings

## Finding 1: Protected Route Access After Logout

**Severity:** Medium  
**Category:** Authentication / Session Management  
**Status:** To be Tested

### Description

The application should prevent users from accessing protected dashboard routes after logout.

### Test Steps

1. Log in to SafeCloud.
2. Open the dashboard.
3. Copy the dashboard URL.
4. Log out.
5. Paste the dashboard URL again.
6. Check whether the user is redirected to login.

### Expected Result

The user should be redirected to the login page.

### Security Impact

If protected routes remain accessible after logout, sensitive files and user data may be exposed.

### Recommendation

Ensure server-side session validation is applied on all protected routes.
