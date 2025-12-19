---
layout: assignment
title: "Individual Project 1"
permalink: /assignments/ip1
parent: Assignments
nav_order: 1
due_date: "Wednesday January 21, 2026 11:00am (EST)"
submission_notes: Submit Code through Github Classroom and Repository Link on Gradescope
---

Welcome aboard to the GameNite team! We're glad that you're here and ready to join our development team as a new software engineer.
We're building an interactive application for an online community to share their knowledge and experience, and are very happy to see that we have so many new developers who can help make this application a reality.
By the end of the semester, you'll be able to propose, design, implement, and test new features for our project.
We understand that some of you may have some web development experience, but don't expect that most of you do, and hence, we have created an individual project to help you get up to speed with our existing codebase and development environment.

GameNite is a web application that consists of some code that runs in each client's web browser, and also code that runs on a server.

This implementation effort will be split across two deliverables. In this first deliverable, you will implement and test the core backend components for this feature, and in the second deliverable, you will implement and test the frontend components.

## Objectives of this assignment

The objectives of this assignment are to:

- Get you familiar with the basics of TypeScript, VSCode, and the project codebase
- Learn how to read and write code in TypeScript
- Translate high-level requirements into code
- Learn how to write unit tests with ViTest or Jest

## Getting started with this assignment

First step is to complete a Gradescope assignment IP0 to privide us with your github.com id (username) or is it khoury github id? does neu email have to be connected to account? We will invite you to our organization. Check your northeastern email or your spam/junk folder and accept the invitation to join neu-cs4530. You can also view invitations from [github site](github.com/settings/organizations). Once your submission is graded.

After that accept our invitation for this assignment. It will create a Github repository for you which will include the starter code for this assignment. 

If you facing any issue, please create a piazza post to contact us and we will try to help. 

If you are new to Git, you may want to work on "Learning Basics of Git" [assignment](https://classroom.github.com/a/QkL5qQ90) first!

### 1. Prerequisites

#### 1.1 Install Node.js and npm

If you haven't installed npm and node.js, follow the tutorial on setting up a development environment for this class

#### 1.2 Install MongoDB

We use MongoDB as the NoSQL database to store data related to this application.

1. Follow the [instructions in the official MongoDB documentation](https://www.mongodb.com/docs/manual/administration/install-community/) to install the free community edition.
2. Choose 'Install on Linux', 'Install on macOS', or 'Install on Windows', depending on your system. (the following steps are for Windows)
3. Scroll down to the section labeled 'Install MongoDB Community Edition.' and click on [MongoDB Download Center](https://www.mongodb.com/try/download/community?tck=docs_server).
4. For Windows, in the Package dropdown, select `msi`. Then download and run the installer.
5. On Windows, select the _“Install MongoDB as a Service”_ checkbox and install. This will start MongoDB as a background service.
6. Install "MongoDB Compass" if prompted.
7. Verify if the MongoDB server is running using the Windows Services app.

Mongo offers several methods of interacting with your Mongo databases.

- MongoDB Compass is an interactive application for creating, modifying, and querying MongoDB connections. It should be started as part of the installation process, showing a connection to `mongodb://localhost:27017/`.

  For Windows, install MongoDB Compass using the instructions above.

  For Mac:

  - Download the dmg file from https://www.mongodb.com/try/download/compass. Once the application starts:
    1.  Click on "Add new connection" in the left sidebar.
    2.  Make sure the URI field contains `mongodb://localhost:27017`
    3.  Click on "Connect" - MongoDB will need to be running as a macOS service

- Mongo shell (_mongosh_) provides a command-line interface that can be used to interact with databases in MongoDB.

  For Windows:

  - Download it [here](https://www.mongodb.com/try/download/shell) using the msi package. You can also use _mongosh_ to see if the MongoDB server is running. Try the MongoDB Community Edition and the command `show dbs`; you should see a list of existing databases in your local instance.

  For Mac:

  - Mongo shell is automatically installed with MongoDB through the Mac installation instructions. To use it, make sure MongoDB is running as a macOS service, then type `mongosh` into the terminal.

- Last and most important, you can use the [Mongoose api](https://mongoosejs.com/docs/index.html) to interact with MongoDB through your JavaScript or TypeScript programs. Mongoose will be installed as part of the installation process for the project starter code.

### 2. Install the starter code and its dependencies

The starter code package, of which this is a part, is divided into 3 main directories: _client_, _server_, and _testing_.

1. Navigate to the root directory of the repo.
2. Install the dependencies for each directory by running the following commands:

```shell

npm install from the project root
```

Once you install the dependencies, you might see the following ESlint errors in some files. The linter error indicates that the code contains carriage return characters (\r, represented as ␍) at the end of each line and usually happens when the file has Windows-style line endings (\r\n) instead of Unix-style line endings (\n). To fix this, you can click on the "CRLF" icon on the lower right corner of VSCode and change it to "LF". Note that this does not count as a linting error when grading.
![image]({{site.baseurl}}{% link /Assignments/ip1/linter-errors.png %})

**Note:** Please ignore the testing directory as it contains cypress tests that we do not cover in grading.

### 3. Setup Environment Variables\*\*

1. Create a file called `.env` in `./client`. In `./client/.env` ensure the following lines:

```
REACT_APP_SERVER_URL=http://localhost:8000
```

2. Create a file called `.env` in `./server`. In `./server/.env` ensure the following lines:

```
MONGODB_URI=mongodb://127.0.0.1:27017
CLIENT_URL=http://localhost:4530
PORT=8000
```

### 4. Populate the initial database

{: .note }
Right now, you may run into errors regarding unknown properties. Once you finish implementing both Task 1 and 2 and modify the schema, you should be able to populate the database correctly.

1. In the `server` directory, run `npm run populate-db` to populate the `fake_so` database with example data that follows the schema definition.
2. If you want to delete all the data at any point, you can use `npm run delete-db` to delete all entries in the `fake_so` database.

### 5. Familiarize Yourself with Project Dependencies

1. Ensure the following packages are installed: mongoose, express, jest, eslint, axios, cors, and nodemon.
2. Refer to the documentation for the following packages:

- [Express](https://expressjs.com/) is a framework to write server-side code.

- The [mongoose data modeling library](https://mongoosejs.com/docs/index.html). Mongoose provides JS/TS bindings to MongoDB, so we can manage and manipulate the data from our JS/TS programs.
- [nodemon](https://www.npmjs.com/package/nodemon) accelerates the development by automatically restarting a node application when file changes in the directory are detected.
- We use the [axios](https://axios-http.com/docs/intro) library to send HTTP requests to our server from our client application.

- We use [cors](https://www.npmjs.com/package/cors) to regulate the permissible connections between the
  clients and the server. The current cors configuration allows all
  connections to the server for convenience. This is fine for a development environment. In a
  production environment where the application is deployed on a cloud service, the CORS policy needs to be specified more strictly. Read more about CORS [https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).


#### Testing Dependencies

For comprehensive testing of your backend implementation, familiarize yourself with these testing libraries:

- **[Jest](https://jestjs.io/)** -Testing framework for unit tests. Jest provides test runners, assertion libraries, and mocking capabilities.

- **[Supertest](https://github.com/visionmedia/supertest)** - A library for testing HTTP endpoints. Supertest allows you to make HTTP requests to your Express server during tests and assert on the responses. It's particularly useful for integration testing of your API routes.

- **[@types/jest](https://www.npmjs.com/package/@types/jest)** and **[@types/supertest](https://www.npmjs.com/package/@types/supertest)** - TypeScript type definitions for Jest and Supertest to enable proper type checking in your test files.

#### Key Testing Concepts

- **Unit Tests**: Test individual functions in isolation (service layer functions)
- **Integration Tests**: Test how different parts work together (API endpoints with Supertest)
- **Mocking**: Replace external dependencies (database calls) with controlled mock responses
- **Test Coverage**: Ensure your tests cover different scenarios, edge cases, and error conditions

Example Supertest usage for testing an API endpoint:
```typescript
import request from 'supertest';
import app from '../app';

describe('POST /api/collections/create', () => {
  it('should create a new collection', async () => {
    const response = await request(app)
      .post('/api/collections/create')
      .send({
        name: 'Test Collection',
        description: 'A test collection',
        questions: [],
        username: 'testuser'
      })
      .expect(200);
    
    expect(response.body.name).toBe('Test Collection');
  });
});
```

### 6. Explore Useful Resources

1. Express Tutorial: [https://expressjs.com/en/guide/routing.html](https://expressjs.com/en/guide/routing.html)
2. MongoDB tutorial: A mini tutorial
3. Mongoose Queries: [https://mongoosejs.com/docs/queries.html](https://mongoosejs.com/docs/queries.html)
4. Mongoose Documents: [https://mongoosejs.com/docs/documents.html](https://mongoosejs.com/docs/documents.html)
5. Jest Basics: [https://jestjs.io/docs/getting-started](https://jestjs.io/docs/getting-started)
6. Mocking in Jest: [https://jestjs.io/docs/mock-functions](https://jestjs.io/docs/mock-functions)
8. Postman tutorial: API Requests & Postman tutorial

## Server/Client Architecture

The schemas for the database are documented in the directory `server/models/schema`.
A class diagram for the schema definition is
shown below:

![image]({{site.baseurl}}{% link /Assignments/ip1/class-diagram.png %})

The starter code package, of which this is a part, is divided into 3 main directories: _client_, _server_, and _testing_.

### Client

Running `npm run dev` will start a client on port 4530.
The client code uses Axios to send HTTP method requests to the server. You should review the client code to understand
how axios sends requests and how the response from the server is processed. You don’t need to make any changes to the client code.

### Server

The server is responsible for taking HTTP requests from the client and executing them on the database. The server code resides in the `server/` directory. The server is responsible for all the data that is sent back and forth to the database.

The main server script is in `server/server.ts`. Running `npm run dev`
will start an HTTP server, which will take HTTP requests on [**https://localhost:8000**](https://localhost:8000/), and execute them on the running database instance.

You can send requests to the server using a tool like Postman, or by writing scripts that use axios to send requests to `localhost:8000`, as you did in the Async activity.

When the server is terminated (using CTRL+C), the message **“Server closed.”** should be displayed. However, the MongoDB service will still be running (You can run `mongosh` to confirm)

### Testing

Unit tests for the server are in `server/tests/*/*.spec.ts`. These are written in Jest, which you are already familiar with.

To run the entire set of server tests, go to the server directory and say `npm run test`. Please ensure that the MongoDB server is up and running for the tests to pass.

If you want to run specific tests, we recommend that you install vsc-jest-runner in your VSC, and select the test or tests that you would like to run.

### Summary of the default host/port settings

|                   |                                                                        |
| :---------------- | :--------------------------------------------------------------------- |
| Client Instance   | [https://localhost:4530/](https://localhost:4530/)                     |
| Server Instance   | [https://localhost:8000/](https://localhost:8000/)                     |
| Database Instance | [mongodb://127.0.0.1:27017/fake_so](mongodb://127.0.0.1:27017/fake_so) |

## Recommendations when working on the project

1. Open the client application in a browser and interact with it. While interacting, monitor
   the application tab in the browser’s developer tools. The application tab will give you
   information about the HTTP requests the client sends to the server. The HTTP requests
   will contain URIs in their headers. You can use this information to understand the
   endpoints in the server.
2. Read the Jest tests. The Jest tests list all the endpoints the server should have, and the
   types of HTTP method associated with them. Further, the tests also have information
   about the Mongoose functions that need to be invoked for the service to send a
   successful response.
3. Start by defining the schemas in the server/models/schema directory to ensure the data structure is consistent.
4. Ensure that you run all Jest tests. These tests are designed to catch issues early. Once all Jest tests pass, the Cypress tests should also pass, assuming no significant changes have been made to the client’s implementation.
5. Use Postman to interact with and manually test your solutions and verify that database queries work as expected.
6. Follow the [debugging policy]({{ site.baseurl }}{% link debugging.md %}) to help in the debugging process.During visiting office hours, TA's will expect you to have exhausted all initial debugging strategies as outlined in the provided link.

## Implementation Tasks

This deliverable has 2 parts; each part will be graded on its own rubric.You should complete the assignment one part at a time, in the order presented here.

### Task 1: Collection

TBA

### Task 2: Communities

TBA

## Submission Instructions & Grading

You will submit your assignment by proving the link of your github classroom repository on Gradescope. Your will be committing your code to that GitHub Classroom. 

This submission will be scored out of 100 points, 80 of which will be awarded for implementation of tasks and accompanying tests, and the remaining 20 in the manual testing section covered below.

The grading repo has the autograder's script files in server/.grading. You are not allowed or supposed to change those files. Editing those files will amount to 0 for the entire assignment. If you accidentally changed them then let us know and we can help you revert those changes. During grading we will verify to see if you changed these files.

When you push to your repo, you will see the autograder run. In the actions tab of your repo look for Autograding Tests job to view your score and feedback from the autograder. The autograder has a total of 80 pts and the breakdown is as follows:
* **Task 1 (Collections)** - 25 pts
    * Correctness tests for service layer - 15 pts
    * Correctness tests for controller layer - 10 pts
* **Task 2 (Communities)** - 25 pts
    * Correctness tests for service layer - 15 pts
    * Correctness tests for controller layer - 10 pts
* **Style checks for lint** - 10 pts *(no partial credit is awarded)*
* **Regression tests** - 10 pts
* **Coverage for tests** - 10 pts

  | Coverage Percentage | Points Awarded |
  |-------------------|----------------|
  | ≥ 95% | 10 pts (Full Credit) |
  | ≥ 90% | 6 pts |
  | ≥ 85% | 2 pts |
  | ≥ 80% | 1 pt |
  | < 80% | 0 pts |
        
During development, you may encounter various linting errors. Some common errors include:
1. Line Ending Errors (CRLF vs LF)
    - You might see carriage return characters (\r, represented as ␍) at the end of lines
    - This usually happens when files have Windows-style line endings (\r \n) instead of Unix-style (\n)
2. Unused Variables/Functions
    - ESLint will flag unused variables or functions
3. Import/Export Issues
    - Missing imports or incorrect module paths

Your code will automatically be evaluated for linter errors and warnings.

- Lint errors result in a style score of 0/10. No partial credit is awarded in this case.
- Line endings will not be counted as errors.

The starter code comes with some lint problems, You are expected you to fix these linter problems, many of them will be fixed as you implement the tasks.

**The use of `eslint-disable` statements is NOT allowed. Each instance outside what is provided in the starter code will have points deducted.**

You can run the following command within the client or server to check for some common lint errors

```
npm run lint
```

You can run the following command within the client or server to fix some common lint errors

```
npm run lint:fix
```

#### Testing

You will be provided with starter code that includes a set of tests. Your task is to ensure that all existing tests pass and to create additional tests to cover any new functionality or edge cases.

**Please Note**: The server tests will fail the first time students run them but this is expected behavior.Please rerun the tests after you have completed the implementation.All server tests will pass after students have implemented the features as well as the corresponding tests. Please also  take note until all tests have passed, the Github actions will fail; this is also an expected behavior.These actions will pass when 1.Community and Collection features are implemented and 2.Tests are implemented.

### Manual Grading

Your code will be manually evaluated for conformance to our [course style guide]({{ site.baseurl }}{% link style.md %}). **Do not wait to run the linter until the last minute**. To check for linter errors, run the command `npm run lint` from the terminal. The handout contains the same ESlint configuration that is used by our grading script.

This manual evaluation will account for 20 points on this assignment. We will manually evaluate your code for style on the following rubric:

* **Manual grading** - 20 pts
    * **Documentation & design compliance** - 6 pts
        * All public properties and methods (other than getters, setters, and constructors) are documented with JSDoc-style comments that describe what the property/method does, as defined in our style guide.
        * The code and tests that you write generally follows the design principles discussed in week one. In particular, your design does not have duplicated code that could have been refactored into a shared method.
        * No duplicate code is allowed.
    * **Manual testing with Postman** - 14 pts


We will review your code and note each violation of this rubric. We will deduct 2 points for each violation, up to a maximum of deducting all 10 style points.

#### GitHub Actions for Test and Lint Output

Once your submission is pushed to your main branch, GitHub will automatically run linting and the tests in `server/tests` for your submission. Check the Actions tab on GitHub classroom to see the output of the run. This output will be used for grading, so ensure there are no errors in the Actions run.

![image]({{site.baseurl}}{% link /Assignments/ip1/ActionsTab.png %})

> {: .note }
> Please update the node version used in the github action workflow to be `22.x` instead of `20.x`. This will fix any memory leaks errors raised while executing the test suite. In particular, change lines 23-26 of `.github/workflows/main.yml` to be:

```yml
- name: Set up Node.js 22.x
  uses: actions/setup-node@v4
  with:
    node-version: "22.x"
```

#### Debugging

If you need help troubleshooting a problem, be sure to follow all the steps outlined in the course's [debugging policy]({{ site.baseurl }}{% link debugging.md %}). This will ensure you have exhausted all initial debugging strategies before reaching out for assistance from the TAs.

### Academic Integrity

**For this assignment, the use of AI tools is NOT allowed.** Please refer to the [course policy page]({{ site.baseurl }}{% link ai.md %}) for more details.


```
