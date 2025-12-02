# 🧪 Automated API Testing Portfolio - User Management CRUD

## Project Overview

This project is an automated API test suite developed using **Postman** to validate the foundational CRUD (Create, Read, Update, Delete) operations for a User Management resource.

It serves as a professional demonstration of functional API testing skills, leveraging environment variables for test chaining and JavaScript assertions for robust response validation.

**Target API:** Reqres (A hosted REST-API for testing purposes).
**Base URL:** `https://reqres.in/api`

## 🛠️ Skills Demonstrated

This collection showcases the following critical API testing competencies:

1. **CRUD Validation:** Comprehensive testing of `POST`, `GET`, `PUT`, and `DELETE` HTTP methods.

2. **Test Chaining:** Using JavaScript in Postman's "Post-response Scripts" (Tests) to extract data from one request's response (`id` from a `POST`) and inject it into subsequent requests (`GET`, `PUT`, `DELETE`).

3. **Environment Variables:** Effective use of a centralized **Environment** to manage dynamic data (`{{newUserId}}`) and configuration (`{{baseUrl}}`).

4. **Automated Assertions:** Writing robust test scripts using the `Chai.js` syntax (`pm.expect()`) to validate status codes, response content, data types, and required fields.

5. **Collection Runner:** Executing the entire suite sequentially to ensure end-to-end workflow integrity.

## 📁 Collection Structure

The collection is organized to simulate a real-world user workflow:

| **#** | **Request Name** | **Method** | **Endpoint** | **Purpose** | **Key Test** | 
 | ----- | ----- | ----- | ----- | ----- | ----- | 
| **1** | Create New User | `POST` | `{{baseUrl}}/users` | Creates a new user record. | Saves the new `id` to `{{newUserId}}` environment variable. | 
| **2** | Get Single User | `GET` | `{{baseUrl}}/users/{{newUserId}}` | Retrieves the newly created user. | Validates the retrieved data matches the creation request. | 
| **3** | Update User | `PUT` | `{{baseUrl}}/users/{{newUserId}}` | Modifies the details of the existing user. | Validates the `job` field is updated correctly. | 
| **4** | Delete User | `DELETE` | `{{baseUrl}}/users/{{newUserId}}` | Deletes the user record. | Validates the successful deletion response code (`204 No Content`). | 
| **5** | List All Users | `GET` | `{{baseUrl}}/users?page=2` | Validates pagination and list retrieval. | Checks for 12 records in the data array. | 

## 🚀 Setup and Execution

### Prerequisites

1. Postman Desktop App (or Web App) installed.

### 1. Import the Collection

1. In Postman, click **Import**.

2. Select the `User Management - Reqres CRUD Tests.json` file provided in this repository.

### 2. Configure the Environment

1. Go to the **Environments** tab in the sidebar.

2. Select the `Reqres-Test-Env`.

3. Ensure the following variable is configured:

| **Variable** | **Value** | **Description** | 
 | ----- | ----- | ----- | 
| `baseUrl` | `https://reqres.in/api` | The root URL for all API calls. | 
| `newUserId` | (Initial: Empty) | Dynamically set by the POST request for test chaining. | 

4. **Crucial:** Select the `Reqres-Test-Env` from the environment dropdown at the top right of Postman.

### 3. Run the Test Suite

1. Navigate to the **Collections** tab.

2. Click on the collection name (`User Management - Reqres CRUD Tests`).

3. Click the **Run** tab (or the **Run** button).

4. Ensure the requests are ordered correctly (1, 2, 3, 4, 5).

5. Click the **Run Collection** button. The entire workflow will execute sequentially.

## ⚠️ Known Issues / Troubleshooting (Real-World QA)

***Note for Reviewers:*** *While developing, I encountered intermittent `403 Forbidden` errors during the POST request. This demonstrates a real-world challenge where server-side or configuration issues can occur.*

**If you encounter a `403 Forbidden` error:**

1. **Check `https`:** Ensure the `baseUrl` environment variable uses `https` (secure protocol).

2. **Verify Body Format:** Confirm the **Body** tab of the POST request is set to `raw` and the type is explicitly set to `JSON`.

Fixing these issues should allow the request to return the expected `201 Created` status, enabling the rest of the test chain to execute successfully.
