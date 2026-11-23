# UI lab results

Tool parts that I chose: Part 2 (Selenium WebDriver in Java) and Part 4 (Cypress)

## Part 0
- Step 0.1, Chrome version: Google Chrome 157.0.8103.79 (arm64)
- Step 0.3, ChromeDriver version: ChromeDriver 157.0.8103.79 (6dc9bc3bdaea5c35ed6d41fceab2f7a3a2227c4f-refs/branch-heads/8103@{#1574})
- Step 0.4, locator of Pizza, Please!: `button[name="pizza-please"]` (CSS, 1 match)
- Step 0.4, locator of Excluded tools: `select` (CSS, the only select on the page, 1 match; options Knife, Pizza cutter, Scissors)
- Step 0.4, locator of Must be vegetarian: `#default-checkbox` (CSS id, 1 match)
- Step 0.4, locator of Custom Pizza Name: `input#pizza-name` (CSS, 1 match; `#pizza-name` alone finds 2 after Pizza, Please!, the box and the h2 of the answer)
- Step 0.5, pizza name, tool and calories of the run by hand: Lab Pizza 20261124-003914, tool Pizza cutter, 525 calories per slice

| Step | Actual result | Pass or fail |
|------|---------------|--------------|
| 1    | The heading read Looking to break out of your pizza routine? | Pass |
| 2    | Max Calories per Slice (holding 1000), Excluded tools, Must be vegetarian and Custom Pizza Name appeared | Pass |
| 3    | Fields showed 600, Knife selected, vegetarian ticked, Lab Pizza 20261124-003914 | Pass |
| 4    | Answer: Name: Lab Pizza 20261124-003914, Dough: Thin, Tool: Pizza cutter, Calories per slice: 525 | Pass |
| 5    | The page said Please log in first. | Pass |
| 6    | Both boxes held admin; after Sign in the page showed Latest pizza recommendations | Pass |
| 7    | Lab Pizza 20261124-003914 (tool=Pizza cutter, ingredients_number=8) (newest), the same tool as in step 4 | Pass |

## Part 2: Selenium WebDriver in Java
- Step 2.4, the failure line for the wrong title: `FirstTest.webFormOpens:20 expected: <Web Form> but was: <Web form>`
- Step 2.15, QP01Test run 1, result and time: BUILD SUCCESS, Tests run: 1, Failures: 0, Errors: 0, test time 7.675 s (Maven total 8.554 s)
- Step 2.15, QP01Test run 2, result and time: BUILD SUCCESS, Tests run: 1, Failures: 0, Errors: 0, test time 12.60 s (Maven total 13.624 s)
- Step 2.20, full run 1, time and number of tests: BUILD SUCCESS, 5 tests (FirstTest, WebFormTest, WaitTest, QP01Test, QP01PageObjectTest), 0 failures, Maven total time 31.413 s (QP01PageObjectTest 7.809 s, QP01Test 18.95 s)
- Step 2.20, full run 2, time and number of tests: BUILD SUCCESS, 5 tests, 0 failures, Maven total time 25.713 s (QP01PageObjectTest 12.71 s, QP01Test 7.606 s); report in `selenium-java/reports`

## Part 4: Cypress
- node --version, npm --version and npx cypress version: node v24.22.1, npm 11.19.1, Cypress package version 16.2.1, Cypress binary version 16.2.1, Electron 41.7.0, bundled Node 24.15.0
- Step 4.3, failure message for the wrong title: `Timed out retrying after 10000ms` / `+ expected - actual` / `-'Web form'` / `+'Web Form'`
- Step 4.6, tool and calories in the answer of POST /api/pizza: tool Pizza cutter, calories 350 (run 2, Lab Pizza 20261124-004530; the page showed the same values, Tool: Pizza cutter and Calories per slice: 350)
- Step 4.7, the line of the admin list that shows your pizza: Lab Pizza 20261124-004530 (tool=Pizza cutter, ingredients_number=5), second in the list because another user's pizza, A Free Calzone, arrived first (newest)
- Step 4.8, run 1, result, duration and pizza name: All specs passed!, 2 of 2 passing, 00:11 in total (pizza-scenario.cy.js 00:09, web-form.cy.js 00:01), Lab Pizza 20261124-004507 (Tool: Pizza cutter, Calories per slice: 425)
- Step 4.8, run 2, result, duration and pizza name: All specs passed!, 2 of 2 passing, 00:16 in total (pizza-scenario.cy.js 00:14, web-form.cy.js 00:02), Lab Pizza 20261124-004530 (Tool: Pizza cutter, Calories per slice: 350)
- Step 4.9 (optional), browser and result: not done, because Firefox is not installed on this Mac

## Part B
1. In Step 0.4, `$$("#floating_filled").length` returned 3, because Max Calories per Slice, Min Number of Toppings and Max Number of Toppings all share the id `floating_filled`, so the id cannot tell the boxes apart. The tests therefore find the label text Max Calories per Slice and step to its input, with `/preceding-sibling::input` in Java and `.siblings('input')` in Cypress. QuickPizza could give each box a unique id such as `max-calories`, or a test attribute such as `data-testid="max-calories"`. A test could then use `By.id("max-calories")` or `cy.get('#max-calories')`, which is shorter and faster and does not break when the label text is reworded or translated.

2. Comparing the tool in the admin list with the tool in the recommendation is a consistency oracle: two parts of the same system must agree with each other, so the test needs no separately computed expected value. In my Cypress run 2 the answer said Tool: Pizza cutter, and the admin line read Lab Pizza 20261124-004530 (tool=Pizza cutter, ingredients_number=5). The check would catch a fault where the server stores a different tool from the one it sends to the customer. For example, the save to the database could pick the tool again at random or store the default Knife, and the customer page would still look correct.

3. A check could read the ingredients in the answer of `POST /api/pizza` (in Cypress, `response.body.pizza.ingredients`, which `cy.intercept` already captures) and assert that every ingredient is vegetarian, and also that `response.body.vegetarian` is true. The page only lists ingredient names such as Mozzarella or Eggplant, so the test needs an oracle that says which ingredients are vegetarian. That oracle could be a `vegetarian` flag on each ingredient in the API response, or a list of meat ingredients (pepperoni, ham, anchovies and so on) kept in the test data. With such a list, the test fails as soon as one ingredient of the answer is on it.

4. Selenium needed an explicit wait at every point where QuickPizza draws something late: `visibilityOfElementLocated` for the heading, `presenceOfElementLocated` for the Knife option, `textToBePresentInElementLocated` for the answer, and `svelte-announcer` on the admin page. Without a wait, WaitTest failed with `NoSuchElementException ... #box0`. Cypress waited by itself, because every `cy.get` and `should` retried for up to 10 seconds, so the `#svelte-announcer` check was the only wait I wrote on purpose. Writing QP-01 was quicker in Cypress, about 50 lines in one spec, than in Java, which needed QP01Test, BaseTest and later two page objects. The Maven report gave only lines such as `FirstTest.webFormOpens:20 expected: <Web Form> but was: <Web form>` plus the screenshots that my own code took. Cypress recorded a video of every spec with the Command Log and showed the diff `-'Web form' +'Web Form'`, which was easier to read.

5. I would choose Cypress for Project 2. First, its automatic retrying removed the waiting code that took most of my effort in Java, and both full runs passed (00:11 and 00:16) with no explicit wait except the SvelteKit check. Second, it records a video and screenshots of a full run once `video: true` is set, which is the evidence that Project 2 asks for, while Selenium needed my own `shot()` method and would need a separate screen recording for a video. `cy.intercept` also let me compare the page with the server answer (Tool: Pizza cutter, calories 350), an extra oracle that my Java tests did not have.

6. (Part 2) If QuickPizza changed the locator of the calories box, QP01Test would need a change at the line inside the test method that builds the XPath `//label[normalize-space()='Max Calories per Slice']/preceding-sibling::input`. Every other test that types into that box would need the same fix. In QP01PageObjectTest nothing changes: only the constant `MAX_CALORIES` at the top of `HomePage.java` changes, and `showAdvanced`, `setMaxCalories` and `maxCalories` all use it. The page object keeps every locator of the page in one place, so the repair is one line, however many tests call `home.setMaxCalories(600)`.

7. (Part 4) Cypress clears the cookies, local storage and page before each `it`, and QuickPizza runs on several servers with a cookie that keeps one browser on one server. The admin list shows only the pizzas of that server, so steps 6 and 7 must run in the same test as steps 1 to 5 to stay on the server that created Lab Pizza 20261124-004530. If steps 6 and 7 were a second test, it would start without the cookie and could reach another server. `cy.contains('li', pizzaName)` would then time out after 10 seconds, because the pizza is not in that list. The second test would also lose `pizzaName` and the alias `@tool`, because Cypress resets aliases between tests.
