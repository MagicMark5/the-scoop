# The Scoop

## Project Overview

In this project, I built all of the routing and database logic for an article submission web app called The Scoop.

The Scoop allows users to:
- Create and log in to custom username handles
- Submit, edit, and delete articles containing a link and description
- Upvote and downvote articles
- Create, edit, and delete comments on articles
- Upvote and downvote comments
- View all of a user's articles and comments

## Run Locally

To start your server, run `npm install` and then `node server.js` from the root directory of this project. 

To view your local version of the site, open **index.html** in Google Chrome.

### Database Properties

* **comments** - an object with keys of IDs and values of the corresponding comments
  - id - Number, unique to each comment
  - body - String
  - username - String, the username of the author
  - articleId - Number, the ID of the article the comment belongs to
  - upvotedBy - Array of usernames, corresponding to users who upvoted the comment
  - downvotedBy - Array of usernames, corresponding to users who downvoted the comment

* **nextCommentId** - a number representing the ID of the next comment to create (to ensure all comments have unique IDs), initializes to `1`


### Route Paths and Functionality

**/comments**
- POST
  - Receives comment information from `comment` property of request body
  - Creates new comment and adds it to database, returns a 201 response with comment on `comment` property of response body
  - If body isn't supplied, user with supplied username doesn't exist, or article with supplied article ID doesn't exist, returns a 400 response

**/comments/:id**
- PUT
  - Receives comment ID from URL parameter and updated comment from `comment` property of request body
  - Updates body of corresponding comment in database, returns a 200 response with the updated comment on `comment` property of the response body
  - If comment with given ID does not exist, returns 404 response
  - If no ID or updated comment is supplied, returns 200 response
- DELETE
  - Receives comment ID from URL parameter
  - Deletes comment from database and removes all references to its ID from corresponding user and article models, returns 204 response
  - If no ID is supplied or comment with supplied ID doesn't exist, returns 400 response

**/comments/:id/upvote**
- PUT
  - Receives comment ID from URL parameter and username from `username` property of request body
  - Adds supplied username to `upvotedBy` of corresponding comment if user hasn't already upvoted the comment, removes username from `downvotedBy` if that user had previously downvoted the comment, returns 200 response with comment on `comment` property of response body
  - If no ID is supplied, comment with supplied ID doesn't exist, or user with supplied username doesn't exist, returns 400 response

**/comments/:id/downvote**
- PUT
  - Receives comment ID from URL parameter and username from `username` property of request body
  - Adds supplied username to `downvotedBy` of corresponding comment if user hasn't already downvoted the comment, remove username from `upvotedBy` if that user had previously upvoted the comment, returns 200 response with comment on `comment` property of response body
  - If no ID is supplied, comment with supplied ID doesn't exist, or user with supplied username doesn't exist, returns 400 response

**JSON Database load/save with Figg (node module)**

**loadDatabase**

- Reads a JSON file containing the database and returns a JavaScript object representing the database

**saveDatabase**

- Writes the current value of `database` to a JSON file


