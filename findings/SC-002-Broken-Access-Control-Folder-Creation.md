# SC-002 - Broken Access Control in Folder Creation

## Severity

High

## CVSS Rating

8.1 (High)

## Category

OWASP Top 10 2021 - A01: Broken Access Control

---

## Description

The application fails to validate ownership during folder creation operations.

An authenticated attacker can manipulate the `owner_id` parameter in the folder creation request and create folders on behalf of another user.

The application trusts user-controlled identifiers without verifying whether the authenticated user actually owns the target account.

---

## Affected Endpoint

```http
POST /rest/v1/folders
```

---

## Affected Parameters

```json
owner_id
```

---

## Steps to Reproduce

### Step 1

Login as User B (Attacker).

### Step 2

Navigate to the folder creation functionality.

### Step 3

Intercept the request using Burp Suite.

### Step 4

Modify the request payload:

```json
{
  "name": "Hacked Folder",
  "owner_id": "USER_A_UUID",
  "parent_folder_id": null
}
```

### Step 5

Forward the modified request.

### Step 6

Observe the server response:

```http
HTTP/2 201 Created
```

### Step 7

Login as User A (Victim).

### Step 8

Verify that the folder has been created inside User A's account despite being created by User B.

---

## Impact

Successful exploitation allows an attacker to:

* Create folders on behalf of other users
* Modify another user's workspace structure
* Pollute victim accounts with attacker-controlled content
* Compromise data integrity
* Abuse application functionality

---
## Evidence

### Screenshot 1 - Victim Account Before Attack

![Victim Before Attack](../screenshots/folder-before.png)

**Explanation:**

This screenshot shows User A's account before exploitation. The attacker-created folder does not exist within the victim's account. This establishes the baseline state before the attack is performed.

---

### Screenshot 2 - Modified Folder Creation Request

![Folder Creation Request](../screenshots/folder-create-request-userB.png)

**Explanation:**

User B (attacker) intercepts the folder creation request using Burp Suite. The attacker modifies the `owner_id` parameter and replaces it with the UUID belonging to User A (victim). This demonstrates that ownership information can be controlled by the client and is not securely enforced by the application.

---

### Screenshot 3 - Successful Server Response

![Folder Creation Response](../screenshots/folder-create-response.png)

**Explanation:**

The application processes the manipulated request and returns a successful response (`HTTP/2 201 Created`). The request is accepted despite the authenticated user not owning the supplied `owner_id`, indicating missing authorization validation.

---

### Screenshot 4 - Folder Appears in Victim Account

![Folder Created Under Victim](../screenshots/folder-created-under-victim.png)

**Explanation:**

After the manipulated request is processed, the newly created folder becomes visible in User A's account. This confirms that User B was able to create resources on behalf of another user without authorization.

---

### Attack Flow Summary

```text
User B (Attacker)
        │
        ▼
Intercept Folder Creation Request
        │
        ▼
Modify owner_id → User A UUID
        │
        ▼
Server Accepts Request (201 Created)
        │
        ▼
Folder Created Under User A Account
```

**Result:**

Unauthorized folder creation was successfully achieved by manipulating the `owner_id` parameter, confirming a Broken Access Control vulnerability.


## Root Cause

The application performs folder creation operations based on user-supplied identifiers without enforcing ownership validation.

---

## Recommendation

The application should verify ownership server-side and ensure authenticated users can only create resources within their own account context.

---

## Status

Confirmed

