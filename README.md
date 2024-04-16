
## Organizations

### GET /api/v1/supers/organizations
- get list organizations

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
      "full_name": "john"
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

## Templates

### GET /api/v1/supers/templates
- get list of templates for user organization

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
      "id": "2", 
      "name": "Report Template 2",
      "created_at": "24-12-2024",
      "organization": {
        "id": "3",
        "name":"Company 1",
        "logo_url": "<URL>"
      }
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

### GET /api/v1/supers/templates/:id


```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Response
```json
{
  "id": "2", 
  "name": "Report Template 2",
  "created_at": "24-12-2024",
  "content":"<div>{{ placeholder-1 }}</div>",
  "organization": {
    "id": "3",
    "name":"Company 1",
    "logo_url": "<URL>"
  },
  "prompts": [
    {
      "id": "2",
      "placeholder": "placeholder-1"
    }
  ],
}
```


### POST /api/v1/supers/templates
- create new template for an organization
```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Payload
```json
{
  "name": "Report template 1",
  "contents": "<MUSTACHE-HTML-TEMPLATE>",
  "organization_id": "4",
  "template_prompts": [
    {
      "placeholder": "Placeholder name",
      "_prompts": "prompts to ML",
      "_structure": "<div>dasdsa</div>",
      "_example": "<div>dasdsa</div>",
    }
  ]
}
```

Response
```json
{
  "id": "2", 
  "name":  "Report template 1",
  "created_at": "24-12-2024",
  "content": "<MUSTACHE-HTML-TEMPLATE>",
  "organization": {
    "id": "3",
    "name":"Company 1",
    "logo_url": "<URL>"
  },
  "prompts": [],
}
```

Error response
```json
{ 
  "errors": {
    "<ATTRIBUTE>":"<ERROR-MESSAGE>",
    "name": "Name is invalid."
  }
}
```

### PUT /api/v1/supers/templates/:id
- update existing template for an organization

```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Payload
```json
{
  "_name": "Report template 1",
  "_contents": "<MUSTACHE-HTML-TEMPLATE>",
  "_organization_id": "4"
}
```

Response
```json
{
  "id": "2", 
  "name":  "Report template 1",
  "created_at": "24-12-2024",
  "content": "<MUSTACHE-HTML-TEMPLATE>",
  "organization": {
    "id": "3",
    "name":"Company 1",
    "logo_url": "<URL>"
  },
  "prompts": [],
}
```

Error response
```json
{ 
  "errors": {
    "<ATTRIBUTE>":"<ERROR-MESSAGE>",
    "name": "Name is invalid."
  }
}
```

### DELETE /api/v1/supers/templates/:id
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

## Template Prompts

### GET /api/v1/supers/templates/:template_id/template_prompts
- get list of prompts for a template

```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Response
```json
{
  "prompts": [
    {
      "id": "2", 
      "placeholder": "Placeholder name"
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

### GET /api/v1/supers/templates/:template_id/template_prompts/:id


```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Response
```json
{
  "id": "2", 
  "placeholder": "Placeholder name",
  "prompts": "prompts to ML",
  "structure": "<div>dasdsa</div>",
  "example": "<div>dasdsa</div>",
}
```


### POST /api/v1/supers/templates/:template_id/template_prompts
- create new template for an organization
```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Payload
```json
{
  "_placeholder": "Placeholder name",
  "_prompts": "prompts to ML",
  "_structure": "<div>dasdsa</div>",
  "_example": "<div>dasdsa</div>",
}
```

Response
```json
{
  "id": "2", 
  "placeholder": "Placeholder name",
  "prompts": "prompts to ML",
  "structure": "<div>dasdsa</div>",
  "example": "<div>dasdsa</div>",
}
```

Error response
```json
{ 
  "errors": {
    "<ATTRIBUTE>":"<ERROR-MESSAGE>",
    "name": "Name is invalid."
  }
}
```

### PUT /api/v1/supers/templates/:template_id/template_prompts/:id
- update existing template for an organization

```json
{
  "Authorization": "Bearer <TOKEN>"
}
```

Payload
```json
{
  "_placeholder": "Placeholder name",
  "_prompts": "prompts to ML",
  "_structure": "<div>dasdsa</div>",
  "_example": "<div>dasdsa</div>",
}
```

Response
```json
{
  "id": "2", 
  "placeholder": "Placeholder name",
  "prompts": "prompts to ML",
  "structure": "<div>dasdsa</div>",
  "example": "<div>dasdsa</div>",
}
```

Error response
```json
{ 
  "errors": {
    "<ATTRIBUTE>":"<ERROR-MESSAGE>",
    "name": "Name is invalid."
  }
}
```

### DELETE /api/v1/supers/templates/:template_id/template_prompts/:id
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
    "placholder": "Placeholder cannot be nil."
  }
}
```
