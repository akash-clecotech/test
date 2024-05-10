## Notes:

- underscore (`_`) means the attribute is optional, but if you want to include the attribute please remove the underscore.

###  POST/api/v1/users/signup
- sign up a new user
- this will create a new user with role admin and its own organization

Payload
```json
{
  "email": "example@mail.com",
  "password": "password",
  "first_name": "james",
  "last_name": "smith",
  "organization_name": "Advisory AI",
}
```

Response
```json
{
  "success": true
}
```

Error Response
```json
{
  "error": "<KEY>",
  "error_description": "<MESSAGE>"
}
```

## Authentication

### POST /oauth/token - sign in
Payload

```json
{
  "grant_type": "password",
  "email": "user@example.com",
  "password": "<PASSWORD>"
}
```

Response
```json
{
  "access_token": "<ACCESS-TOKEN>",
  "token_type": "Bearer",
  "expires_in": 7200,
  "refresh_token": "<REFRESH-TOKEN>",
  "scope": "api",
  "created_at": 1707374345
}
```

Error response
```json
{ 
  "errors": {
    "<ATTRIBUTE>":"<ERROR-MESSAGE>",
    "email": "Email is invalid."
  }
}
```

### POST /oauth/token - renew access token
- until the token is revoked or signout

Payload

```json
{
  "grant_type": "refresh_token",
  "refresh_token": "<REFRESH-TOKEN>",
}
```

Response
```json
{
  "access_token": "<ACCESS-TOKEN>",
  "token_type": "Bearer",
  "expires_in": 7200,
  "refresh_token": "<REFRESH-TOKEN>",
  "scope": "api",
  "created_at": 1707374345
}
```

Error response
```json
{ 
  "errors": {
    "<ATTRIBUTE>":"<ERROR-MESSAGE>",
    "email": "Email is invalid."
  }
}
```

### POST /oauth/revoke - sign out

Header
```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Payload

```json
{
  "token": "<ACCESS-TOKEN>",
}
```

Response
- will always return 200 regardless
```json
{}
```

## Organization
- only admin can change their managed organization

### PUT /api/v1/users/organization
```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Payload
- only sent attributes that need to be updated
```json
{
  "_name": "new organization name",
  "_attachment_id": "<SIGNED_ID_FROM_ATTACHMENT>", // for s3 sned null to remove
}
```

Response
```json
{
  "id": "asd4c",
  "name": "Organization name",
  "logo_url": "<LOGO_URL>", // or null
}
```

Error response
```json
{ 
  "errors": {
    "<ATTRIBUTE>":"<ERROR-MESSAGE>",
    "email": "Email is invalid."
  }
}
```

## Members
- only admin can access members

### GET /api/v1/users/organization/members
- get list of members for current organization
```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Response
```json

{
  "members": [
    {
      "id": "asd4c",
      "first_name": "john",
      "last_name": "smith",
      "role": "advisor",
      "member_id": 2
    }
  ],
  "meta": {
    "current_page":"2",
    "prev_page":"1", // or null
    "next_page":"3", // or null
    "total_pages":"4",
    "total_count":"500"
  }
}

```

### GET /api/v1/users/organization/members/:id
```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Response
```json
{
  "id": "asd4c",
  "email": "user@example.com",
  "first_name": "john",
  "last_name": "smith",
  "role": "advisor",
  "member_id": 2
}
```

### POST /api/v1/users/organization/members
- create new organization member
```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Payload
```json
{
  "email": "user@example.com",
  "role": "advisor", // one of [advisor, paraplanner]
  "first_name": "john",
  "_last_name": "smith",
  "_password": "password" // temporary password (default will be assigned random password until user accept invitation)
}
```

Response
```json
{
  "id": "asd4c",
  "email": "user@example.com",
  "first_name": "",
  "last_name": "",
  "role": "advisor",
  "member_id": 2

}
```

Error response
```json
{ 
  "errors": {
    "<ATTRIBUTE>":"<ERROR-MESSAGE>",
    "email": "Email is invalid."
  }
}
```

### PUT /api/v1/users/organization/members/:id
- update existing organization member

```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Payload
```json
{
  "_email": "members@example.com",
  "_role": "advisor", // one of [advisor, paraplanner]
  "_first_name": "john",
  "_last_name": "smith",
}
```

Response
```json
{
  "id": "asd4c",
  "email": "user@example.com",
  "first_name": "john",
  "last_name": "smith",
  "role": "advisor",
  "member_id": 2

}
```

Error response
```json
{ 
  "errors": {
    "<ATTRIBUTE>":"<ERROR-MESSAGE>",
    "email": "Email is invalid."
  }
}
```

### DELETE /api/v1/users/organization/members/:id
```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Response
```json
{
  "success": true
}
```

Error response
```json
{ 
  "errors": {
    "<ATTRIBUTE>":"<ERROR-MESSAGE>",
    "email": "Email is invalid."
  }
}
```

# Account
- Get and update current user account

### GET /api/v1/users/account
```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Response
```json
{
  "id": "asd4c", 
  "email": "email@example.com",
  "first_name": "John",
  "last_name": "Doe",
  "role": "", 
  "member_id": "1",
  "organization": {
    "id": "cd3s", 
    "name": "organization 1", 
    "logo_url": "<URL>",
  }
}
```

Error response
```json
{ 
  "errors": {
    "<ATTRIBUTE>":"<ERROR-MESSAGE>",
    "email": "Email is invalid."
  }
}
```

### PUT /api/v1/users/account
```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Payload
- only sent attributes that need to be updated
```json
{
  "_email": "email@example.com",
  "_first_name": "John",
  "_last_name": "Doe",
  // must sent all password below to change password
  "_current_password",
  "_password",
  "_password_confirmation"
}
```

Response
```json
{
  "id": "asd4c", 
  "email": "email@example.com",
  "first_name": "John",
  "last_name": "Doe",
  "role": "", 
  "member_id": "1",
  "organization": {
    "id": "cd3s", 
    "name": "organization 1", 
    "logo_url": "<URL>",
  }
}
```

Error response
```json
{ 
  "errors": {
    "<ATTRIBUTE>":"<ERROR-MESSAGE>",
    "email": "Email is invalid."
  }
}
```

## Clients

### GET /api/v1/users/clients
- get list of clients for current_user
```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Response
```json

{
  "clients": [
    {
      "id": "asd4c",
      "full_name": "john",
      "email": "john@example.com", // or null
    }
  ],
  "meta": {
    "current_page":"2",
    "prev_page":"1", // or null
    "next_page":"3", // or null
    "total_pages":"4",
    "total_count":"500"
  }
}

```

### GET /api/v1/users/clients/:id
```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Response
```json
{
  "id": "asd4c",
  "full_name": "john",
  "email": "john@example.com",
  "notes": "some notes",
  "documents": [
    {
      "id": "csd22", 
      "document_no": 2,
      "name": "document.docx",
      "upload_date": "16-02-2024"
    }
  ]
}
```

### POST /api/v1/users/clients
- create new organization member
```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Payload
```json
{
  "full_name": "Carl Johnson",
  "_email": "cjohn@example.com",
  "_notes": "a very long notes",
  "_documents": [
    { attachment_id: 'SIGNED-ID' }
  ],
  "_audios": [
    { attachment_id: 'SIGNED-ID' }
  ]
}
```

Response
```json
{
  "id": "asd4c",
  "full_name": "Carl Johnson",
  "notes": "a very long notes",
  "documents": [ 
     DocumentPayload
  ]
}
```

Error response
```json
{ 
  "errors": {
    "<ATTRIBUTE>":"<ERROR-MESSAGE>",
    "full_name": "Full name is invalid"
  }
}
```

### PUT /api/v1/users/clients
- create new organization member
```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Payload
```json
{
  "_full_name": "Carl Johnson",
  "_email": "cj@gmail.com",
  "_notes": "a very long notes",
}
```

Response
```json
{
  "id": "asd4c",
  "full_name": "Carl Johnson",
  "notes": "a very long notes",
  "documents": [
    {
      "id": "csd22", 
      "document_no": 2,
      "name": "document.docx",
      "upload_date": "16-02-2024"
    }
  ]
}
```

Error response
```json
{ 
  "errors": {
    "<ATTRIBUTE>":"<ERROR-MESSAGE>",
    "full_name": "Full name is invalid"
  }
}
```

### DELETE /api/v1/users/clients/:id
```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Response
```json
{
  "success": true
}
```

Error response
```json
{ 
  "errors": {
    "<ATTRIBUTE>":"<ERROR-MESSAGE>",
    "client": "Record not found."
  }
}
```

## Documents

### GET /api/v1/users/clients/:client_id/documents
- get list of clients documents for specified client

```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Response
```json

{
  "documents": [
    {
      "id": "csd22", 
      "document_no": 2,
      "name": "document.docx",
      "upload_date": "16-02-2024"
    }
  ],
  "meta": {
    "current_page":"2",
    "prev_page":"1", // or null
    "next_page":"3", // or null
    "total_pages":"4",
    "total_count":"500"
  }
}

```

### GET /api/v1/users/clients/:client_id/documents/:id
```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Response
```json
{
  "id": "csd22", 
  "document_no": 2,
  "name": "document.docx",
  "upload_date": "16-02-2024",
  "file_url": "<URL>"
}
```

### POST /api/v1/users/clients/:client_id/documents
- create/add new document for specified client

```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Payload
```json
{
  "attachment_id": "<SIGNED_ID_FROM_ATTACHMENT>" // for s3
}
```

Response
```json
{
  "id": "csd22", 
  "document_no": 2,
  "name": "document.docx",
  "upload_date": "16-02-2024",
  "file_url": "<URL>" // or null if not finish upload yet
}
```

Error response
```json
{ 
  "errors": {
    "<ATTRIBUTE>":"<ERROR-MESSAGE>",
    "full_name": "Full name is invalid"
  }
}
```

### DELETE /api/v1/users/clients/:client_id/documents/:id
- delete/remove specifed document

```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Response
```json
{
  "success": true
}
```

Error response
```json
{ 
  "errors": {
    "<ATTRIBUTE>":"<ERROR-MESSAGE>",
    "client": "Record not found."
  }
}
```

## Attachments

Steps:

1. send POST /api/v1/users/attachments
    ```js
    import CryptoJS from 'crypto-js'

    // https://gist.github.com/elliott-king/77cf0809c6abae892eb7c911692d87f4
    async function md5FromFile(file) {
      // FileReader is event driven, does not return promise
      // Wrap with promise api so we can call w/ async await
      // https://stackoverflow.com/questions/34495796
      return new Promise((resolve, reject) => {
        const reader = new FileReader()
      
        reader.onload = (fileEvent) => {
          let binary = CryptoJS.lib.WordArray.create(fileEvent.target.result)
          const md5 = CryptoJS.MD5(binary)
          resolve(md5)
        }
        reader.onerror = () => {
          reject('oops, something went wrong with the file reader.')
        }
        // For some reason, readAsBinaryString(file) does not work correctly,
        // so we will handle it as a word array
        reader.readAsArrayBuffer(file)
      })
    }

    async function fileChecksum(file) {
      const md5 = await md5FromFile(file)
      const checksum = md5.toString(CryptoJS.enc.Base64)
      return checksum
    }

    const fileInput = document.querySelector('#file0input');
    const file = fileInput.files.[0];

    const contentType = file.type
    const checksum = fileChecksum(file)

    fetch('/api/v1/users/attachments', {
      method: "POST",
      body: {
        filename: file.name, // include extension
        byte_size: file.size, // file size in bytes
        checksum: checksum // file MD5 hash in base64
        content_type: contentType,
        upload_type: 'logo' // one of [logo, document]
      }.to_json,
      headers: {
        'Bearer': 'Authorization <TOKEN>'
      }
    })
    ```
2. Upload to s3 using the returned url and header
    ```js

    // signedURL is the returned url
    const uploaded = await fetch(signedURL, {
      method: "PUT",
      body: file, 
      headers, // given headers
    });
    ```
3. send back signed_id to appropriate update path like organization logo or document file upload
  ```js
  fetch('/api/v1/users/organization', {
      method: "PUT",
      body: {
        attachment_id: "<SIGNED_ID>",
      }.to_json,
      headers: {
        'Bearer': 'Authoirization <TOKEN>'
      }
    })
  ```


### POST /api/v1/users/attachments
- generate s3 presigned upload url

```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Payload
```json
{
  "filename": "myfile.txt", // include extension
  "byte_size": "1024", // file size in bytes
  "checksum": "2dsdasdasd===", // file MD5 hash in base64
  "content_type": "image/jpg",
  "upload_type": "logo", // one of [logo, document]
  "_client_id": "xECsWW" // required for client document
}
```

Response
```json
{
  "url": "https://bucket.s3.region.amazonaws.com/etc", // temporary presigned url
  "signed_id": "dasdasvedasdaddasd",
  "headers": {
    "Content-Type": "image/jpeg",
    "Content-MD5": "3Tbhfs6EB0ukAPTziowN0A=="
  },
}
```

## Templates

### GET /api/v1/users/templates
- get list of templates for user organization

Headers
```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Response
```json

{
  "documents": [
    {
      "id": "csd22", 
      "name": "Report Template 2",
    }
  ],
  "meta": {
    "current_page":"2",
    "prev_page":"1", // or null
    "next_page":"3", // or null
    "total_pages":"4",
    "total_count":"500"
  }
}
```

### GET /api/v1/users/templates/:id
```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Response
```json
{
  "id": "csd22", 
  "name": "Report Template 2",
  "contents": "<div>{{ placeholder }}</div>",
}
```

## Confirmations

### GET /api/v1/users/confirmations?=confirmation_token=<TOKEN>
- for admin to confirm their user email
- after confirmation, they should be redirected to sign in page

Response
```json
{
  "success": true
}
```

### POST /api/v1/users/confirmations
- send a new confirmation email for registered email

Payload
```json
{
  "email": "user@example.com"
}
```

Response
```json
{
  "success": true
}
```

## Invitations

### PUT /api/v1/users/invitations
- for user to accept invite and set password

Payload
```json
{
  "invitation_token": "TOKEN",
  "password": "password",
  "password_confirmation": "password",
}
```

Response
```json
{
  "success": true
}
```

## Integrations
- use the `redirect_url` fomr index to authorize user with their outlook/integration account
- this url will then redirect back with a authroization code eg: `https://app.advisoryai.com/outlook/callback?code=<AUTH_CODE>`
- send back this code

### GET /api/v1/users/integrations
- get available integrations and status


Headers
```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Response
```json
{
  "integrations": [
    {
      "id": "xsd2f" ,
      "name": "Outlook",
      "description" : "some description",
      "active" : false,
      "platform" : "outlook",
      "redirect_url": "https://login.microsoftonline.com/common/oauth2/v2.0/authorize?dasdsadad" // or null for active integration
    }
  ]
}
```

### POST /api/v1/users/integrations
- activate user integration using the microsft authorization code

Headers
```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Payload
```json
{
  "platform": "outlook",
  "code": "M.C106_BAY.2.d4e282b0-7eef-7543-4e10-56156c27dbd2"
}
```

Response
```json
{
  "id": "xsd2f" ,
  "name": "Outlook",
  "description" : "some description",
  "active" : true,
  "platform" : "outlook",
  "redirect_url": null
}
```

### POST /api/v1/users/integrations/:id/sync
- manual data sync with integrated platform (for now for outlooks only)

Headers
```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Payload
```json
{}
```

Response
```json
{
  "success": true,
  "message": "Your %{integration} data has been resync."
}
```

## Meetings

### GET /api/v1/users/meetings
- get current_user meetings

Headers
```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Response
```json
{
  "meetings": [
    {
      "id": "xsd2f" ,
      "subject": "meeting subject",
      "meeting_url": "http://team.com/url",
      "from": "20-2-2024 7:00AM",
      "to":  "20-2-2024 8:00AM",
      "status": "upcoming",
    }
  ],
  "meta": {
    "current_page":"2",
    "prev_page":"1", // or null
    "next_page":"3", // or null
    "total_pages":"4",
    "total_count":"500"
  }
}
```

### GET /api/v1/users/meetings/:id
- get meeting details and transcription

```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Response
```json
{
  "id": "xsd2f" ,
  "subject": "meeting subject",
  "meeting_url": "http://team.com/url",
  "from": "20-2-2024 7:00AM",
  "to":  "20-2-2024 8:00AM",
  "status": "upcoming",
  "transcription":"transctriont tect long",
  "client_id": "xAZS" // or null
}
```

## Client Reports


### GET /api/v1/users/clients/:client_id/reports
- get list of reports for specified client

```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Response
```json

{
  "reports": [
    {
      "id": "csd22", 
      "title": "Client 1 Example report ",
      "generated_at": "16-02-2024",
      "report_type": "meeting_note",
    }
  ],
  "meta": {
    "current_page":"2",
    "prev_page":"1", // or null
    "next_page":"3", // or null
    "total_pages":"4",
    "total_count":"500"
  }
}

```

### GET /api/v1/users/clients/:client_id/reports/:id
```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Response
```json
{
  "id": "csd22", 
  "title": "Client 1 Example report ",
  "generated_at": "16-02-2024",
  "report_type": "meeting_note",
  "generated_contents": "<HTML>", // or null if not finish upload yet
  "raw_content": { "placeholder": "generated content from claude" }, // or null if no content from claude
  "template_contents": "<b>{{ placeholder }}</b>",
  "docx_url": "<URL>", // null if not generated yet
  "pdf_url": "<URL>", // null if not generated yet
  "advisor": {
    "first_name": "Clark",
    "last_name": "Kent"
  },
  "client": {
    "full_name": "David Heimmer",
  }
}
```

### POST /api/v1/users/clients/:client_id/reports
- create/add new document for specified client

```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Payload
```json
{
  "report_type": "", // one of ['meeting_note', 'suitability_letter']
  "template_id": "sXWDSS"
}
```

Response
```json
{
  "id": "csd22", 
  "title": "Client 1 Example report ",
  "generated_at": "16-02-2024",
  "report_type": "meeting_note",
  "generated_contents": "<HTML>", // or null if not finish upload yet
  "template_contents": "<b>{{ placeholder }}</b>",
  "raw_content": { "placeholder": "generated content from claude" }, // or null if no content from claude
  "docx_url": "<URL>", // null if not generated yet
  "pdf_url": "<URL>", // null if not generated yet
  "advisor": {
    "first_name": "Clark",
    "last_name": "Kent"
  },
  "client": {
    "full_name": "David Heimmer",
  }
}
```

### PUT /api/v1/users/clients/:client_id/reports/:id
- update report with generated contents or update contents

```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Payload
```json
{
  "generated_contents": "<HTML>"
}
```

Response
```json
{
  "id": "csd22", 
  "title": "Client 1 Example report ",
  "generated_at": "16-02-2024",
  "report_type": "meeting_note",
  "generated_contents": "<HTML>", // or null if not finish upload yet
  "template_contents": "<b>{{ placeholder }}</b>",
  "raw_content": { "placeholder": "generated content from claude" }, // or null if no content from claude
  "docx_url": "<URL>", // null if not generated yet
  "pdf_url": "<URL>", // null if not generated yet
  "advisor": {
    "first_name": "Clark",
    "last_name": "Kent"
  },
  "client": {
    "full_name": "David Heimmer",
  }
}
```


Error response
```json
{ 
  "errors": {
    "<ATTRIBUTE>":"<ERROR-MESSAGE>",
    "full_name": "Full name is invalid"
  }
}
```

### DELETE /api/v1/users/clients/:client_id/reports/:id
- delete/remove specifed report

```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Response
```json
{
  "success": true
}
```

Error response
```json
{ 
  "errors": {
    "<ATTRIBUTE>":"<ERROR-MESSAGE>",
    "client": "Record not found."
  }
}
```


## Reports


### GET /api/v1/users/reports
- get list of reports generated by the advisor

```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Response
```json

{
  "reports": [
    {
      "id": "csd22", 
      "title": "Client 1 Example report ",
      "generated_at": "16-02-2024",
      "report_type": "meeting_note",
    }
  ],
  "meta": {
    "current_page":"2",
    "prev_page":"1", // or null
    "next_page":"3", // or null
    "total_pages":"4",
    "total_count":"500"
  }
}

```

### GET /api/v1/users/reports/:id
- get report generated b id owned by the advisor

```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Response
```json
{
  "id": "csd22", 
  "title": "Client 1 Example report ",
  "generated_at": "16-02-2024",
  "report_type": "meeting_note",
  "generated_contents": "<HTML>", // or null if not finish upload yet
  "template_contents": "<b>{{ placeholder }}</b>",
  "raw_content": { "placeholder": "generated content from claude" }, // or null if no content from claude
  "docx_url": "<URL>", // null if not generated yet
  "pdf_url": "<URL>", // null if not generated yet
  "advisor": {
    "first_name": "Clark",
    "last_name": "Kent"
  },
  "client": {
    "full_name": "David Heimmer",
  }
}
```

### POST /api/v1/users/reports
- create a new report for the advisor

```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Payload
```json
{
  "template_id": "sXWDSS",
  "meeting_id": "xxxWWS",
  "client_id":"xasew"
}
```

Response
```json
{
  "id": "csd22", 
  "title": "Client 1 Example report ",
  "generated_at": "16-02-2024",
  "report_type": "meeting_note",
  "generated_contents": "<HTML>", // or null if not finish upload yet
  "template_contents": "<b>{{ placeholder }}</b>",
  "docx_url": "<URL>", // null if not generated yet
  "pdf_url": "<URL>", // null if not generated yet
  "advisor": {
    "first_name": "Clark",
    "last_name": "Kent"
  },
  "client": {
    "full_name": "David Heimmer",
  }
}
```

### PUT /api/v1/users/reports/:id
- update report with generated contents or update contents

```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Payload
```json
{
  "generated_contents": "<HTML>"
}
```

Response
```json
{
  "id": "csd22", 
  "title": "Client 1 Example report ",
  "generated_at": "16-02-2024",
  "report_type": "meeting_note",
  "generated_contents": "<HTML>", // or null if not finish upload yet
  "template_contents": "<b>{{ placeholder }}</b>",
  "docx_url": "<URL>", // null if not generated yet
  "pdf_url": "<URL>", // null if not generated yet
  "advisor": {
    "first_name": "Clark",
    "last_name": "Kent"
  },
  "client": {
    "full_name": "David Heimmer",
  }
}
```


Error response
```json
{ 
  "errors": {
    "<ATTRIBUTE>":"<ERROR-MESSAGE>",
    "full_name": "Full name is invalid"
  }
}
```

### DELETE /api/v1/users/reports/:id
- delete/remove specifed report owned by the advisor

```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Response
```json
{
  "success": true
}
```

Error response
```json
{ 
  "errors": {
    "<ATTRIBUTE>":"<ERROR-MESSAGE>",
    "client": "Record not found."
  }
}
```

### Report websocket

- after creating a report subscribe to websocket channel

response
```jsonc
// signify streaming start
{
  "identifier": "{\"channel\":\"ReportChannel\",\"id\":\"ZjpvqmNP\" }",
  "message": {
    "event": "content_start",
	}
}


// signify streaming body
{
  "identifier": "{\"channel\":\"ReportChannel\",\"id\":\"ZjpvqmNP\" }",
  "message": {
    "event": "content_body",
		"placeholder": "emergency_fund",
		"value": "Setting"
	}
}

// signify streaming end
{
  "identifier": "{\"channel\":\"ReportChannel\",\"id\":\"ZjpvqmNP\" }",
  "message": {
    "event": "content_stop",
	}
}
```

example
```js
report_id = 'ZjpvqmNP';
token = 'somee-token';


let ws = new WebSocket('wss://advisoryai-be.staging.advisoryai.com/cable',null, null, {
    Authorization: `Bearer ${token}`
});

ws.onopen = function() {
  //Subscribe to the channel
  ws.send(JSON.stringify({"command": "subscribe","identifier":`{"channel":"ReportChannel","id":"${ZjpvqmNP}"}`}));
}

ws.onmessage = function(msg) {
  data = JSON.parse(msg.data)
  if (data.identifier === "{\"channel\":\"ReportChannel\",\"id\":\"ZjpvqmNP\"}") {
    console.log(data_obj.message.placeholder, data_obj.message.value);
  }
}
```

### Admin
### Plans
### GET /api/v1/users/plans
- Get list of plans
```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Response
```json
{
    "plans": [
        {
            "id": 1,
            "name": "Level 1",
            "price": "30.0",
            "setup_cost": null,
            "interval": "month",
            "description": "Base Monthly Plan",
            "plan_type": "standard",
            "organization_id": null,
            "organization_name": null,
            "members_count": null,
            "is_subscribed": false,
            "plan_line_items": [
                {
                    "id": 1,
                    "template_name": "Meeting notes",
                    "quantity": 20,
                    "is_unlimited": false
                },
                {
                    "id": 2,
                    "template_name": "Share Your Thoughts",
                    "quantity": 20,
                    "is_unlimited": false
                }
            ]
        },
        {
            "id": 2,
            "name": "Bespoke",
            "price": "250.0",
            "setup_cost": "3000.0",
            "interval": "month",
            "description": "Bespoke Special plan",
            "plan_type": "bespoke",
            "organization_id": 1,
            "organization_name": "Clecotech",
            "members_count": null,
            "is_subscribed": true,
            "plan_line_items": [
                {
                    "id": 3,
                    "template_name": "Meeting notes",
                    "quantity": null,
                    "is_unlimited": true
                },
                {
                    "id": 4,
                    "template_name": "Sharing point",
                    "quantity": null,
                    "is_unlimited": true
                }
            ]
        }
    ]
}
```

### GET /api/v1/users/plans/:id
- get plan

```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Response
```json
{
    "id": 1,
    "name": "Level 1",
    "price": "30.0",
    "setup_cost": null,
    "interval": "month",
    "description": "Base Monthly Plan",
    "plan_type": "standard",
    "organization_id": null,
    "organization_name": null,
    "members_count": 2,
    "is_subscribed": false,
    "plan_line_items": [
        {
            "id": 1,
            "template_name": "Meeting notes",
            "quantity": 20,
            "is_unlimited": false
        },
        {
            "id": 2,
            "template_name": "Share Your Thoughts",
            "quantity": 20,
            "is_unlimited": false
        }
    ]
}
```

Error response
```json
{
    "errors": {
        "not_found": "Record cannot be found."
    }
}
```

### Subscription
### GET api/v1/users/subscriptions/subscribed_plan
- get subscribed_plan

```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Response
```json
{
    "id": 1,
    "card_token": "card_1PEoHF07TxYtdOaZskUBaeEG",
    "card_brand": "Visa",
    "card_last_digit": "4242",
    "next_cycle_at": "2024-06-11",
    "current_period_start": "2024-05-10",
    "current_period_end": "2024-06-10",
    "plan_name": "Level 1: £60 per head",
    "transaction_id": null,
    "plan": {
        "id": 2,
        "name": "Level 1: £60 per head",
        "price": "60.0",
        "setup_cost": null,
        "interval": "month",
        "description": "",
        "plan_type": "standard",
        "organization_id": null,
        "organization_name": null,
        "members_count": 2,
        "is_subscribed": true
    }
}
```

Error response
```json
{
    "errors": {
        "not_found": "Record cannot be found."
    }
}
```

### POST api/v1/users/subscriptions
- create new subscription
```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Payload
```json
{
    "plan_id": 24,
    "stripe_token": "tok_1P94y***********d2vf8PWwXB",
    "card_brand": "Visa",
    "card_token": "card_1P94y*******D6d2QjFJozAr",
    "card_last_digit": "4242"
}
```

Response
```json
{
    "id": 3,
    "card_token": "card_1P94ylSGDBAfD6d2QjFJozAr",
    "card_brand": "Visa",
    "card_last_digit": "4242",
    "next_cycle_at": "2024-06-11",
    "current_period_start": "2024-05-10",
    "current_period_end": "2024-06-10",
    "plan_name": "Level 3: £150 per head",
    "transaction_id": "ch_3PEos807TxYtdOaZ05j3579Z",
    "plan": {
        "id": 4,
        "name": "Level 3: £150 per head",
        "price": "150.0",
        "setup_cost": null,
        "interval": "month",
        "description": "Level 3: £150 per head",
        "plan_type": "standard",
        "organization_id": null,
        "organization_name": null,
        "members_count": null,
        "is_subscribed": true
    }
}
```

Error response
```json
{
    "errors": {
        "stripe_token": {
            "key": "stripe_token",
            "symbol": "stripe_error",
            "message": "You cannot use a Stripe token more than once: tok_1PEorw07TxYtdOaZNjoRR4jL.",
            "index": null
        }
    }
}
```

### PATCH /api/v1/users/subscriptions/:id
- Update subscription
```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Payload
```json
{
    "stripe_token": "tok_1P94ylSGDBAfD6d2vf8PWwXB"
}
```

Response
```json
{
    "id": 3,
    "card_token": "card_1PEp0807TxYtdOaZ67xzFNWR",
    "card_brand": "visa",
    "card_last_digit": "4242",
    "next_cycle_at": "2024-06-11",
    "current_period_start": "2024-05-10",
    "current_period_end": "2024-06-10",
    "plan_name": "Level 3: £150 per head",
    "transaction_id": null,
    "plan": {
        "id": 4,
        "name": "Level 3: £150 per head",
        "price": "150.0",
        "setup_cost": null,
        "interval": "month",
        "description": "Level 3: £150 per head",
        "plan_type": "standard",
        "organization_id": null,
        "organization_name": null,
        "members_count": null,
        "is_subscribed": true
    }
}
```

Error response
```json
{
    "errors": {
        "stripe_token": {
            "key": "stripe_token",
            "symbol": "stripe_error",
            "message": "You cannot use a Stripe token more than once: tok_1PEorw07TxYtdOaZNjoRR4jL.",
            "index": null
        }
    }
}
```

### DELETE /api/v1/users/subscriptions/:id/cancel

```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Response
```json
{
  "success": true
}
```

Error response
```json
{
    "errors": {
        "stripe_token": {
            "key": "stripe_subscription",
            "symbol": "stripe_error",
            "message": "Subscription not present.",
            "index": null
        }
    }
}
```