# User management



Feature Branch name: `user-mangement_feature`  
API Contract: [here](#)

UMS is the initial Anuvaad module that facilitates user login and other account-related functionalities. It features admin level login and user level login. Only super Admin has the authority to create new organizations or add new users to the system (if not for sign-up). Admin can assign roles to the new users as well.

## Modules

### User Modules

#### CreateUsers

Whitelisted bulk API to create/register users in the system.

**Mandatory params:** `userName`, `email`, `password`, `roles`

**Actions:**

- Validating input params as per the policies
- Storing user entry in the database and assigning a unique id (`userID`)
- Triggering verification email

<details>
<summary>CreateUsers CURL Request</summary>

```bash
curl --location --request POST 'https://auth.anuvaad.org/anuvaad/user-mgmt/v1/users/create' \
--header 'Content-Type: application/json' \
--data-raw '{ 
    "users": [ 
        { 
            "name": "Jainy Joy", 
            "userName": "sample.user@anuvaad.com", 
            "password": "password123", 
            "email": "sample.user@anuvaad.com", 
            "orgID" : "ANUVAAD", 
            "roles": [ 
                { 
                    "roleCode":"TRANSLATOR", 
                    "roleDesc":"Has access to translation services" 
                } 
            ], 
            "models": [ 
                { 
                    "src_lang": "en", 
                    "tgt_lang": "ml", 
                    "uuid": "7156838-90b6-465f-a9aa-5e8f7bfa97e8" 
                }, 
                { 
                    "src_lang": "en", 
                    "tgt_lang": "hi", 
                    "uuid": "2e2fb17a-c470-4562-9cf6-aef0a0ba70ec" 
                } 
            ] 
        } 
    ]    
}'
```
</details>

#### VerifyUsers

Whitelisted API to verify and complete the registration process on Anuvaad.

**Mandatory params:** `userName`, `userID`

**Actions:**

- Validating input params as per the policies
- Activating the user
- Triggering registration successful email

<details>
<summary>VerifyUsers CURL Request</summary>

```bash
curl --location --request POST 'https://auth.anuvaad.org/anuvaad/user-mgmt/v1/users/verify-user' \
--header 'Content-Type: application/json' \
--data-raw '{ 
    "userName": "sample.user@anuvaad.com", 
    "userID": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx" 
}'
```
</details>

#### UserLogin

Whitelisted API for login.

**Mandatory params:** `userName`, `password`

**Actions:**

- Validating input params as per the policies
- Issuing auth token (JWT token)
- Activating user session

<details>
<summary>UserLogin CURL Request</summary>

```bash
curl --location --request POST 'https://auth.anuvaad.org/anuvaad/user-mgmt/v1/users/login' \
--header 'Content-Type: application/json' \
--data-raw '{ 
    "userName": "sample.user@anuvaad.com", 
    "password": "password123" 
}'
```
</details>

#### UserLogout

Whitelisted API for logging out.

**Mandatory params:** `userName`

**Actions:**

- Validating input params as per the policies
- Turning off user session

<details>
<summary>UserLogout CURL Request</summary>

```bash
curl --location --request POST 'https://auth.anuvaad.org/anuvaad/user-mgmt/v1/users/logout' \
--header 'auth-token;' \
--header 'Content-Type: application/json' \
--data-raw '{ 
    "userName": "sample.user@anuvaad.com" 
}'
```
</details>

#### AuthTokenSearch

API to validate auth tokens and fetch back user details.

**Mandatory params:** `token`

**Actions:**

- Validating the token
- Returning user records matching the token only when the token is active
- Same API is used for verifying a token generated on forgot-password as well.

<details>
<summary>AuthTokenSearch CURL Request</summary>

```bash
curl --location --request POST 'https://auth.anuvaad.org/anuvaad/user-mgmt/v1/users/auth-token-search' \
--header 'Content-Type: application/json' \
--data-raw '{ 
    "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VySUQiOiIxMTIyMzM0NCIsImV4cCI6MTYyNzU1MTk5NX0.Wqha17Jsf-D_6KXOsEj3STpV4FBfM_27DRghYKXp7Sg" 
}'
```
</details>

#### UpdateUsers

Bulk API to update user details, RBAC enabled.

**Mandatory params:** `userID`

**Updatable fields:** `orgID`, `roles`, `models`, `email`

**Actions:**

- Validating input params as per the policies
- Updating DB records

<details>
<summary>UpdateUsers CURL Request</summary>

```bash
curl --location --request POST 'https://auth.anuvaad.org/anuvaad/user-mgmt/v1/users/update' \
--header 'auth-token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyTmFtZSI6Imt1bWFyLmRlZXBha0B0YXJlbnRvLmNvbSIsInBhc3N3b3JkIjoiYickMmIkMTIka2V1VFNUU2dTZW5vUzI1Y2djTmJxLmpaVWF1cVN6SXpaL0xGWHdySDRrenJTZE1WMkZPQnUnIiwiZXhwIjoxNjE3OTU4MTMxfQ.TEIg306dXvtiTvuCYdPWF1ZNjv9fQ1Y0iZyBXHoaqzM' \
--header 'Content-Type: application/json' \
--data-raw '{ 
    "users": [ 
        { 
            "userID": "530761e5be1e4e4ebf1335b985c0b1181617878383934", 
            "orgID": "NONMT", 
            "roleCode": "TRANSLATOR", 
            "models": [ 
                { 
                    "src_lang": "en", 
                    "tgt_lang": "ml", 
                    "uuid": "7156838-90b6-465f-a9aa-5e8f7bfa97e8" 
                } 
            ] 
        } 
    ] 
}'
```
</details>

#### ForgotPassword

API for forgot password.

**Mandatory params:** `userName`

**Actions:**

- Validating input params as per the policies
- Generating reset password link and sending it via email

<details>
<summary>ForgotPassword CURL Request</summary>

```bash
curl --location --request POST 'https://auth.anuvaad.org/anuvaad/user-mgmt/v1/users/forgot-password' \
--header 'Content-Type: application/json' \
--data-raw '{ 
    "userName": "sample.user@anuvaad.com" 
}'
```
</details>

#### ResetPassword

API to update password, RBAC enabled.

**Mandatory params:** `userName`, `password`

**Actions:**

- Validating input params as per the policies
- Generating reset password link and sending it via email

<details>
<summary>ResetPassword CURL Request</summary>

```bash
curl --location --request POST 'https://auth.anuvaad.org/anuvaad/user-mgmt/v1/users/reset-password' \
--header 'x-user-id: 7505827e810344b98db9433b8bab4f3d1606377202908' \
--header 'Content-Type: application/json' \
--data-raw '{ 
    "userName": "sample.user@anuvaad.com", 
    "password": "xxxxxxx" 
}'
```
</details>

### Admin Modules

(Only Admin has access)

#### OnboardUsers

Bulk API to onboard users to the Anuvaad system.

**Mandatory params:** `userName`, `email`, `password`, `roles`

**Actions:**

- Validating input params as per the policies
- Storing user entry in the database and assigning a unique `userID`
- User account is verified and activated by default

<details>
<summary>OnboardUsers CURL Request</summary>

```bash
curl --location --request POST 'https://auth.anuvaad.org/anuvaad/user-mgmt/v1/users/onboard-users' \
--header 'auth-token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyTmFtZSI6Imt1bWFyLmRlZXBha0B0YXJlbnRvLmNvbSIsInBhc3N3b3JkIjoiYickMmIkMTIkeFFISlZiUGhkVTFaL2RnNzAzbkUxdWtwZy5YY2wwV1A3R3U3S29JWEI2aHd2aHZILjVqN0snIiwiZXhwIjoxNjEyMzMzNDk0fQ.kVZRyyqaDnHOT9Qgqpet1sIzHjVbJwDALTgOpVxA6yo' \
--header 'Content-Type: application/json' \
--data-raw '{ 
    "users": [ 
        { 
            "name": "Test User", 
            "userName": "testest04@gmail.com", 
            "password": "password1123", 
            "email": "test@mail.com", 
            "phoneNo": "", 
            "roles": [ 
                { 
                    "roleCode": "TRANSLATOR", 
                    "roleDesc": "Has access to translation related resources" 
                } 
            ], 
            "orgID": "TESTORG03" 
        } 
    ] 
}'
```
</details>

#### SearchUsers

API for bulk search with pagination property.

**Actions:**

- Validating input params as per the policies
- All user records are returned if `skip_pagination` is set to True
- When no offset and limit are provided, default values are set as per configs
- Only the records matching the search values are returned if `skip_pagination` is False

<details>
<summary>SearchUsers CURL Request</summary>

```bash
curl --location --request POST 'https://auth.anuvaad.org/anuvaad/user-mgmt/v1/users/search' \
--header 'auth-token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyTmFtZSI6Imt1bWFyLmRlZXBha0B0YXJlbnRvLmNvbSIsInBhc3N3b3JkIjoiYickMmIkMTIkNUpsTWhYOUt0REVOQmxzYlZqYS5OdUYuLmxvWkV4VWw0b2ZDNng3S0dNaHhGMkVraHQvWjInIiwiZXhwIjoxNjExMjk0NzU4fQ.mXlh6tL4ahc1xL16QGv8qDHBWamEYsJmE5b5_lDiioE' \
--header 'Content-Type: application/json' \
--data-raw '{ 
    "userIDs": [], 
    "userNames": [], 
    "roleCodes": [ 
        "TRANSLATOR", 
        "ANNOTATOR" 
    ], 
    "offset": null, 
    "limit": null, 
    "skip_pagination": false 
}'
```
</details>

#### ActivateDeactivateUser

API to update the activation status of a user.

**Mandatory params:** `userName`, `is_active`

**Actions:**

- Validating input params as per the policies
- Updating the user activation status

<details>
<summary>ActivateDeactivateUser CURL Request</summary>

```bash
curl --location --request POST 'https://auth.anuvaad.org/anuvaad/user-mgmt/v1/users/activate-user' \
--header 'auth-token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyTmFtZSI6Imt1bWFyLmRlZXBha0B0YXJlbnRvLmNvbSIsInBhc3N3b3JkIjoiYickMmIkMTIkeFFISlZiUGhkVTFaL2RnNzAzbkUxdWtwZy5YY2wwV1A3R3U3S29JWEI2aHd2aHZILjVqN0snIiwiZXhwIjoxNjEyMzMzNDk0fQ.kVZRyyqaDnHOT9Qgqpet1sIzHjVbJwDALTgOpVxA6yo' \
--header 'Content-Type: application/json' \
--data-raw '{ 
    "userName": "testest03@gmail.com", 
    "is_active": true 
}'
```
</details>

#### SearchRoles

API to fetch active roles in Anuvaad.

**Actions:**

- Returning active role codes

<details>
<summary>SearchRoles CURL Request</summary>

```bash
curl --location --request GET 'https://auth.anuvaad.org/anuvaad/user-mgmt/v1/users/get-roles' \
--header 'auth-token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyTmFtZSI6Imt1bWFyLmRlZXBha0B0YXJlbnRvLmNvbSIsInBhc3N3b3JkIjoiYickMmIkMTIka2V1VFNUU2dTZW5vUzI1Y2djTmJxLmpaVWF1cVN6SXpaL0xGWHdySDRrenJTZE1WMkZPQnUnIiwiZXhwIjoxNjE3OTU4MTMxfQ.TEIg306dXvtiTvuCYdPWF1ZNjv9fQ1Y0iZyBXHoaqzM' \
--data-raw ''
```
</details>


### Organization Modules (Currently only ADMIN has access)

**CreateOrganization**: Bulk API to upsert organizations.

**Mandatory params**: `code`, `active`

**Actions**:
- Validating input params as per the policies
- Creating or deactivating orgs as per `active` status on request

<details>
<summary>CreateOrganization CURL Request</summary>

```bash
curl --location --request POST 'https://auth.anuvaad.org/anuvaad/user-mgmt/v1/org/upsert' \
--header 'auth-token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyTmFtZSI6Imt1bWFyLmRlZXBha0B0YXJlbnRvLmNvbSIsInBhc3N3b3JkIjoiYickMmIkMTIkN1dkc1MzUW9Ob1dxY1NUSzUxREsxZWFIUFhWUW9oRWl2LnFtSTFXM2pJZVZoejVCdnVwRjYnIiwiZXhwIjoxNjExMTU0MjIxfQ.5aDzGWOemHW7dgdwezJhnAWiRXS6ljOSWEuPwW6pQUQ' \
--header 'Content-Type: application/json' \
--data-raw '{ 
    "organizations": [ 
        { 
            "code": "ANUVAAD", 
            "active": true, 
            "description": "default org for the users of Anuvaad system" 
        } 
    ] 
}'
```
</details>

**SearchOrganization**: API to get organization details.

**Actions**:
- If `org_code` is given, searches for that organization alone; otherwise, all organizations are returned.

<details>
<summary>SearchOrganization CURL Request</summary>

```bash
curl --location --request GET 'https://auth.anuvaad.org/anuvaad/user-mgmt/v1/org/search?org_code=ANUVAAD' \
--header 'auth-token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyTmFtZSI6Imt1bWFyLmRlZXBha0B0YXJlbnRvLmNvbSIsInBhc3N3b3JkIjoiYickMmIkMTIkcmI3TlZ3SEk1RVZYcFpmU05KSms2Lng0dEw4b01RMW9oZldsR01SNUFqdkFWa3BSRWNzckcnIiwiZXhwIjoxNjIwNDUxNjcxfQ.dpCOSd0pvxcKsyGqt3HzxtjWZDdNlLG_0zjhSsKfNbA' \
--data-raw ''
```
</details>

### Extension (for Anuvaad web extension)

**GenerateIdToken**: Generating token for web extension user.

**Mandatory params**: `id_token`

**Actions**:
- Decrypting and validating the token
- If the token is valid, register the user and return auth token

<details>
<summary>GenerateIdToken CURL Request</summary>

```bash
curl --location --request POST 'https://auth.anuvaad.org/anuvaad/user-mgmt/v1/extension/users/get/token' \
--header 'Content-Type: application/json' \
--data-raw '{ 
    "id_token": "eE7S2Tn/s8+xhU/EGJKxSC+SvR9IOrGcnbC7Jq5iCLuFrpxNOe8c/aGg5Le1:eV0n09cXpNXXVCfPSkdPmCi4gC68b1oH" 
}'
```
</details>

## Notes

- Add APIs with Zuul if they need external access.
- Rebuild and deploy UMS whenever a new role is added with Zuul.
- Email ID used for system notifications: `anuvaad.support@tarento.com`
- Email templates are available [here](#).

## Setup Tips

- Run the docker container.
- Initialize the DB by creating a Super-Admin account directly in the DB.
- Additional users can be added from the UI by logging into the super admin account.

### How to Initialize UMS without UI?

1. Create an account (Admin is preferred) using the API `anuvaad/user-mgmt/v1/users/create`.
2. Get the verification token from the email (2nd last ID on the ‘verify now’ link) or the `userID` from the user table.
3. Complete the registration process by calling the `anuvaad/user-mgmt/v1/users/verify-user` API.
