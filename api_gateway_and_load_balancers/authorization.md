# [Master Role-Based Access Control Patterns](https://www.youtube.com/watch?v=Cy-o0LECWWU) - By Piyush Garg

## Table of content

- Authorization
- Types of Authorizations
    - RBAC
    - PBAC
    - ABAC
    - RE-BAC
- Google White paper (zanzibar)
- How Google does implement auhorization on huge scale?
- Implementation of Dynamic authorization in projects.

We don't only have Role Based Access Control (RBAC), we also so many access control policies.


### Difference between Authorization and Authentication:

**Authentication:**
- It is the process of verifying that a user or system entity is who they claim to be.

- It will be done on platform. e.g.. (Google, Facebook etc.)

**Authorization / Authority:** It is the process of determining whether you have the authority to perform specific action.

**Example taken:** Twitter feed just need to have authenetication not authorization. You cannot change the other's feed because you don't have the authority to perform that action but you can delete your own feed because it's your feed and you have the authority to delete it.


**Implementing authentication is stright forward but implementing authorization is complex.**

- You can use authentication with google.
- authentication with OAuth2.0
- Email password authentication.
- Magic link authentication.

Authentication has just CRUD operations just for the user to login, logout, register etc.
to achieve this you can use JWT Tokens, cookies, session storage etc.

---

**The main problem starts with Authorization.**

- How to scale an authorization system.
- How to handle dynamic authorization.
- How to handle multiple roles.
- How to handle permissions. 


### Patterns of Authorizations (types of authorizations)

Example: You build an Personal Blogging website you provide authentication.

- Who can post on the website you created personal? 

The answer will be you yourself because you are the admin.  (Role)
Editors can edit the blog posts. (Permission) he can't delete the post just he can edit it.
User can read the blog post. (No Permission)

- In the database you will be having the table named users where you will store all the users information like Id, name, email address, password, role etc. (the role can be admin, editor, user).


### Simpliest form of Authorization: 

1. It is Role Based Access control (RBAC).
- The simplest way to do authorization is by using Roles.
- We can assign different roles to our users.
- Each role will have some permission associated with them.
- For example:
    - Admin: Can create, update, delete any post.
    - Editor: Can create, update any post.
    - User: Can only read the post.

- If your backend has a route 
    - `POST/create-post (Check if role is admin)` (No other roles can create a new post except Admin if the other role is trying to create a post then we can send `401:Unauthorized` error message.)  
    - `PUT/edit-post (Check if role is editor)`
    - `GET/read-post (Check if role is user)`


### Fine Grained Access Control (FAC):

You are binding a resource with a user.

**example:** you have a blog named blog1 and the user with named user1 is the owner of that blog. 

- Now you will have other user named user2 with the moderator role. He can edit the blog post but he can't delete the post.

- **Example:** You build an Public Blogging website you provide authentication like medium.

- In the medium anyone can post the blog but the user can't delete the post after posting it, he can just view the blog the person who posted the blog has the authority to delete the post not the normal user.

- **Example 2:** Facebook group.

- In platform level everyone is the user.


- You can delete your own posts in instagram, you can delete comments below your own posts but you cannot delete someone else's posts or comments on someone else's posts, This is called fine grained access control.

- Technically Fine Grained Access Control (FAC) is a part of Role Based Access Control (RBAC).

- **In Fine Garained Access Control (FAC):** You will be having a table named resources which contains id, title, description, owner_id etc.

- And other table named users where you will store all the users information like Id, name, email address, password.

- Now you have other table `user_roles` where you will store the relationship between users and roles.

This table `user_roles` shows the mapping of the user with their respective roles in the database.

---

- Here the complexity increased, to perform a single action we have do the query in extra table.

- Still the FAC is easy to implement because everything can be done in Database, our processing will be saved 

---

## Attribute - Based Access Control

### If we go to Entreprise level

- We have roles like manager, sub-manager, Editor, we have complete hierarchy. 

- We can control the particular resource by there attributes. (example: Google Sheets)

- **Database will be:** List_of_attributes this will be changing dynamically, other table named users where you will store all the users information like Id, name, email address, password. Another table named teams another table named Team_user_mapping which maps the userid, teamid and role then we should match the team_user_mapping based on attribute.

- Here there is the problem in scalibility, to over come it we are going for Policy-Based Access Control.

---

## Policy - Based Access Control

- Technically if we want to implement Attribute - Based Access Control we can go through Policy - Based Access Control.

- You have table in that table you have id, title, content and you have table named users in that you have id, name, email and another table named teams in that table we have team_id, team_name. 

- Policy Based Access Control says that you make a policy, this can be stored in the format of JSON, XML or etc.  

- Policy will have resource with resourceid, taget with targetid, effect with actions, you should decide the structure of the policy.

--- 

## Advantages of Policy - Based Access Control

- All your codes comes to a central place and you can give the entire policy logic to a separate microservice.

- Policy Based Access Control basically helps in a decoupled way to implement role based or fine grained access control.

--- 

## Relationship - Based Access Control - ReBAC

- This is the most difficult thing. 

- Based on parent based access and direct access.

- It uses backtracking 

- First check for direct access if true then give direct access if there is no direct access then use backtracking.


This is a difficult task to implement this within a distributed system on Google's Scale.

- To solve these problems google released a white paper zenzibar.

**Google Zenzibar:** It's a consistent, Global Authorization System.

- OpenFGA (Open Fine - Grained Access Control) a project developed by google and denoted to CNCF (Cloud Native Computing Foundation).

- Using OpenFGA we can test Relationship - Based Access Control. OpenFGA just uses Graph Traversal (Backtracking), it is writen in GoLang 