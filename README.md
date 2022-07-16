# Payments Data Dashboard

This is a Circle Internet Financial interview problem to create a dashboard to display some peer-to-peer payments data and create new payments.  You've been given a web server that produces some data describing a stream of peer-to-peer payments.  You should create a React.js or Vue.js web dashboard that displays the data and creates new payments.


## Server

You are given a web server in the `server` directory that produces the payments data and accepts new payments data.  The `server` directory contains a `README.md` with documentation of the data format and instructions for getting the server running.  The server has a number of endpoints:
  - It has a `GET /payments` endpoint that produces payments data.  Every second it returns a different JSON object describing a single peer-to-peer payment.  If you query it multiple times within the same clock second, it will return the same sample payment.  Once the clock second has passed the endpoint will return a new sample payment, and you will be unable to retrieve the previous payment from the server again.
  - It has a `POST /payments` endpoint that allows you to record a new payment on the server.  This endpoint is intentionally flaky, and it will respond with an error code indicating success or failure.  Please note that even if the payment is successful, it will not show up on the `GET /payments` endpoint.

You are required to interact with these two endpoints (more on this below), but you are welcome to use any other endpoints on the server that seem helpful (in particular the `GET /users` endpoint may be useful).  The `README.md` in the `server` directory has instructions for interacting with each of the endpoints.

You may not modify the server's code, or use a different server, as part of your solution.  You also may not add an intermediary server of your own; you can only write browser-side code (besides whatever HTTP server your chosen framework provides to serve its client bundle).


## Dashboard

Your job is to create a web dashboard to interact with the server.  Your dashboard should have two main areas of functionality: listing existing payments, and creating new ones.  It must be written in some form of [React.js](https://reactjs.org/) or [Vue.js](https://vuejs.org/), though you're welcome to use additional frameworks like [Next.js](https://nextjs.org/) or [React Redux](https://react-redux.js.org/) on top.  You may not use a completely different reactive framework like [Angular.js](https://angularjs.org/).

### Listing Payments

Your dashboard should have a page that shows a list of the 25 most recent payments that reactively updates as the server produces new payments on the `GET /payments` endpoint, and incorporates created payments (described below).  For each payment you should show the sender's name, receiver's name, amount, and currency.  You should also have a search bar that allows dynamically filtering payments based on all fields returned by the API, not just the display names, amount, and currency.  You should put a little work into your UI to make it reasonably attractive, though it doesn't have to be perfect.  Using some pre-built components from a library like [Bootstrap](https://getbootstrap.com/) will be more than enough.

As described above the server will not allow you to query past payments, so you'll need to keep and update some client-side state to display historical payments.  You don't need to worry about persisting payments data in the browser across refreshes; you can start with an empty list of payments each time the page is reloaded.

### Creating Payments

Your dashboard should also have a simple web form to allow you to submit new payments to the `POST /payments` endpoint.  You should allow the user to enter the relevant details for the payment, and hen submit it.  As described in the server's `README.md`, the `POST /payments` endpoint has a particular data format that it requires, and you must make requests in this format.  Additionally, as described in the server's `README.md`, the endpoint is flaky, and will sometimes fail.  If the endpoint responds with a `503` code (the flaky error code), your form must automatically keep trying to submit the payment until the server responds with a `201` code (the success error code).  If you retry a payment after it is successful, the server will respond with a `409` code, and you should avoid your dashboard ever receiving this code.  You should not require the user to keep pressing a "submit" button until the payment is successful; your code should invisibily handle the necessary retries.

As described in the server's `README.md`, payments must be created for valid users.  The `GET /users` endpoint is provided to give you a list of valid users to use in this form.  We recommend, though do not require, that you use it.

Finally, note that payments submitted via this endpoint will not show up in the `GET /payments` endpoint.  However, you should still display payments created via the dashboard on your dashboard's list of payments.  Essentially, your payment list should be merging the data from the server with the manually created payments.


## Logistics

You should return to your recruiter a tarball containing your web dashboard that satisfies the above requirements, along with a README that describes how to get it running and any design choices you made that you think are particularly interesting.  We recognize that when job searching your time is valuable and so we think this exercise should take you about 5-7 hours; please do not feel that you need to spend much more time than that on it.  Please reach out to your recruiter if you have any questions about the exercise.  Thanks!

# Clay's Setup Instructions and Design Writeup:

Here is how to get up and running with this project. Note there are two separate `package.json` files: one for the frontend and one for the backend.

1. **Install and run backend**

  The backend is located in the `/server` directory. In your terminal, navigate to `/server` and run the following commands:

  ```sh
  npm install
  ```

  Followed by:

  ```sh
  npm start
  ```

2. **Install and run frontend**

The frontend is located in the directory `/app`. In a separate terminal tab, navigate to `/app` and run the same commands:

  ```sh
  npm install
  ```

  Followed by:

  ```sh
  npm start
  ```

Once everything is running, you should see the app running in your browser at http://localhost:3000/.

3. **Testing suite**

For the frontend, there are a few tests written in `/app/src/App.test.js` but there are mostly `test.todo()`s written to write tests for. To run this test suite, in a separate terminal tab, navigate to `/app` and run the command:

  ```sh
  npm run test
  ```


## Requirements:

  **General:**
  - [x] Code interacts with both API endpoints (`GET /payments` and `POST /payments`).
  - [x] Server's code is unmodified and the only server used.
  - [x] Frontend is written in [React.js](https://reactjs.org/).
  - [x] Dashboard has two main areas of functionality: listing existing payments, and creating new ones.
  - [x] UI is reasonably attractive, though not perfect.
  - [x] README that describes how to get it running (this file you're reading!) and any particularly noteworthy design choices

  **Payments feed**
  - [x] Feed shows a list of the 25 most recent payments and reactively updates as the server produces new payments on the `GET /payments` endpoint 
  - [X] Feed incorporates manually created payments (see below).
  - [x] For each payment, app displays the sender's name, receiver's name, amount, and currency.
  - [x] Search bar dynamically filters payments based on all fields returned by the API, not just the display names, amount, and currency.
  - [x] Payments data does not persist in the browser across refreshes; it starts with an empty list of payments each time the page is reloaded.
  - [x] Client-side state stores and displays historical payments.

  **Create payments form**
  - [x] Dashboard has a simple web form to allow you to submit new payments to the `POST /payments` endpoint. 
  - [x] If the `POST /payments` endpoint responds with a `503` code (the flaky error code), form automatically tries to submit the payment again until the server responds with a `201` code (the success error code).
  - [x] Retry code does not retry payment after it's successful (to avoid the `409` code).
  - [x] User does not have to keep pressing "Send" button (in fact, they cannot) until the payment is successful; the code invisibly handles the necessary retries.
  - [x] Payments are created only for valid users and `GET /users` endpoint is used to populate dropdown of valid users.
  - [x] Payments created manually show up on dashboard's list of payments.


## High level tech choices:
- I decided to use [React.js](https://reactjs.org/) and [Material UI](https://mui.com/) for the frontend
- I used [Axios](https://axios-http.com/) and [UUID](https://github.com/uuidjs/uuid) for making API calls and generating universally unique IDs, respectively.
- I did not use a central state container like [REDUX](https://redux.js.org/) for this project because the app is fairly simple and only has a few slices of state. As it grows in complexity we probably would want to refactor to use a more organized state management system.

## Tech Improvements for future iterations:
- The "Send" button disable functionality/state is not very clean- (state isn't the sole source of truth on whether button is disabled). I would like to refactor this to have state be the sole source of truth on whether button is disabled.
- More tests! Given more time I'd want to write more unit tests, particularly the ones I have written out as `test.todo()` in `/app/src/App.test.js`
- Better error handling: for most errors back from API the error is just logging to the console.
- Rewrite frontend in TypeScript! This seems to make everyone's life easier in the long run :)

## UX Design Choices

  - The sender and receiver name inputs are dropdowns populated from the API instead of free form text fields. This ensures correct names and avoids user error. Obviously with a real-world application involving money transfer, sender would be already determined based on authentication, and there would be security as well as UX issues with displaying every possible receiver in a dropdown.
  - When the user selects a sender in the dropdown, that name is removed from the receiver dropdown. Likewise, when a user selects a receiver in the dropdown, that name is removed from the sender dropdown. This is to avoid the undesired error of someone sending money from person X to person X (i.e. to themselves).
  - User cannot enter a negative amount of any currency.
  - User can add more than two decimal places for amount (e.g. 102.4882). While this is not ideal for most currencies like USD, which should only allow 2 decimals (e.g. $102.48), BTC is available in smaller increment amounts than cents, so the number of decimals user can enter for this version is unlimited.
  - No values are selected by default. While this is more work for the user, when the stakes are high (e.g. money involved) bias is towards requiring users to explicitly input each field (e.g. if we defaulted to BTC, user could think they are sending USD and end up sending 50 BTC instead of 50 USD!)
  - "Send" button is disabled by default and only enables once the required fields (sender, receiver, amount, and currency are selected).
  - Button disables after hitting "Send" and only enables again once a successful payment is made (i.e. `201` status code). E.g. if the API gives back `503` a few times, the retries will be done automatically and during that time the button will be disabled. Once the payment is successful, the button will be enabled.
  - After payment is successfully sent, the form clears out and a success toast appears on the bottom of screen.
  - Although there is a success toast at bottom of screen when payment succeeds, I did not include a failure message for the flakey API because we already have the retry mechanism that automatically fires and disables the button. I think it could make things more confusing for the user if they quickly see a flash of red error followed by green success.

  ## Ideas for future UX Improvements:

  - The columns sometimes resize depending on the size of the person's name which can be visually jarring- fix the column width to appropriate size.
  - Ability to have user specify how many Payments they want to see and potentially paginate results (e.g. have 3 pages with 25 on each)
  - Have Amount show the symbol (e.g. "$") for the currency that is selected
  - Better form validation for user- e.g. if fields are missing and "Send" button is attempted to hit while disabled, show which fields have missing info
  - Success Alert toast shows payment details (e.g. "Payment of $124.09 USD successfully sent from John Doe to Jane Doe!")
  - Adding more fields to table (e.g. date, memo) and allow sorting based on columns rather than chronological order