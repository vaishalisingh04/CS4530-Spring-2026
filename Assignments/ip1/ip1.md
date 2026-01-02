---
layout: assignment
title: "Individual Project 1"
permalink: /assignments/ip1
parent: Assignments
nav_order: 1
due_date: "Wednesday January 21, 2026 11:00am (EST)"
submission_notes: Submit Code through Github Classroom, Submit Link and Written Work on Gradescope
---

The individual and group projects for this course place you in the role of the most recent hire at GameNite, an interactive application for people who like playing, or commenting on, multiplayer strategy games. It recently received a large investment thanks due to its elevator pitch, “what if we had a version of Twitch, but for correspondence chess?”

GameNite is a web application that consists of some code that runs in each client's web browser, and also code that runs on a server. By the end of the semester, you’ll propose, design, implement, and test new features for our project. We understand that some of you may have some web development experience, but don’t expect that most of you do, the goal of this first project is to get you up to speed with our existing codebase and development environment.

## 1. Objectives of This Assignment

The objectives of this assignment are to:

- Get you familiar with the basics of TypeScript, VSCode, and the project codebase
- Get familiar with how REST API requests work
- Learn how to read and write code in TypeScript
- Learn how to write unit tests with Vitest
- Translate high-level requirements into code

## 2. Getting Started With This Assignment

If you were registered for the class before Monday, January 5, you should have an email from a TA with an invitation to our organization. Check your spam/junk folder and accept the invitation if you don't have it. If you cannot find the invitation, if you registered for the class late, or have any other issue that keeps you from getting started, please create a Piazza post to contact us and we will try to help.

After that accept our [GitHub Classroom Invitation for this assignment] (XXX TODO) . It will create a Github repository for you which will include the starter code for this assignment.

If you are new to Git, you may want to work on "Learning Basics of Git" [assignment](https://classroom.github.com/a/QkL5qQ90) first!

### 2.1. Prerequisites

You'll need to start by [setting up your development environment]({{site.baseurl}}{% link tutorials/week1-getting-started.md %}), including node.js and npm.

Next, make sure you have used Git to clone the starter code. This code is divided into 3 main directories: client, server, and shared. These are connected in a single [NPM workspace](https://docs.npmjs.com/cli/v7/using-npm/workspaces), but you shouldn’t have to worry much about what that means.

After that, follow the following steps:

1. Navigate to the root directory of the repo.

2. Install the dependencies:

   ```
   ip1-me $> npm install
   ```

3. Start the server:

   ```
   ip1-me $> cd server
   server $> npm run dev
   ```

   You should then be able to visit <http://localhost:8000> and get a message that tells you that you should be looking at elsewhere for the Vite frontend.

4. **Leave `npm run dev` running in the `./server` directory for the next step.**

5. Open a new terminal window and start [Vite](https://vite.dev/), which builds the frontend client:

   ```
   ip1-me $> cd client
   client $> npm run dev
   ```

   This will give you a url to go to, probably <http://localhost:4530/>. If you see a login page, then you’re up and running. You can create a new user or use one of the four automatically-created username-password combinations: `user0/pwd0000`, `user1/pwd1111`, `user2/pwd2222`, or `user3/pwd3333`.

### 2.2. Understanding "The Server"

The code in the server directory is all TypeScript that is intended to be run on a web server. It’s built on top of [express](https://expressjs.com/), a library used to create programs that accept and respond to HTTP requests. Express can do a lot of things, but we’re only going using it to send and receive chunks of JSON-formatted data (line 18 of `sever/src/app.ts` (XXX TODO check) tells the Express library to behave that way).

#### HTTP GET Requests

Your web browser makes one kind of HTTP request, a GET request, whenever you type in a URL and hit ENTER. If you go to <http://localhost:8000/api/thread/list> while the server is running, you will see a bunch of JSON-formatted data. Line 36 of `server/src/app.ts` (XXX TODO check) causes the request to that URL to be sent to the `getList` controller, which is just a function that is defined in `server/src/controllers/thread.controller.ts`.

HTTP GET requests are supposed to just look up information. It is considered bad behavior if a HTTP GET requests changes something about the world. In the context of GameNite, a HTTP GET request shouldn’t add a new comment, or create a new post, or initialize a new game.

#### HTTP POST Requests

A HTTP POST request, on the other hand, sends information to a server and receives information in return; it may change things about the state of the world. If you run this cURL command on while the server is running, it will make a new post that you can see if you go to the website’s frontend.

```
curl --location 'localhost:8000/api/thread/create' \
--header 'Content-Type: application/json' \
--data '{
    "auth": {
        "username": "user3",
        "password": "pwd3333"
    },
    "payload": {
        "title": "New Post",
        "text": "Text goes here"
    }
}'
```

(XXX TODO check this ^)

Line 35 of `server/src/app.ts` (XXX TODO check) causes this request to be sent to the `postCreate` controller, which is also just a function defined in `server/src/controllers/thread.controller.ts`.

You may find that it’s easier to make HTTP POST requests from a tool like Postman: see our Postman tutorial (XXX TODO link) for details. If you click the `</>` icon while developing a request, you can pick “cURL” from the dropdown menu and get a command-line snippet that performs the same request.

#### Other HTTP Requests

There is a whole vocabulary of HTTP requests, but you the most important two for this class are GET and POST.

### 2.3. Understanding "The Client"

The code in the `client` directory is mostly TypeScript that is intended to be run in a web browser. But web browsers don’t know how to run TypeScript, they only know how to run JavaScript. While you’re working on a project, it’s important to be able to be able to look at what your code is doing in a web browser, and when you’re done working on your project, all your code needs to be turned into a form that can get shipped to a web browser.

Strategy.town uses [Vite](https://vite.dev/) for this. Vite is a _build tool_ — you use to preview the website you’re building, and you use it to build the final website that you ship to users.

### 2.4. Shared Code

The TypeScript types in the shared directory are used by both the client and the server. The data sent by HTTP POST request to create a post has the TypeScript type `WithAuth<CreateThreadMessage>`, which, if you look at `shared/src/auth.types.ts` and `shared/src/thread.types.ts`, you can see expands to this TypeScript interface:

```typescript
type TypeOfPostRequest = {
  auth: {
    username: string;
    password: string;
  };
  payload: {
    title: string;
    text: string;
  };
}
```

The client can use the `WithAuth<CreateThreadMessage>` type to ensure that it is sending a message with the right structure to the server. Because these types are described with [zod](https://zod.dev/) schema validators, the server can use the same code to check that it received something with the right structure. (Usually the server gets things sent by the client, but as we saw, you can also use cURL to send random nonsense to the server. The server can’t trust anything about the structure of information it receives from a HTTP request!)

### 2.5. Testing

Unit tests for the server are in `server/tests/**/*.spec.ts`. These are written in [Vitest](https://vitest.dev/), a tool discussed in one of the first two lectures. (If you’ve used Jest, you should be good to go: Vitest is designed to be very similar.)

You can test the server by going to the `server` directory and running `npm run test`. It can be very helpful to have tests constantly rerunning whenever you edit code, which you can do by running `npm run vitest`.

## 3. Recommendations when working on the project

1. Open the client application in a browser and interact with it. While interacting, monitor the application tab in the browser’s developer tools. The application tab will give you information about the HTTP requests the client sends to the server. The HTTP requests will contain URIs in their headers. You can use this information to understand the endpoints in the server.
2. Use Postman to interact with and manually test your solutions and verify that database queries work as expected.
3. Make sure VS Code is set up as described in the development environment tutorial, with ESLint, Typescript, and Prettier installed.
4. Do not wait until the last minute to run npm run lint and npm run build to check for linter and typescript errors!
5. Follow the debugging policy to help in the debugging process.

## 4. Handing in the project

We will grade your code on GitHub by using the "Feedback" PR that is automatically created when the assignment is. Grades will be assigned on Gradescope.

On Gradescope, you will submit a plain `.txt` file containing three things:

 1. The link to your project's GitHub repo (e.g. `https://github.com/neu-cs4530/ip1-robsimmons`)
 2. The written responses and cURL commands requested in Task 3.

### TypeScript ESLint, Vitest, and Configuration Files

The GitHub project contains a number of configuration files you **may not modify**: `package.json`, `package-lock.json`, `.prettierrc`, `tsconfig.json`, `vitest.config.mjs`, and `vite.config.mjs` are configuration files, as is everything in the `.github` directory. If you change any of these files, take care to change them back; the list of changes in the feedback PR should not show any changes to these files. You also may not include use `eslint-disable` commands to disable ESLint's checks in your final submission.

The code you submit must pass GitHub's automatic checks, which mostly just amount to the TypeScript typechecker, the ESLint linter, and the tests. You can run these yourself like this:

```
ip1-me $> npm run check –workspaces
ip1-me $> npm run lint –workspaces
ip1-me $> npm run test –workspaces
```

When you push your code to GitHub, you can see the status icon for your most recent submission:

![image]({{site.baseurl}}{% link /Assignments/ip1/github-ci.png %})

After the tests run, this will turn into a red ❌ or a green ✅, and clicking on the icon will let you see details of the tests we ran.

The Actions tab on GitHub has the results of previous runs.

![image]({{site.baseurl}}{% link /Assignments/ip1/ActionsTab.png %})

**Up to 25% of your grade may be deducted for CI failures, and in severe cases we may decline to grade your assignment entirely. Give yourself sufficient time to find and fix any errors.**

## 5. Implementation Tasks

### Task 1: Tic-Tac-Toe

You should try to start a game of Tic-Tac-Toe in the development server. 
(Log in as two different users in two different windows.) Tic-Tac-Toe does not work! To make Tic-Tac-Toe work on GameNite, you'll need to fill in the missing implementation in `server/src/games/ticTacToe.ts`.

Places to look for guidance:
 - The type specification for a game implementation in `server/src/games/gameLogic.ts`, explains what each unimplemented function does.
 - The types of Tic-Tac-Toe in `shared/src/games/ticTacToe.ts`, explain how the game is intended to work.
 - The implementations of Nim and Guessing Game in the directory `server/src/games`, which may be a helpful basis for comparison.

### Task 2: Tests for Tic-Tac-Toe

The purpose of this part of the assignment is to get used to writing Vitest tests. *(Copilot-esque LLM autocompletion is quite good at completing tests. If you have Copilot-style autocompletion enabled, you are violating the course's academic integrity policy and risking a failing grade, and in addition, this task will be exceptionally pointless and boring.)*

Write tests for Tic-Tac-Toe in a new file, `server/tests/games/ticTacToe.spec.ts`. Your tests should achieve full branch coverage: when you run `npm run test` in the `server` directory, Vitest should report no "Uncovered Line #s" in `src/games/ticTacToe.ts`.

### Task 3: Exposing Errors in the User Service

There are several problems with the User service for GameNite. In the remainder of this assignment, you will begin addressing them.

The first and most glaring is that some of the functions, despite claiming to return `SafeUserInfo` objects, actually return passwords, which are then passed on to the controller and returned. REST API calls should never expose passwords in their responses like this!

 1. Investigate the five User REST API endpoints listed in the README, the User controller in `server/src/controllers/user.controller.ts`, and the User service in `server/src/services/user.service.ts`, to find some or all of the errors. In the written part of your submission, include a cURL command that result in the User API returning passwords.

 2. It’s quite embarrassing that these bugs exist despite the user service having 100% test coverage! Here's your chance to do test-driven development. **First**, modify the tests so that they actually fail (as they should!) on the current implementation. The User record’s random id and the username should never be exposed through REST API endpoints, and the tests should account for this requirement. **Second**, modify the user service so that it passes the new tests.

 3. In the written part of your submission, briefly explain why TypeScript allowed a SafeUserInfo-returning service function to include a password field despite SafeUserInfo having no password field.

 4. **Challenge**: there's another security error in the user service that's a little more difficult to find. Identify the bug and include a cURL command that exercises it, explain why the cURL command demonstrates something bad happening, create a failing test, and fix the bug. (This is just worth 5% of the overall score for this assignment. Don't get stuck here and neglect the other tasks!)

### Task 4: Creating an Auth model

There are two connected issues with the User model: 

1. There’s a frequent use of for-loops to loop through all the usernames to find a record that matches a specific username. 
2. Storing authentication information like a password alongside user profile information like a display name is not a great design: it can make accidental leaks like the ones we've encountered more likely!

These aren’t totally connected problems, but we can solve them together. The file `server/src/services/user.service.ts` contains an unused `storedAuths` object that you will use to maintain a mapping from valid **usernames** to the user IDs that let you look up profile information (like the display name) for that user. The AuthRecord defined in `server/src/models/auth.model.ts` also contains a password.

Whenever you create a user, you’ll continue to create a new and random user ID, and add to `storedUsers` an entry that maps from the user ID to the user’s record. You should modify `UserRecord` to no longer contain a password, and ensure that the stored record does not contain a password. Additionally, when you create a user, you’ll add an entry to `storedAuths` that maps the username to the `UserRecord`. By using stored auth, you can find the user record associated with a username in two steps: find the user ID using `storedAuths`, and then find the user’s record with that ID in `storedUsers`.

To complete this part, you should:
- Remove the `password` field from `UserRecord`, and instead
- Maintain the property that whenever you have `storedUsers[id] === user`, then `storedAuth[user.username].user === id`.
- Avoid having any functions in `user.service.ts` that loop over all elements in a JavaScript object in order to find one in particular.
- Ensure that all tests still pass and that the tests still provide total coverage for the User service.

### Task 5: Refactoring the Auth model

The purpose of this part of the assignment is to get your fingers comfortable with the functionality that TypeScript provides, and comfortable with seeing “red squiggy” errors in VSCode and then fixing them. *(Again: if you use LLM or Copilot-powered autocompletion on this part, you are doing nothing but interfering with your own learning.)*

You now have two different types of code mixed up in `user.service.ts` — code primarily concerned with authentication, and code primarily concerned with the user’s personal data. In this part, your goal is to move the following elements from `user.service.ts` to a new file, `auth.service.ts`:
- `storedAuth` (which should not be exported from `auth.service.ts`)
- `resetStoredAuth()`
- `getUserByUser()`
- `checkAuth()`
- `enforceAuth()`

The following should stay in `user.service.ts`:
- `disallowedUsernames` and `storedUsers` (which should not be exported)
- `populateSafeUserInfo()`
- `createUser()`
- `getUsersByUsername()`
- `updateUser()`
- `resetStoredUsers()`

You can add additional exported functions from `auth.service.ts` if needed — you’ll need to create at least one other helper function to create and/or update `AuthRecord` records, and call those helper functions within `user.service.ts`. 

Moving these functions will break a lot of other parts of the server, but using TypeScript, the linter, and your tests, it should not be too arduous to find what broke and fix it. 

## Grading

- Task 1: 25 points
- Task 2: 20 points
- Task 3: 20 points
- Task 4: 20 points
- Task 5: 15 points