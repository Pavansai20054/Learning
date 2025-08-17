# [Master Role-Based Access Control Patterns](https://www.youtube.com/watch?v=Cy-o0LECWWU) - By Piyush Garg

## Table of Contents

- [Introduction to Authorization](#introduction-to-authorization)
- [Authentication vs. Authorization](#authentication-vs-authorization)
- [Types of Access Control Systems](#types-of-access-control-systems)
  - [Role-Based Access Control (RBAC)](#role-based-access-control-rbac)
  - [Fine-Grained Access Control (FAC)](#fine-grained-access-control-fac)
  - [Attribute-Based Access Control (ABAC)](#attribute-based-access-control-abac)
  - [Policy-Based Access Control (PBAC)](#policy-based-access-control-pbac)
  - [Relationship-Based Access Control (ReBAC)](#relationship-based-access-control-rebac)
- [Enterprise-Scale Authorization](#enterprise-scale-authorization)
  - [Google's Zanzibar](#googles-zanzibar)
  - [OpenFGA](#openfga)

## Introduction to Authorization

Authorization is a critical component of modern application security. While many developers are familiar with Role-Based Access Control (RBAC), there are several other access control models that offer different approaches to solving authorization challenges.

## Authentication vs. Authorization

**Authentication** verifies identity:

- Confirms a user is who they claim to be
- Typically handled through platforms like Google, Facebook, or custom systems
- Implemented through methods like:
  - OAuth 2.0
  - Email/password verification
  - Magic links
  - JWT tokens, cookies, or session storage

**Authorization** determines permissions:

- Controls what authenticated users can do within a system
- Defines which actions a user can perform on specific resources
- Example: On Twitter, you can delete your own posts but not others' posts

While authentication is relatively straightforward, authorization presents more complex challenges:

- Scaling authorization systems
- Managing dynamic permissions
- Handling multiple roles
- Implementing granular permission controls

## Types of Access Control Systems

### Role-Based Access Control (RBAC)

The simplest authorization approach:

- Users are assigned specific roles
- Each role has predefined permissions
- Permissions determine what actions users can perform

**Example: Blog Platform**

- Admin: Can create, update, and delete any post
- Editor: Can create and update posts, but not delete them
- User: Can only read posts

**Implementation:**

- Store user information in a database table (id, name, email, password, role)
- Check role before allowing actions:
  - `POST /create-post` → Check if role is admin
  - `PUT /edit-post` → Check if role is editor or admin
  - `GET /read-post` → Allow all authenticated users

### Fine-Grained Access Control (FAC)

FAC extends RBAC by binding specific resources to specific users:

- Associates individual resources with their owners
- Allows more precise control over who can do what with each resource

**Examples:**

1. **Medium-style blog platform:** Anyone can post content, but only the author can delete their own posts
2. **Social media platforms:** Users can delete their own posts and comments on their posts, but not others' content

**Implementation:**

- Database tables:
  - `users`: Stores user information (id, name, email, password)
  - `resources`: Stores content (id, title, description, owner_id)
  - `user_roles`: Maps users to their roles

While this adds complexity (requiring additional database queries), it remains relatively straightforward to implement since all logic can be handled at the database level.

### Attribute-Based Access Control (ABAC)

ABAC is suited for enterprise environments with complex hierarchies:

- Permissions are based on attributes of users, resources, and environment
- Allows for dynamic, context-aware access decisions
- Example: Google Sheets permissions based on document attributes

**Implementation:**

- Database structure includes:
  - `users`: Basic user information
  - `list_of_attributes`: Dynamic attribute definitions
  - `teams`: Team information
  - `team_user_mapping`: Maps users to teams with roles

ABAC offers flexibility but introduces scalability challenges, which leads to Policy-Based Access Control.

### Policy-Based Access Control (PBAC)

PBAC provides a structured approach to implementing ABAC:

- Policies are defined in structured formats (JSON, XML)
- Each policy specifies:
  - Resource identifiers
  - Target users/groups
  - Allowed actions
  - Conditions for access

**Advantages:**

- Centralizes authorization logic
- Can be implemented as a separate microservice
- Provides a decoupled way to implement RBAC or FAC
- Easier to maintain and update access rules

### Relationship-Based Access Control (ReBAC)

ReBAC is the most sophisticated access control model:

- Based on relationships between entities
- Considers both direct access and inherited (parent-based) access
- Uses graph traversal techniques like backtracking

**Implementation challenges:**

- Complex to implement in distributed systems
- Requires efficient graph traversal algorithms
- Difficult to scale to Google-level systems

## Enterprise-Scale Authorization

### Google's Zanzibar

To address large-scale authorization challenges, Google developed Zanzibar:

- A consistent, global authorization system
- Documented in a published white paper
- Powers permissions in Google products like Drive, Photos, and YouTube

### OpenFGA

OpenFGA (Open Fine-Grained Access Control):

- An open-source implementation inspired by Zanzibar
- Developed and donated to the Cloud Native Computing Foundation (CNCF)
- Implements ReBAC using graph traversal techniques
- Written in Go for performance and scalability

---