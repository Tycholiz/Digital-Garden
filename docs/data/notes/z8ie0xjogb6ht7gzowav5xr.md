
K6 is an open-source framework for implementing [[load testing|testing.method.load]]

K6 tests work well when run within a [[CI-CD|deploy.CI-CD]] pipeline or on a schedule.

K6 does not use the [[js.v8]] engine and is not [[nodejs|js.node]] based.
- therefore we can't import node modules into our K6 script

## K6 Script
### Making scripts
K6 scripts can be made in several ways:
- *K6 test builder* - In the K6 UI, create the test with an endpoint
- *K6 recorder* - with a browser extension, simply record the actions by clicking through your application's UI.
- *converter* - generate K6 scripts from HAR files, Postman collections, [[OpenAPI|open-api]] specifications
- *manual* - write the script (with Javascript) by hand
  - tip: use VSCode extension

### Components
- The default export of the script is the entrypoint for the virtual users (VUs), similar to `main()` of other programming languages.
  - code inside this function runs over and over as long as the load test is running, while the other functions are *init* functions that only run once per VU.

## Gotchas
- all variables must be prefixed with `K6_` (can be added in the CI-CD tool UI, like Gitlab)
- `.js` files work best

## Resources
- [K6 test lifecycle](https://k6.io/docs/using-k6/test-life-cycle/)
- [K6 API (including built-in modules)](https://k6.io/docs/javascript-api/)