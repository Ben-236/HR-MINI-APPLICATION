<p align="center"><a href="https://laravel.com" target="_blank"><img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="400" alt="Laravel Logo"></a></p>

<p align="center">
<a href="https://github.com/laravel/framework/actions"><img src="https://github.com/laravel/framework/workflows/tests/badge.svg" alt="Build Status"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/dt/laravel/framework" alt="Total Downloads"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/v/laravel/framework" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/l/laravel/framework" alt="License"></a>
</p>

HR Mini Application API (v1)

This API provides basic authentication and user management functionalities for an HR mini-application. It includes endpoints for registering, logging in, managing users, and calculating total salaries.

All protected routes require authentication via Laravel Sanctum.

Base URL
/api/v1

Public Routes
1. POST /register

Description:
Registers a new user in the system.

Controller: AuthenticationController@register
Access: Public

Request Body Example:

{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "password123",
  "position": "HR Manager",
  "salary": 50000,
  "department": "Human Resources"
}


Response Example:

{
  "message": "User registered successfully",
  "user": {
    "id": 1,
    "name": "John Doe",
    "email": "john@example.com"
  },
  "token": "your-auth-token"
}

2. POST /login

Description:
Authenticates a user and returns an access token.

Controller: AuthenticationController@login
Access: Public

Request Body Example:

{
  "email": "john@example.com",
  "password": "password123"
}


Response Example:

{
  "message": "Login successful",
  "token": "your-auth-token"
}

Protected Routes

All routes below require a valid Sanctum token in the Authorization header:

Authorization: Bearer <token>

3. POST /logout

Description:
Logs out the authenticated user by revoking their token.

Controller: AuthenticationController@logout
Access: Authenticated users

Response Example:

{
  "message": "Logged out successfully"
}

4. GET /users

Description:
Retrieves a list of all users.

Controller: UserController@index
Access: Authenticated users

Response Example:

[
  {
    "id": 1,
    "name": "John Doe",
    "email": "john@example.com",
    "position": "HR Manager",
    "salary": 50000,
    "department": "Human Resources"
  },
  ...
]

5. POST /users

Description:
Creates a new user.

Controller: UserController@create
Access: Admin only (admin middleware)

Request Body Example:

{
  "name": "Jane Smith",
  "email": "jane@example.com",
  "password": "securePass123",
  "position": "Developer",
  "salary": 40000,
  "department": "Engineering"
}


Response Example:

{
  "message": "User created successfully",
  "user": {
    "id": 2,
    "name": "Jane Smith",
    "email": "jane@example.com"
  }
}

6. GET /users/{user}

Description:
Retrieves details of a specific user by ID.

Controller: UserController@show
Access: Authenticated users

Response Example:

{
  "id": 2,
  "name": "Jane Smith",
  "email": "jane@example.com",
  "position": "Developer",
  "salary": 40000,
  "department": "Engineering"
}

7. PUT /users/{user}

Description:
Updates a user's information.

Controller: UserController@update
Access: Admin only (admin middleware)

Request Body Example:

{
  "position": "Senior Developer",
  "salary": 45000
}


Response Example:

{
  "message": "User updated successfully"
}

8. DELETE /users/{user}

Description:
Deletes a user from the system.

Controller: UserController@destroy
Access: Admin only (admin middleware)

Response Example:

{
  "message": "User deleted successfully"
}

9. GET /total-salary

Description:
Returns the total salary of all employees.

Controller: UserController@totalSalary
Access: Admin only (admin middleware)

Response Example:

{
  "total_salary": 250000
}

Authentication

All protected routes require an Authorization header with a valid token obtained from the /login or /register endpoint.

Example:

Authorization: Bearer your-auth-token

Middleware Overview

auth:sanctum: Ensures only authenticated users can access the route.

admin: Restricts access to admin users only.