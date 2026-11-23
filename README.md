# ui-lab

ITCS386 Software Verification and Validation, Lab: Automated UI Testing.

Tool parts that I chose: **Part 2, Selenium WebDriver in Java** and **Part 4, Cypress**.
Both parts automate test case QP-01 (a custom pizza reaches the admin list) on
https://quickpizza.grafana.com. The results of every step are in [RESULTS.md](RESULTS.md).

## selenium-java (Part 2)

A Maven project with JUnit tests in `src/test/java/uilab`: a first test, the web form test,
a wait test, QP-01 written directly (`QP01Test`), and QP-01 with the page objects in `pages/`
(`QP01PageObjectTest`). `screenshots/` holds the pictures of steps 3, 4, 5 and 7 of each run,
and `reports/` holds the Surefire report of the last run. The tests need the environment
variable `CHROMEDRIVER` set to the path of a ChromeDriver that matches your Chrome.

```
cd selenium-java
mvn test -Dheadless=true
```

## cypress (Part 4)

A Cypress project with two specs in `cypress/e2e`: `web-form.cy.js` and `pizza-scenario.cy.js`.
The second one is QP-01, with `cy.intercept` on `POST /api/pizza` and the custom command
`cy.adminLogin` in `cypress/support/commands.js`. `cypress/videos` and `cypress/screenshots`
hold the evidence of a full run. Run `npm install` once in the folder before the first run.

```
cd cypress
npx cypress run --browser chrome
```
