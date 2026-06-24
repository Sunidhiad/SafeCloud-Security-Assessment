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

The victim account does not contain the attacker-created folder.

---

### Screenshot 2 - Modified Folder Creation Request

![Folder Creation Request](../screenshots/folder-create-request.png)

The attacker modifies the `owner_id` parameter to the victim's UUID.

---

### Screenshot 3 - Successful Server Response

![Folder Creation Response](../screenshots/folder-create-response.png)

The application returns a successful response after processing the modified request.

---

### Screenshot 4 - Folder Appears in Victim Account

![Folder Created Under Victim](../screenshots/folder-after-create.png)

The attacker-controlled folder is visible within the victim's account.

---

## Root Cause

The application performs folder creation operations based on user-supplied identifiers without enforcing ownership validation.

---

## Recommendation

The application should verify ownership server-side and ensure authenticated users can only create resources within their own account context.

---

## Status

Confirmed

