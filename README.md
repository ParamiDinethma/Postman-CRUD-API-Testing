# 🧪 Automated API Testing Portfolio - User Management CRUD
 
## Project Overview
 
This project is an automated API test suite developed using **Postman** to validate the foundational CRUD (Create, Read, Update, Delete) operations for a User Management resource.
 
It serves as a professional demonstration of functional API testing skills, leveraging environment variables for test chaining, JavaScript assertions for robust response validation, and adapting to real-world API changes encountered during development.
 
**Target API:** [Reqres](https://reqres.in) — a hosted REST API for testing purposes.
**Base URL:** `https://reqres.in/api`
**Authentication:** Requires an `x-api-key` header (Reqres moved to a freemium model requiring free API keys — see Setup below).
 
## 🛠️ Skills Demonstrated
 
This collection showcases the following critical API testing competencies:
 
1. **CRUD Validation:** Comprehensive testing of `POST`, `GET`, `PUT`, and `DELETE` HTTP methods.
2. **Test Chaining:** Using JavaScript in Postman's Tests scripts to extract data from one request's response (`id` from a `POST`) and inject it into subsequent requests via environment variables.
3. **Environment Variables:** Centralized management of dynamic data (`{{newUserId}}`, `{{testUserId}}`), configuration (`{{baseUrl}}`), and secrets (`{{apiKey}}`) — keeping credentials out of individual requests and out of version control.
4. **Automated Assertions:** Robust test scripts using `Chai.js` syntax (`pm.expect()`) to validate status codes, headers, response schema, data types, and required fields — including defensive checks (e.g. verifying the response is valid JSON before parsing it).
5. **Root-Cause Debugging:** Diagnosing and resolving real API behavior changes and mock-data limitations (documented in Known Issues below) rather than treating test failures as blockers.
6. **Collection Runner:** Executing the suite sequentially to validate end-to-end workflow integrity.
## 📁 Collection Structure
 
| **#** | **Request Name** | **Method** | **Endpoint** | **Purpose** | **Key Test** |
| ----- | ----- | ----- | ----- | ----- | ----- |
| **1** | Create New User | `POST` | `{{baseUrl}}/users` | Creates a new user record. | Validates `201` status, JSON content-type, `id` and `createdAt` fields; saves `id` to `{{newUserId}}` for chaining. |
| **2** | Get Single User | `GET` | `{{baseUrl}}/users/{{testUserId}}` | Retrieves an existing fixture user. | Validates `200` status and full response schema (`id`, `email`, `first_name`, `last_name`, `avatar`). |
| **3** | Update User | `PUT` | `{{baseUrl}}/users/{{newUserId}}` | Modifies the details of a user record. | Validates the `job` field is updated correctly and an `updatedAt` timestamp is returned. |
| **4** | Delete User | `DELETE` | `{{baseUrl}}/users/{{newUserId}}` | Deletes a user record. | Validates `204 No Content` and an empty response body. |
 
## 🚀 Setup and Execution
 
### Prerequisites
 
1. Postman Desktop App (or Web App) installed.
2. A free Reqres API key — sign up at [app.reqres.in](https://app.reqres.in) to get one.
### 1. Import the Collection and Environment
 
1. In Postman, click **Import**.
2. Select `User Management - Reqres CRUD Tests.postman_collection.json` and `Reqres-Test-Env.postman_environment.json` from this repository.
### 2. Configure the Environment
 
1. Go to the **Environments** tab in the sidebar and select `Reqres-Test-Env`.
2. Set the following variables:
| **Variable** | **Value** | **Description** |
| ----- | ----- | ----- |
| `baseUrl` | `https://reqres.in/api` | The root URL for all API calls. |
| `apiKey` | *your own key from app.reqres.in* | Sent as the `x-api-key` header on every request. **Not included in this repo — you must add your own.** |
| `testUserId` | `2` | A real Reqres fixture user ID (1–12), used for the GET test. |
| `newUserId` | *(leave empty)* | Dynamically set by the POST request for chaining into PUT/DELETE. |
 
3. **Select `Reqres-Test-Env`** from the environment dropdown in the top-right of Postman.
### 3. Run the Test Suite
 
1. Open the collection → click **Run**.
2. Confirm requests run in order: Create → Get → Update → Delete.
3. Click **Run Collection**. All requests should pass.

<img width="1315" height="880" alt="Screenshot 2026-07-30 120222" src="https://github.com/user-attachments/assets/bfef6025-13b2-4307-b153-12f41c16d731" />

## ⚠️ Known Issues / Troubleshooting (Real-World QA) 
This section documents real problems encountered while building and maintaining this suite — and how they were diagnosed and resolved, rather than just worked around.
 
**1. `missing_api_key` / `401` errors**
Reqres moved to a freemium model requiring an `x-api-key` header on every request. This wasn't the case when the suite was first built. **Fix:** sign up for a free key at `app.reqres.in` and add it as the `apiKey` environment variable (see Setup above). The key is intentionally excluded from this repository — you must supply your own.
 
**2. `403 Forbidden` on POST**
Caused by either an insecure `http` base URL or an incorrectly configured request body. **Fix:** ensure `baseUrl` uses `https`, and confirm the POST request's Body tab is set to `raw` with type `JSON`.
 
**3. GET returns `404` for a newly created user**
Reqres's legacy `/users` endpoint returns a simulated `id` on POST, but does not actually persist the new record server-side — only fixture users (IDs 1–12) are real and retrievable. Chaining `{{newUserId}}` into the GET request therefore always returns `404`, even though the POST response itself is valid.
**Fix:** GET is tested against a known fixture user (`{{testUserId}}`) instead of the fabricated `{{newUserId}}`, so it validates real data. The POST → `newUserId` chaining logic is kept and still demonstrated for PUT/DELETE.
 
**4. PUT and DELETE "succeed" for IDs that don't actually exist**
Unlike GET, Reqres's PUT and DELETE mocks don't validate that the target ID is real — they return a success response (`200` / `204`) for any ID, including the fabricated one from POST. This is a mock-API limitation worth noting: passing PUT/DELETE tests here demonstrate correct request/response handling, not proof that a real record was modified or removed.
 
## 📌 Notes
 
- This project uses Reqres, a public fake REST API, for demonstration purposes — it is not a custom-built backend. This is disclosed here for transparency.
- API keys are never committed to this repository. If you fork or clone this project, you must supply your own key via the environment file.
