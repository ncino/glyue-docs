# extra\_credit.json

```json
{
  "info": {
    "_postman_id": "51d39973-5b27-42e7-b059-8f0d084dc7e4",
    "name": "Glyue Tutorial: Build a Basic Integration",
    "schema": "https://schema.getpostman.com/json/collection/v2.1.0/collection.json"
  },
  "item": [
    {
      "name": "hackernews_email_digest",
      "request": {
        "method": "POST",
        "header": [],
        "body": {
          "mode": "raw",
          "raw": "{\r\n    \"email\": \"{{email}}\",\r\n    \"limitQuery\": 100,\r\n    \"limitEmail\": 5\r\n}",
          "options": {
            "raw": {
              "language": "json"
            }
          }
        },
        "url": {
          "raw": "https://{{host}}/integrations/execute/hackernews_email_digest",
          "protocol": "https",
          "host": ["{{host}}"],
          "path": ["integrations", "execute", "hackernews_email_digest"]
        }
      },
      "response": []
    }
  ],
  "auth": {
    "type": "basic",
    "basic": [
      {
        "key": "password",
        "value": "{{password}}",
        "type": "string"
      },
      {
        "key": "username",
        "value": "{{username}}",
        "type": "string"
      }
    ]
  },
  "event": [
    {
      "listen": "prerequest",
      "script": {
        "type": "text/javascript",
        "exec": [""]
      }
    },
    {
      "listen": "test",
      "script": {
        "type": "text/javascript",
        "exec": [""]
      }
    }
  ],
  "variable": [
    {
      "key": "username",
      "value": "put_glyue_username_here",
      "type": "default"
    },
    {
      "key": "password",
      "value": "put_glyue_password_here",
      "type": "default"
    },
    {
      "key": "host",
      "value": "glyue.domain.name.com",
      "type": "default"
    },
    {
      "key": "email",
      "value": "destination_email_address",
      "type": "default"
    }
  ]
}
```
