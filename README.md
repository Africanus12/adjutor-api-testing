# Adjutor API Testing

This repository contains Postman collections and scripts for testing various functionalities of the Adjutor API, including Signup, Login, and BVN verification. The purpose of these tests is to validate the API endpoints, ensure correct functionality, and identify potential performance and security issues.

## Table of Contents
- [Setup](#setup)
- [Usage](#usage)
- [Test Cases](#test-cases)
- [Performance Analysis](#performance-analysis)
- [Security Considerations](#security-considerations)
- [Contributing](#contributing)
- [License](#license)

## Setup

### Prerequisites
- [Postman](https://www.postman.com/downloads/)
- A valid API key for the Adjutor API
- Node.js and npm (for automated testing)

### Installation
1. **Clone the Repository**
   ```bash
   git clone https://github.com/yourusername/adjutor-api-testing.git
   cd adjutor-api-testing
   ```

2. **Import Postman Collection**
   - Open Postman.
   - Click on "Import" in the top-left corner.
   - Select the `Adjutor_API_Testing.postman_collection.json` file from the repository.

3. **Set Up Environment Variables**
   - In Postman, go to the "Environments" tab.
   - Create a new environment named `Adjutor API`.
   - Add the following variables:
     - `base_url`: `https://adjutor.lendsqr.com/v2/`
     - `api_key`: `your_api_key_here`
     - `bvn`: `your_bvn_here`
     - `contact`: `your_contact_here`
   - Save the environment.

## Usage

### Running Tests
1. **Manual Testing**
   - Open the `Adjutor API Testing` collection in Postman.
   - Select an endpoint and click "Send" to run the request.
   - Check the response and the test results.

2. **Automated Testing**
   - Ensure you have Node.js and npm installed.
   - Install Newman, the command-line collection runner for Postman:
     ```bash
     npm install -g newman
     ```
   - Run the Postman collection using Newman:
     ```bash
     newman run Adjutor_API_Testing.postman_collection.json -e Adjutor_API_Environment.json
     ```

## Test Cases

### Signup
**Endpoint:** `POST {{base_url}}/signup`

**Headers:**
- `Content-Type: application/json`

**Body:**
```json
{
  "username": "testuser",
  "password": "password123",
  "email": "testuser@example.com"
}
```

**Test Scripts:**
- **Pre-request Script:** None
- **Tests:**
  ```javascript
  pm.test("Status code is 200", function () {
      pm.response.to.have.status(200);
  });

  pm.test("Response contains user details", function () {
      var jsonData = pm.response.json();
      pm.expect(jsonData).to.have.property('username');
  });
  ```

### Login
**Endpoint:** `POST {{base_url}}/login`

**Headers:**
- `Content-Type: application/json`

**Body:**
```json
{
  "username": "testuser",
  "password": "password123"
}
```

**Test Scripts:**
- **Pre-request Script:** None
- **Tests:**
  ```javascript
  pm.test("Status code is 200", function () {
      pm.response.to.have.status(200);
  });

  pm.test("Response contains token", function () {
      var jsonData = pm.response.json();
      pm.expect(jsonData).to.have.property('token');
  });
  ```

### Retrieving API Keys
**Endpoint:** `GET {{base_url}}/keys`

**Headers:**
- `Authorization: Bearer {{api_key}}`

**Test Scripts:**
- **Pre-request Script:** None
- **Tests:**
  ```javascript
  pm.test("Status code is 200", function () {
      pm.response.to.have.status(200);
  });

  pm.test("Response contains API keys", function () {
      var jsonData = pm.response.json();
      pm.expect(jsonData).to.have.property('keys');
  });
  ```

### BVN Name Inquiry
**Endpoint:** `POST {{base_url}}/verification/bvn/{{bvn}}`

**Headers:**
- `Content-Type: application/json`
- `Authorization: Bearer {{api_key}}`

**Body:**
```json
{
  "contact": "{{contact}}"
}
```

**Test Scripts:**
- **Pre-request Script:** None
- **Tests:**
  ```javascript
  pm.test("Status code is 200", function () {
      pm.response.to.have.status(200);
  });

  pm.test("Response contains OTP status", function () {
      var jsonData = pm.response.json();
      pm.expect(jsonData.status).to.eql("otp");
  });
  ```

### BVN Verification with OTP
**Endpoint:** `PUT {{base_url}}/verification/bvn/{{bvn}}`

**Headers:**
- `Content-Type: application/json`
- `Authorization: Bearer {{api_key}}`

**Body:**
```json
{
  "otp": "242366"
}
```

**Test Scripts:**
- **Pre-request Script:** None
- **Tests:**
  ```javascript
  pm.test("Status code is 200", function () {
      pm.response.to.have.status(200);
  });

  pm.test("Response indicates success", function () {
      var jsonData = pm.response.json();
      pm.expect(jsonData.status).to.eql("success");
  });
  ```

## Performance Analysis

### Response Times
- **Signup:** 362 ms
- **Login:** 1117 ms
- **Retrieving API Keys:** 340 ms
- **BVN Name Inquiry:** 1 m 0.55 s
- **BVN Verification with OTP:** 1 m 0.60 s

### Opportunities for Improvement
1. **Optimize Server Performance:** Address server timeouts and optimize endpoints for quicker responses.
2. **Improve Error Handling:** Provide more informative error messages for easier debugging.
3. **Scalability:** Ensure the API can handle higher loads to prevent timeouts.

## Security Considerations

1. **Authentication and Authorization:** Ensure all endpoints require proper authentication and authorization.
2. **Input Validation:** Validate all inputs to prevent injection attacks.
3. **Rate Limiting:** Implement rate limiting to prevent abuse.
4. **Sensitive Data Exposure:** Avoid exposing sensitive data in error messages.

## Contributing

1. Fork the repository.
2. Create a new branch (`git checkout -b feature-branch`).
3. Make your changes.
4. Commit your changes (`git commit -m 'Add some feature'`).
5. Push to the branch (`git push origin feature-branch`).
6. Open a pull request.

## License

This project is licensed under the MIT License.
