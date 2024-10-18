# Editor Management System
## Stockholders
•	Admin
•	Editor
•	Client
## Requirements
### Admin:
•	Admin can log in and log out.
•	Admin can change their password.
•	Admin can view orders from both Clients and Editors.
•	Admin can update order statuses.
•	Admin can create and edit users (both Clients and Editors).
### Client:
•	 Clients can register, log in, and log out.
•	Clients can view services provided by all Editors.
•	Clients can create orders for services.
•	Clients can view the status of their orders.
### Editor:
•	Editors can register, log in, and log out.
•	Editors can create services.
•	Editors can only view and manage their own services.
•	Editors can submit work for orders assigned to them.
•	Editors can accept or cancel orders assigned to them.

# Database Schema

## Users Table
This table handles the basic authentication and user roles.
Field	Type	Description
id	INT (PK)	Unique identifier
username	VARCHAR	Unique username
email	VARCHAR	User's email
password	VARCHAR	Hashed password
role	ENUM	Enum: Admin, Client, Editor
created at	TIMESTAMP	Date of account creation
updated at	TIMESTAMP	Last account update
Services Table 
Field	Type	Description
id	INT (PK)	Unique identifier
editor	JSON	Full Editor object (instead of FK)
title	VARCHAR	Service title
description	TEXT	Service description
price	DECIMAL	Service price
service_type	JSON	Full Service Type object
created_at	TIMESTAMP	Date the service was created
updated_at	TIMESTAMP	Last update to the service


Service Type Table
This is a new table that allows for categorizing services into different types.
Field	Type	Description
id	INT (PK)	Unique identifier
name	VARCHAR	Name of the service type (e.g., "Design")
description	TEXT	Description of the service type

Orders Table (Pass Full Objects)
Orders will now store complete Client, Editor, and Service objects.
Field	Type	Description
id	INT (PK)	Unique identifier
client	JSON	Full Client object (instead of FK)
editor	JSON	Full Editor object (instead of FK)
service	JSON	Full Service object (instead of FK)
status	ENUM	Enum: Pending, In Progress, Completed, Cancelled
created_at	TIMESTAMP	Date the order was created
updated_at	TIMESTAMP	Last update to the order

Submissions Table
Editors can submit drafts or final versions of their work related to a project (order).
Field	Type	Description
id	INT (PK)	Unique identifier
order	JSON	Foreign key to Orders table
Editor	JSON	Foreign key to Users (Editor)
submission_type	ENUM	Enum: Draft, Final
content	TEXT	Submitted work or reference to file
submitted_at	TIMESTAMP	Date of submission

API Endpoints
Admin:
•	POST /admin/login: Admin login.
•	POST /admin/logout: Admin logout.
•	PUT /admin/change-password: Change password.
•	GET /admin/orders: View all orders from Clients and Editors.
•	PUT /admin/orders/:id: Update order status.
•	POST /admin/users: Create a new user (Client or Editor).
•	PUT /admin/users/:id: Edit an existing user (Client or Editor).
Client:
•	POST /client/register: Client registration.
•	POST /client/login: Client login.
•	POST /client/logout: Client logout.
•	GET /client/services: View services from all Editors.
•	POST /client/orders: Create an order for a service.
•	GET /client/orders: View all orders created by the client.
•	GET /client/orders/:id: View status of a specific order.
Editor:
•	POST /editor/register: Editor registration.
•	POST /editor/login: Editor login.
•	POST /editor/logout: Editor logout.
•	POST /editor/services: Create a new service.
•	GET /editor/services: View only their own services.
•	POST /editor/orders/:id/submit: Submit work for an assigned order.
•	PUT /editor/orders/:id/accept: Accept an assigned order.
•	PUT /editor/orders/:id/cancel: Cancel an assigned order.

