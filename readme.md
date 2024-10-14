# Ajax Library

## Overview

This lightweight Ajax library provides a simple and flexible way to interact with RESTful APIs using JavaScript. It encapsulates common HTTP methods (GET, POST, PUT, DELETE) and provides a base class for creating API-specific clients.

## Table of Contents

1. [Features](#features)
2. [How It Works](#how-it-works)
3. [REST URL Structure](#rest-url-structure)
4. [Usage](#usage)
5. [API Reference](#api-reference)

## Features

- Supports common HTTP methods: GET, POST, PUT, DELETE
- Automatic token-based authentication
- Customizable base URL and API URL
- Easy-to-use API for common CRUD operations
- Promise-based for asynchronous operations

## How It Works

The library consists of several components:

1. `AjaxBase`: A base class that provides methods for common CRUD operations.
2. Individual Ajax method files (`ajaxGet.js`, `ajaxPost.js`, `ajaxPut.js`, `ajaxDelete.js`): These files contain the core logic for making HTTP requests using the Fetch API.

The `AjaxBase` class uses these individual Ajax methods to perform operations. It constructs the full URL by combining the `baseUrl`, `apiUrl`, and any additional parameters or IDs.

Authentication is handled automatically by retrieving a token from `localStorage` and including it in the `Authorization` header of each request.

## REST URL Structure

The library is designed to work with RESTful API structures. It constructs URLs in the following format:

```
${apiUrl}/${baseUrl}[/${id}][?${queryString}]
```

- `apiUrl`: The base URL of your API (e.g., "https://api.example.com")
- `baseUrl`: The specific endpoint for a resource (e.g., "users")
- `id`: (Optional) The identifier for a specific resource
- `queryString`: (Optional) Additional query parameters

Examples:
- GET all users: `https://api.example.com/users`
- GET a specific user: `https://api.example.com/users/123`
- GET users with query: `https://api.example.com/users?role=admin`

## Usage

Here's how you can use the Ajax library in your project:

1. First, create an instance of `AjaxBase` for your specific API endpoint:

```javascript
import AjaxBase from './AjaxBase';

const usersApi = new AjaxBase('users', 'https://api.example.com');
```

2. Now you can use the methods provided by `AjaxBase` to interact with your API:

```javascript
// Get all users
const getAllUsers = async () => {
  const response = await usersApi.get();
  const users = await response.json();
  console.log(users);
};

// Get a specific user
const getUser = async (id) => {
  const response = await usersApi.getOne(id);
  const user = await response.json();
  console.log(user);
};

// Create a new user
const createUser = async (userData) => {
  const response = await usersApi.create(userData);
  const newUser = await response.json();
  console.log(newUser);
};

// Update a user
const updateUser = async (id, userData) => {
  const response = await usersApi.update(id, userData);
  const updatedUser = await response.json();
  console.log(updatedUser);
};

// Delete a user
const deleteUser = async (id) => {
  const response = await usersApi.delete(id);
  console.log(`User ${id} deleted`);
};
```

## API Reference

### AjaxBase

#### Constructor

```javascript
new AjaxBase(baseUrl, apiUrl)
```

- `baseUrl`: The base endpoint for the resource (e.g., "users")
- `apiUrl`: The base URL of your API (e.g., "https://api.example.com")

#### Methods

- `get(queryString)`: Fetches all resources or filters based on the query string
- `getOne(id)`: Fetches a single resource by ID
- `create(data)`: Creates a new resource
- `update(id, data)`: Updates an existing resource
- `delete(id)`: Deletes a resource

Each method returns a Promise that resolves to the Fetch API Response object.

### Note on Authentication

This library assumes you're using token-based authentication. Make sure to set the token in localStorage:

```javascript
localStorage.setItem("token", "your-auth-token-here");
```

The token will be automatically included in the `Authorization` header of each request.This token is read from localStorage and not from cookies, if you need that then this needs to be changed.