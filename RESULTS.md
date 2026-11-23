# UI lab results

Tool parts that I chose: Part 2 (Selenium WebDriver in Java) and Part 4 (Cypress)

## Part 0
- Step 0.1, Chrome version: Google Chrome 157.0.8103.79 (arm64)
- Step 0.3, ChromeDriver version: ChromeDriver 157.0.8103.79 (6dc9bc3bdaea5c35ed6d41fceab2f7a3a2227c4f-refs/branch-heads/8103@{#1574})
- Step 0.4, locator of Pizza, Please!: `button[name="pizza-please"]` (CSS, 1 match)
- Step 0.4, locator of Excluded tools: `select` (CSS, the only select on the page, 1 match; options Knife, Pizza cutter, Scissors)
- Step 0.4, locator of Must be vegetarian: `#default-checkbox` (CSS id, 1 match)
- Step 0.4, locator of Custom Pizza Name: `input#pizza-name` (CSS, 1 match; `#pizza-name` alone finds 2 after Pizza, Please!, the box and the h2 of the answer)
- Step 0.5, pizza name, tool and calories of the run by hand: Lab Pizza 20261124-002500, tool Scissors, 525 calories per slice

| Step | Actual result | Pass or fail |
|------|---------------|--------------|
| 1    | The heading reads Looking to break out of your pizza routine? | Pass |
| 2    | Max Calories per Slice (holding 1000), Excluded tools, Must be vegetarian and Custom Pizza Name appeared | Pass |
| 3    | Fields showed 600, Knife selected, vegetarian ticked, Lab Pizza 20261124-002500 | Pass |
| 4    | Answer: Name: Lab Pizza 20261124-002500, Tool: Scissors, Calories per slice: 525 | Pass |
| 5    | The page said Please log in first. | Pass |
| 6    | Both boxes held admin; after Sign in the page showed Latest pizza recommendations | Pass |
| 7    | Lab Pizza 20261124-002500 (tool=Scissors, ingredients_number=6), same tool as step 4 | Pass |

## Part 2: Selenium WebDriver in Java
- Step 2.4, the failure line for the wrong title: `uilab.FirstTest.webFormOpens -- Time elapsed: 1.259 s <<< FAILURE!`
- Step 2.15, QP01Test run 1, result and time: BUILD SUCCESS, Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, test time 7.675 s, Maven total time 8.554 s
- Step 2.15, QP01Test run 2, result and time: BUILD SUCCESS, Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, test time 12.60 s, Maven total time 13.624 s
- Step 2.20, full run 1, time and number of tests: BUILD SUCCESS, Tests run: 5, Failures: 0, Errors: 0, Skipped: 0, test time 7.809 s+0.858 s+18.95 s+0.968 s+2.082 s, Maven total time 31.413 s
- Step 2.20, full run 2, time and number of tests: BUILD SUCCESS, Tests run: 5, Failures: 0, Errors: 0, Skipped: 0, test time 12.71 s+0.846 s+7.606 s+1.856 s+1.682 s, Maven total time 25.713 s

## Part 4: Cypress
- node --version, npm --version and npx cypress version:
- Step 4.3, failure message for the wrong title:
- Step 4.6, tool and calories in the answer of POST /api/pizza:
- Step 4.7, the line of the admin list that shows your pizza:
- Step 4.8, run 1, result, duration and pizza name:
- Step 4.8, run 2, result, duration and pizza name:
- Step 4.9 (optional), browser and result:

## Part B
1.
2.
3.
4.
5.
6.
7.
