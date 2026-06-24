# SC-002 - Broken Access Control in Folder Management

## Severity

High

## Category

OWASP A01:2021 - Broken Access Control

## Description

The application fails to validate ownership during folder management operations.

An authenticated attacker can manipulate identifiers such as:

* owner_id
* folder_id

to perform actions on folders belonging to other users.

## Affected Operations

### Folder Creation

An attacker can create folders under another user's account by modifying the owner_id parameter.

### Folder Modification

An attacker can modify folders belonging to another user.

### Folder Deletion

An attacker can delete folders belonging to another user by modifying folder identifiers.

## Impact

Successful exploitation may allow:

* Unauthorized folder creation
* Unauthorized folder modification
* Unauthorized folder deletion
* Integrity compromise of user data

## Evidence

### Screenshot 1 - Original Folder Structure

![Before Attack](../screenshots/folder-before.png)

### Screenshot 2 - Modified Folder Creation Request

![Folder Create Request](../screenshots/folder-create-request.png)

### Screenshot 3 - Successful Creation Response

![Folder Create Response](../screenshots/folder-create-response.png)

### Screenshot 4 - Folder Created Under Victim Account

![Folder Created](../screenshots/folder-after-create.png)

### Screenshot 5 - Modified Folder Delete Request

![Delete Request](../screenshots/folder-delete-request.png)

### Screenshot 6 - Folder Deleted

![Delete Success](../screenshots/folder-delete-success.png)

## Status

Confirmed
