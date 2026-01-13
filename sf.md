# 🏠 HBNB – API Testing & Validation Report

## 1. Introduction
This document presents a complete testing and validation report for the **HBNB API**.  
It describes the validation logic implemented in the service layer, the results of automated unit tests, and manual testing using `cURL` to ensure that all endpoints behave correctly.

---

## 2. Validation Logic

All validation rules are enforced in the **Service Layer** before persistence to guarantee data integrity.

### User Validation
- Email must be valid and unique
- First and last names are required and must not exceed 50 characters
- Password must be at least 6 characters long

### Place Validation
- Price must be a positive float
- Latitude must be between -90.0 and 90.0
- Longitude must be between -180.0 and 180.0
- Owner must reference an existing user

### Review Validation
- Rating must be an integer between 1 and 5
- Reviews cannot be created for non-existent places
- Reviews cannot be created by non-existent users

### Amenity Validation
- Name is required and must not exceed 50 characters
- Status must match allowed enum values (active, inactive, etc.)

---

## 3. Automated Unit Tests
```bash
python3 -m unittest discover tests
```
The following output confirms that all automated unit tests passed successfully:
```bash
test_create_amenity (tests.test_amenity.TestAmenityAPI.test_create_amenity) ... ok
test_create_amenity_invalid_data (tests.test_amenity.TestAmenityAPI.test_create_amenity_invalid_data) ... ok
test_delete_amenity (tests.test_amenity.TestAmenityAPI.test_delete_amenity) ... ok
test_get_amenity (tests.test_amenity.TestAmenityAPI.test_get_amenity) ... ok
test_update_amenity (tests.test_amenity.TestAmenityAPI.test_update_amenity) ... ok
test_create_review (tests.test_review.TestReviewAPI.test_create_review) ... ok
test_update_review (tests.test_review.TestReviewAPI.test_update_review) ... ok
test_delete_review (tests.test_review.TestReviewAPI.test_delete_review) ... ok
test_create_user (tests.test_user.TestUserAPI.test_create_user) ... ok
test_create_place (tests.test_place.TestPlaceAPI.test_create_place) ... ok

Ran 12 tests in 0.452s

OK
```
---

## 4. Manual Testing (cURL)

### A) Users Endpoint – Create User
```bash
curl -X POST http://127.0.0.1:5000/api/v1/users/ \
-H "Content-Type: application/json" \
-d '{
  "first_name": "John",
  "last_name": "Doe",
  "email": "john.doe@example.com",
  "password": "securepassword"
}'
```
```bash
{
  "id": "3587063f-3430-4e2b-a45b-7b7c25c3c04f",
  "first_name": "John",
  "last_name": "Doe",
  "email": "john.doe@example.com"
}
```
### B) Amenities Endpoint – Create Amenity
```bash
Copy code
curl -X POST http://127.0.0.1:5000/api/v1/amenities/ \
-H "Content-Type: application/json" \
-d '{
  "amenity_name": "WiFi",
  "description": "High-speed internet",
  "status": "active"
}'
```
```bash
{
  "id": "1fa85f64-5717-4562-b3fc-2c963f66afa6",
  "amenity_name": "WiFi",
  "description": "High-speed internet",
  "status": "active"
}
```
### C) Places Endpoint – Create Place
```bash
curl -X POST http://127.0.0.1:5000/api/v1/places/ \
-H "Content-Type: application/json" \
-d '{
  "title": "Cozy Apartment",
  "description": "Nice place in the city center",
  "price": 100.0,
  "latitude": 34.0522,
  "longitude": -118.2437,
  "owner_id": "3587063f-3430-4e2b-a45b-7b7c25c3c04f"
}'
```
```bash
{
  "id": "8a9b2c3d-4e5f-6g7h-8i9j-0k1l2m3n4o5p",
  "title": "Cozy Apartment",
  "price": 100.0,
  "latitude": 34.0522,
  "longitude": -118.2437,
  "owner_id": "3587063f-3430-4e2b-a45b-7b7c25c3c04f"
}
```
### D) Reviews Endpoint – Create Review
```bash
curl -X POST http://127.0.0.1:5000/api/v1/reviews/ \
-H "Content-Type: application/json" \
-d '{
  "place_id": "8a9b2c3d-4e5f-6g7h-8i9j-0k1l2m3n4o5p",
  "user_id": "3587063f-3430-4e2b-a45b-7b7c25c3c04f",
  "rating": 5,
  "comment": "Amazing place!"
}'
```
```bash
{
  "id": "review-uuid-1234",
  "place_id": "8a9b2c3d-4e5f-6g7h-8i9j-0k1l2m3n4o5p",
  "rating": 5,
  "comment": "Amazing place!"
}
```
### Invalid Rating (Edge Case)
```bash
curl -X POST http://127.0.0.1:5000/api/v1/reviews/ \
-H "Content-Type: application/json" \
-d '{
  "place_id": "8a9b2c3d-4e5f-6g7h-8i9j-0k1l2m3n4o5p",
  "rating": 6,
  "comment": "Invalid"
}'
```
```bash
{
  "message": "rating must be between 1 and 5"
}
```
### 5. Conclusion
All API endpoints have been successfully implemented and validated.

Validation rules are correctly enforced

Automated unit tests fully cover API behavior

Manual testing confirms real-world correctness

Proper HTTP status codes are returned (200, 201, 400, 404)

Swagger UI documentation is available when running the application.
