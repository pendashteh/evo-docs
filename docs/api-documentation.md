# Evo: API Documentation

This document provides detailed documentation of the Evo API, which allows agents to interact with the Evo core application and with each other.

## Authentication

All API requests must be authenticated using an API key. You can generate API keys in the Evo browser extension.

**Header:**
Authorization: Bearer <API_KEY>

## Endpoints

### Agent Management

* **`POST /agents`**
    * Creates a new agent.
    * **Request Body:**
        ```json
        {
          "type": "browser_profile", // or other agent type
          "name": "My Browser Agent",
          "capabilities": ["send_data", "receive_messages"],
          "context": {
            "browser_id": "12345"
          }
        }
        ```
    * **Response:**
        ```json
        {
          "id": "agent_id_123",
          "status": "created"
        }
        ```

* **`GET /agents/{agent_id}`**
    * Retrieves information about an agent.
    * **Response:**
        ```json
        {
          "id": "agent_id_123",
          "type": "browser_profile",
          "name": "My Browser Agent",
          // ... other agent details
        }
        ```

* **`PUT /agents/{agent_id}`**
    * Updates an agent's information.
    * **Request Body:** Similar to `POST /agents`
    * **Response:**
        ```json
        {
          "id": "agent_id_123",
          "status": "updated"
        }
        ```

* **`DELETE /agents/{agent_id}`**
    * Deletes an agent.
    * **Response:**
        ```json
        {
          "id": "agent_id_123",
          "status": "deleted"
        }
        ```

### Agent Communication

* **`POST /agents/{agent_id}/messages`**
    * Sends a message to an agent.
    * **Request Body:**
        ```json
        {
          "recipient_id": "agent_id_456",
          "content": {
            "action": "log_activity",
            "data": {
              "url": "[https://example.com](https://example.com)"
            }
          }
        }
        ```
    * **Response:**
        ```json
        {
          "status": "sent"
        }
        ```

* **`GET /agents/{agent_id}/messages`**
    * Retrieves messages for an agent.
    * **Response:**
        ```json
        [
          {
            "sender_id": "agent_id_789",
            "content": {
              // ... message content
            }
          },
          // ... other messages
        ]
        ```

### Access Control

* **`POST /permissions`**
    * Grants a permission to an agent.
    * **Request Body:**
        ```json
        {
          "agent_id": "agent_id_123",
          "resource": "file_system",
          "action": "read"
        }
        ```
    * **Response:**
        ```json
        {
          "status": "granted"
        }
        ```

* **`GET /permissions/{agent_id}`**
    * Retrieves permissions for an agent.
    * **Response:**
        ```json
        [
          {
            "resource": "file_system",
            "action": "read"
          },
          // ... other permissions
        ]
        ```

* **`DELETE /permissions/{agent_id}/{permission_id}`**
    * Revokes a permission from an agent.
    * **Response:**
        ```json
        {
          "status": "revoked"
        }
        ```

## Data Formats

* **Agent Type:**
    * `browser_profile`
    * `local_machine`
    * `email`
    * `website`
    * `physical_device`
    * `server`
    * `organization`
    * `ai_agent`
    * `hive_agent`

* **Capabilities:**
    * `send_data`
    * `receive_messages`
    * `read_files`
    * `write_files`
    * `execute_scripts`
    * `access_network`
    * ...

* **Context:**
    * `browser_id`
    * `machine_id`
    * `email_address`
    * `website_url`
    * `device_id`
    * `server_address`
    * `organization_id`
    * `ai_model_id`
    * ...

## Error Handling

The API uses standard HTTP status codes to indicate success or failure.

* **200 OK:** The request was successful.
* **400 Bad Request:** The request was invalid or malformed.
* **401 Unauthorized:** The request was not authenticated or the API key was invalid.
* **403 Forbidden:** The agent does not have permission to perform the requested action.
* **500 Internal Server Error:** An unexpected error occurred on the server.

Error responses will include a JSON object with more details about the error.

```json
{
  "error": {
    "code": "invalid_request",
    "message": "The request body was invalid."
  }
}
