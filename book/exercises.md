# Exercises and Quizzes

Exercises and quizzes can be activated using the **plugins** configuration in your `book.json`. Example of book.json:

```js
{
    "plugins": [ "exercises", "quizzes" ]
}
```

#### Exercises

A book can contain interactive exercises (currently only in Javascript). An exercise is a code challenge provided to the reader, which is given a code editor to write a solution which is checked against the book author's validation code.

An exercise is defined by 4 simple parts:

* Exercise **Message**/Goals (in markdown/text)
* **Initial** code to show to the user, providing a starting point
* **Solution** code, being a correct solution to the exercise
* **Validation** code that tests the correctness of the user's input

Exercises needs to start and finish with a separation bar (```---``` or ```***```). It should contain 3 code elements (**base**, **solution** and **validation**). It can contain a 4th element that provides **context** code (functions, imports of libraries etc ... that shouldn't be displayed to the user).

    ---

    Define a variable `x` equal to 10.

    ```js
    var x =
    ```

    ```js
    var x = 10;
    ```

    ```js
    assert(x == 10);
    ```

    ```js
    // This is context code available everywhere
    // The user will be able to call magicFunc in his code
    function magicFunc() {
        return 3;
    }
    ```

    ---


#### Quizzes

A book can also contain interactive quizzes.

A Quiz is defined in the same way as an exercise.

    ---

    Here is the introduction for the quiz

    This is Question 1:
    - [x] This is the proposition 1 (the correct one)
    - [ ] This is the proposition 2

    > This is a help message when the answer to question 1 is wrong

    This is Question 2:
    - [ ] This is the proposition 1
    - [x] This is the proposition 2 (correct)
    - [x] This is the proposition 3 (correct)

    > This is a help message when the answer to question 2 is wrong

    ---

