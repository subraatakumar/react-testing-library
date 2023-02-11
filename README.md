# React Testing

## What should you test ?

- High value features
- Edge cases in high value features
- Things that are easy to break
- Basic React component testing
    - User Interactions
    - Conditional Rendering
    - Utils/Hooks

## Manual Vs. Automation Testing

It's important to make the distinction between manual and automated tests. Manual testing is done in person, by clicking through the application or interacting with the software and APIs with the appropriate tooling. This is very expensive since it requires someone to setup an environment and execute the tests themselves, and it can be prone to human error as the tester might make typos or omit steps in the test script.

Automated tests, on the other hand, are performed by a machine that executes a test script that was written in advance. These tests can vary in complexity, from checking a single method in a class to making sure that performing a sequence of complex actions in the UI leads to the same results. It's much more robust and reliable than manual tests – but the quality of your automated tests depends on how well your test scripts have been written.

## Types of Testing

- Unit Testing

Unit tests are very low level and close to the source of an application. They consist in testing individual methods and functions of the classes, components, or modules used by your software. Unit tests are generally quite cheap to automate and can run very quickly by a continuous integration server.

- Integration

Integration tests verify that different modules or services used by your application work well together. For example, it can be testing the interaction with the database or making sure that microservices work together as expected. These types of tests are more expensive to run as they require multiple parts of the application to be up and running.

- End to End

End-to-end testing replicates a user behavior with the software in a complete application environment. It verifies that various user flows work as expected and can be as simple as loading a web page or logging in or much more complex scenarios verifying email notifications, online payments, etc...

End-to-end tests are very useful, but they're expensive to perform and can be hard to maintain when they're automated. It is recommended to have a few key end-to-end tests and rely more on lower level types of testing (unit and integration tests) to be able to quickly identify breaking changes.

## Other Types of Testing

In addition to above three there are other types of testing as below :

- Acceptance Testing

Acceptance tests are formal tests that verify if a system satisfies business requirements. They require the entire application to be running while testing and focus on replicating user behaviors. But they can also go further and measure the performance of the system and reject changes if certain goals are not met.

- Performance Testing

Performance tests evaluate how a system performs under a particular workload. These tests help to measure the reliability, speed, scalability, and responsiveness of an application. For instance, a performance test can observe response times when executing a high number of requests, or determine how a system behaves with a significant amount of data. It can determine if an application meets performance requirements, locate bottlenecks, measure stability during peak traffic, and more. 

- Somke Testing

Smoke tests are basic tests that check the basic functionality of an application. They are meant to be quick to execute, and their goal is to give you the assurance that the major features of your system are working as expected.

Smoke tests can be useful right after a new build is made to decide whether or not you can run more expensive tests, or right after a deployment to make sure that they application is running properly in the newly deployed environment.

In this course we will learn about `Unit Testing` and `Integration Testing`.

## Create A New React App

```js
npx create-react-app@latest testing
cd testing
code . // Open the code in VSCode
```

now open package.json file. You will see the below lines in dependencies.

As we are using create-react-app, Jest (and React Testing Library) comes by default with the installation. If you are using a custom React setup, you need to install and set up Jest (and React Testing Library) yourself.

```js
    "@testing-library/jest-dom": "^5.16.5",
    "@testing-library/react": "^13.4.0",
    "@testing-library/user-event": "^13.5.0",
```

Jest offers you the following functions for your tests:

```js
describe('my function or component', () => {
  it('does the following', () => {
    expect(something).toBe(anotherthing);
  });
});
```

- describe: It is the test suite which can have multiple test cases.
- it: It is the test case. we can also name it `test` instead of `it`.
- expect: It is called assertion. It either turn out to be successful (green) or erroneous (red).

## Write first test case

Out App.test.js file already have test cases. below that add this test case.

```js
describe('true is truthy and false is falsy', () => {
  it('true is truthy', () => {
    expect(true).toBe(true);
  });

  it('false is falsy', () => {
    expect(false).toBe(false);
  });
});
```

Now your App.test.js may look like below:

```js
import { render, screen } from '@testing-library/react';
import App from './App';

test('renders learn react link', () => {
  render(<App />);
  const linkElement = screen.getByText(/learn react/i);
  expect(linkElement).toBeInTheDocument();
});

describe('true is truthy and false is falsy', () => {
  it('true is truthy', () => {
    expect(true).toBe(true);
  });

  it('false is falsy', () => {
    expect(false).toBe(false);
  });
});
```

Now go to terminal and run the below command 

```js
npm run test
```

When we run the test command, Jest's test runner matches all files with a test.js suffix by default. You could configure this matching pattern and others things in a custom Jest configuration file.

Output:

![](https://firebasestorage.googleapis.com/v0/b/mymasai-school.appspot.com/o/react-testing%2Ftest1.JPG?alt=media&token=7b7795a2-6aa4-4081-9622-3d11c9d959ed)


Note: If you are using any `Jest` extension in Vs-Code then you can see the green ticks in VS Code itself.

![](https://firebasestorage.googleapis.com/v0/b/mymasai-school.appspot.com/o/react-testing%2Ftest2.JPG?alt=media&token=a39215b9-7c2c-494f-8411-dba498127480)

In a real world project, your javascript function will be in another file and test cases will be on another. Let's create below two files.

```js
// sum.js
function sum(a,b){
    return a+b;
}

export default sum;
```

```js
// sum.test.js
import sum from "./sum";

describe("sum of 2 and 4 should be 6", () => {
  it("sums up two values", () => {
    expect(sum(2, 4)).toBe(6);
  });
});
```

## Summary Till Now

Jest is a test runner, which gives you the ability to run tests from command line. It offers us functions for test suits, test cases and assertions. 

## VITEST VS REACT TESTING LIBRARY

Vitest is a popular alternative to Jest, especially when being used in Vite. Vitest can be seen as direct replacement to Jest, because it also comes with a test runner, test suites (describe-block), test cases (it-block), and assertions (e.g. expect). I recommend Vitest to everyone who is using Vite instead of create-react-app (also recommended) for single page applications. You can also use React Testing Library in Vite.

## RTL: Rendering a component

RTL's render function takes any JSX as argument to render it as output. Afterward, you should have access to the React component in your test. To convince yourself that it's there, you can use RTL's debug function.

```js
// Counter.jsx
import React, { useState } from "react";

const Counter = () => {
  const [ counter, setCounter ] = useState(0);

  return (
    <div>
      <h1>Counter App</h1>
      <button onClick={() => setCounter((prev) => prev - 1)}>-</button>
      {counter}
      <button onClick={() => setCounter((prev) => prev + 1)}>+</button>
    </div>
  );
};

export default Counter;
```

```js
import React from "react";
import { render, screen } from "@testing-library/react";
import Counter from './Counter'

describe('<Counter />',()=>{
    test('counter renders successfully',()=>{
        render(<Counter />);
        screen.debug();
    })
})
```

Output:

```js
    <body>
      <div>
        <div>
          <h1>
            Counter App
          </h1>
          <button>
            -
          </button>
          <button>
            +
          </button>
        </div>
      </div>
    </body>
```

After running your test on the command line, you should see the HTML output of your component. The great thing about it, React Testing Library doesn't care much about different React features (useState, event handler, props) and concepts (controlled component).

## RTL : Selecting Elements

After you have rendered your React component(s), React Testing Library offers you different search functions to grab elements. These elements are then used for assertions or for user interactions. But before we can do these things, let's learn about how to grab them:

```js
import React from "react";
import { render, screen } from "@testing-library/react";
import Counter from "./Counter";

describe("<Counter />", () => {
  test("counter renders successfully", () => {
    render(<Counter />);
    screen.debug();
    //expect(screen.getByText(/Counter app/i)).toBeInTheDocument();
    expect(
      screen.getByText("Counter app", { exact: false })
    ).toBeInTheDocument();
  });
});
```

In the above test assertion, you can use either regex or {exact: false} to ignore the case of letters.

## Search Type and Search Variant 

In `getByText` , `getBy` is called search variant and `Text` is called search Type.

### Different Search Types

- getByText:
- getByRole: Based on [implicit roles of html element](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles).
- getByLabelText: ` <label for="search" />`
- getByPlaceholderText:  `<input placeholder="Search" />`
- getByAltText: `<img alt="profile" />`
- getByDisplayValue: `<input value="JavaScript" />`
- getByTestId: assign `data-testid` attribute in html element

### Different Search Variant

- queryByText
- queryByRole
- queryByLabelText
- queryByPlaceholderText
- queryByAltText
- queryByDisplayValue

- findByText
- findByRole
- findByLabelText
- findByPlaceholderText
- findByAltText
- findByDisplayValue

## What's the difference between getBy vs queryBy?

The big question in the room: When to use getBy and when to use the other two variants queryBy and findBy. You already know that getBy returns an element or an error. This makes it difficult to check for elements which shouldn't be there. Because getBy throws an error before we can make the assertion.

So every time you are asserting that an element isn't there, use queryBy.

```js
test("There should not be input element", () => {
  render(<Counter />);
  // expect(screen.getByRole("input")).toBeNull();
  expect(screen.queryByRole("input")).toBeNull();
});
```

In above example `getByRole` will through error `TestingLibraryElementError: Unable to find an accessible element with the role "input"`.

## When to use findBy?

The findBy search variant is used for asynchronous elements which will be there eventually.

```js
// UserScreen.jsx
import * as React from "react";

const getUser = () => {
  return Promise.resolve({ id: "1", name: "Robin" });
};

function UserScreen() {
  const [search, setSearch] = React.useState("");
  const [user, setUser] = React.useState(null);

  React.useEffect(() => {
    const loadUser = async () => {
      const user = await getUser();
      setUser(user);
    };

    loadUser();
  }, []);

  function handleChange(event) {
    setSearch(event.target.value);
  }

  return (
    <div>
      {user ? <p>Signed in as {user.name}</p> : null}

      <Search value={search} onChange={handleChange}>
        Search:
      </Search>

      <p>Searches for {search ? search : "..."}</p>
    </div>
  );
}

export default UserScreen;

function Search({ value, onChange, children }) {
  return (
    <div>
      <label htmlFor="search">{children}</label>
      <input id="search" type="text" value={value} onChange={onChange} />
    </div>
  );
}
```

```js
import * as React from "react";
import { render, screen } from "@testing-library/react";

import UserScreen from "./UserScreen";

describe("App", () => {
  it("renders App component", async () => {
    render(<UserScreen />);

    expect(screen.queryByText(/Signed in as/)).toBeNull();
    expect(await screen.findByText(/Signed in as/)).toBeInTheDocument();
  });
});
```

## [Test-driven development (TDD) ](https://en.wikipedia.org/wiki/Test-driven_development)

It is a software development process relying on software requirements being converted to test cases before software is fully developed, and tracking all software development by repeatedly testing the software against all test cases. This is as opposed to software being developed first and test cases created later.

```js
// CommentForm.test.js
import React from "react";
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import CommentForm from "./CommentForm";

describe("<CommentForm />", () => {
  it("initial render testing", () => {
    render(<CommentForm />);
    const commentInput = screen.getByRole("textbox");
    expect(commentInput).toBeInTheDocument();
    const checkbox = screen.getByLabelText("i agree to terms and conditions", {
      exact: false,
    });
    expect(checkbox).toBeInTheDocument();
    const submitButton = screen.getByRole("button", {
      name: "comment",
      exact: false,
    });
    expect(submitButton).toBeDisabled();
  });
});
```

## Multiple Elements

- getAllBy
- queryAllBy
- findAllBy

## Assertive Functions

Assertive functions happen on the right hand-side of your assertion.

- toBeDisabled
- toBeEnabled
- toBeEmpty
- toBeEmptyDOMElement
- toBeInTheDocument
- toBeInvalid
- toBeRequired
- toBeValid
- toBeVisible
- toContainElement
- toContainHTML
- toHaveAttribute
- toHaveClass
- toHaveFocus
- toHaveFormValues
- toHaveStyle
- toHaveTextContent
- toHaveValue
- toHaveDisplayValue
- toBeChecked
- toBePartiallyChecked
- toHaveDescription






