# SC-001 - Weak Password Policy

## Severity

Low

## Category

Authentication Security

## Description

The application does not enforce strong password requirements during user registration. Weak passwords such as `123456` are accepted without requiring additional complexity controls.

## Affected Functionality

User Registration

## Proof of Concept

1. Navigate to the registration page.
2. Enter a valid email address.
3. Use the password:

```text
123456
```

4. Submit the registration form.
5. Observe successful account creation.

## Impact

Weak passwords significantly increase the risk of:

* Password Guessing Attacks
* Credential Stuffing
* Brute Force Attacks
* Unauthorized Account Access

## Evidence

### Screenshot 1

Weak password accepted during registration.

### Screenshot 2

Account successfully created using the weak password.

## Recommendation

Implement password complexity requirements such as:

* Minimum length of 8-12 characters
* Uppercase characters
* Lowercase characters
* Numbers
* Special characters

## Status

Confirmed

