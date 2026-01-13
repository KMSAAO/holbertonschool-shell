# HBNB - API Testing & Validation Report

## Introduction
This report documents the testing process for the HBNB API. It details the validation logic implemented in the service layer, the results of automated unit tests, and manual testing examples using cURL to ensure all endpoints function as expected.

---

## 1. Validation Logic

The following validation rules have been implemented in the Service layer to ensure data integrity before persistence:

### **User Validation**
* **Email:** Must be a valid email format and unique.
* **First/Last Name:** Required fields, must not exceed 50 characters.
* **Password:** Must be at least 6 characters long.

### **Place Validation**
* **Price:** Must be a positive `float` value (greater than 0).
* **Latitude:** Must be between -90.0 and 90.0.
* **Longitude:** Must be between -180.0 and 180.0.
* **Owner:** The `owner_id` must correspond to an existing user.

### **Review Validation**
* **Rating:** Must be an integer between 1 and 5.
* **Place:** Reviews cannot be created for non-existent places.
* **User:** Reviews cannot be created by non-existent users.

### **Amenity Validation**
* **Name:** Required and must not exceed 50 characters.
* **Status:** Must match the allowed Enum values (`active`, `inactive`, etc.).

---

## 2. Automated Unit Tests

We used the `unittest` library to cover all endpoints (GET, POST, PUT, DELETE), ensuring both success scenarios and edge cases (validation errors) are handled correctly.

### **Execution Command:**
Tests were executed from the project root directory using:

```bash
python3 -m unittest discover tests
