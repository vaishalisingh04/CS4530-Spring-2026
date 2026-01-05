---
layout: page
title: Test-Driven Development
nav_exclude: true
---

## Activity: Test-Driven Development

This activity is intended to supplement the CS4530 lecture on test-driven development. You will need to have already completed the development environment setup for the class.

### Steps
1. Download and unpack the [starter Code](https://github.com/neu-se/transcript-service/archive/refs/tags/s26-module2.zip)
2. Run `npm install` inside the directory that is created when you unzip the download.
3. Check that every member of the group can run 
   
    ```
    npx vitest --run src/types.spec.ts
    ```

    and see one failing and one succeeding test.

4. Check that every member of the group can run 

    ```
    npx vitest --run src/transcript.service.spec.ts
    ```

    and see five passing tests.

5. Check that every member of your group can see the ESLint error in `src/transcript.service.ts`. Discuss how to fix the error.

6. Write down two testable behaviors for the `addGrade` function corresponding to the condition of satisfaction that “the user can add a new grade for an existing student.” These should be written as `it("<testable beahvior>", () => {})` in `src/transcript.service.spec.ts`. 

7. Come up with at least two ways in which the `addGrade` function isn't completely specified by the conditions of satisfaction given in lecture. Document these as comments in `src/transcript.service.spec.ts`.

8. Add Vitest tests to your testable behaviors in `src/transcript.service.spec.ts`. (There should be at least four new tests for `addGrade` at this point.)

9. When you are done, submit the modified `src/transcript.service.spec.ts` file as required by your instructor.

### Grading Rubric:

Grading will be done via "specification grading". 

There are only two non-zero grades: Satisfactory (10 points), and Minimal (5 points).

To obtain a grade of Satisfactory, you must deliver all of the requirements listed in the instructions.

To obtain a grade of Minimal, you must have at least two meaningful tests for `addGrade` in `src/transcript.service.spec.ts`.

Grading will be manual; the TAs will look at your solution; they may or may not run them.