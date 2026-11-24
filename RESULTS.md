# UI lab results

Tool parts that I chose: **Part 2 - Selenium WebDriver in Java** and **Part 4 - Cypress**.

## Part 0

- Step 0.1, Chrome version: `157.0.8103.79`
- Step 0.3, ChromeDriver version: `ChromeDriver 157.0.8103.79`
- Step 0.4, locator of Pizza, Please!: `button[name="pizza-please"]`
- Step 0.4, locator of Excluded tools: `select`
- Step 0.4, locator of Must be vegetarian: `#default-checkbox`
- Step 0.4, locator of Custom Pizza Name: `input#pizza-name`
- Step 0.5, pizza name, tool and calories of the run by hand: `Lab Pizza 20261124-205230`, Scissors, 475 calories per slice.

| Step | Actual result | Pass or fail |
|------|---------------|--------------|
| 1 | The home page displayed the heading `Looking to break out of your pizza routine?`. | Pass |
| 2 | Turning on Advanced displayed Max Calories per Slice, Excluded tools, Must be vegetarian, and Custom Pizza Name. | Pass |
| 3 | The browser showed 600, Knife selected, vegetarian checked, and `Lab Pizza 20261124-205230`. | Pass |
| 4 | The recommendation kept the pizza name, used Scissors rather than Knife, and showed 475 calories per slice. | Pass |
| 5 | Clicking Love it! displayed `Please log in first.` | Pass |
| 6 | Signing in with the practice admin account displayed `Latest pizza recommendations`. | Pass |
| 7 | The shared list changed rapidly and the hand-run entry was no longer among the 15 visible rows; the same name-and-tool check passed in both automated implementations and their saved evidence. | Inconclusive by hand; automated pass |

## Part 2: Selenium WebDriver in Java

- Step 2.4, the failure line for the wrong title: `FirstTest.webFormOpens:26 expected: <Web Form> but was: <Web form>`; `Tests run: 1, Failures: 1, Errors: 0, Skipped: 0`.
- Step 2.15, QP01Test run 1, result and time: passed, `12.54 s`.
- Step 2.15, QP01Test run 2, result and time: passed, `14.89 s`.
- Step 2.20, full run 1, time and number of tests: `BUILD SUCCESS`, `48.080 s`, 5 tests, 0 failures, 0 errors.
- Step 2.20, full run 2, time and number of tests: `BUILD SUCCESS`, `40.787 s`, 5 tests, 0 failures, 0 errors.
- Page-object evidence run 1: `Lab Pizza 20261124-204745`, Scissors, 500 calories per slice.
- Page-object evidence run 2: `Lab Pizza 20261124-204904`, Pizza cutter, 375 calories per slice.

## Part 4: Cypress

- `node --version`, `npm --version` and `npx cypress version`: `v22.20.0`, `10.9.3`, and `16.2.1`.
- Step 4.3, failure message for the wrong title: `Timed out retrying after 10000ms`; actual `Web form`, expected `Web Form`; 0 passing and 1 failing.
- Step 4.6, tool and calories in the answer of `POST /api/pizza`: Pizza cutter and 350 calories per slice in run 2.
- Step 4.7, the line of the admin list that shows the pizza: `Lab Pizza 20261124-204649 (tool=Pizza cutter, ingredients_number=5) (newest)`.
- Step 4.8, run 1, result, duration and pizza name: all specs passed, 2 tests passed in `00:22`; `Lab Pizza 20261124-204532`.
- Step 4.8, run 2, result, duration and pizza name: all specs passed, 2 tests passed in `00:17`; `Lab Pizza 20261124-204649`.
- Step 4.9 was optional and was not included in the required submission.

## Part B

### 1. Why the calories box is located through its label

The page gives three number inputs the same `floating_filled` id, so that id does not uniquely identify Max Calories per Slice. The tests therefore locate the visible label and move to its preceding sibling input, which returned exactly one element in the browser. The page should give this input a unique id such as `max-calories-per-slice` and connect the label with `for="max-calories-per-slice"`. That change would let a tester use a short, stable id locator and would also improve the label-input relationship for accessibility tools.

### 2. The oracle in QP-01 step 7

The comparison between the recommended tool and the tool in the admin list is a consistency oracle. In the runs, the tool shown in the recommendation also appeared in the matching admin-list line, for example Pizza cutter in Cypress run 2. This oracle would catch a fault where QuickPizza saved or displayed a different tool in the admin list from the one returned to the customer. It would also expose stale or incorrectly mapped data even if both pages looked reasonable on their own.

### 3. A vegetarian check

QP-01 should check that the recommendation is vegetarian after the Must be vegetarian box is selected. The strongest check in this lab would assert that the `/api/pizza` response has `vegetarian: true`, because the response already contains that field. A user-visible check would be better still, but the page would need to display a vegetarian marker or list ingredient metadata that the test could evaluate. Without either a page indicator or a trusted data source, the UI test cannot prove the property merely from the ingredient names.

### 4. Comparison of Selenium WebDriver and Cypress

Selenium waited with explicit `WebDriverWait` conditions for the heading, advanced controls, recommendation, login result, and admin-list entry; its two full runs took 48.080 s and 40.787 s. Cypress automatically retried queries and assertions for up to 10 seconds and used `cy.wait('@pizza')` for the POST response; its full runs took 22 s and 17 s. QP-01 was quicker to write in Cypress because the action and assertion chains were compact, while the Java version needed more setup but became very readable after the locators moved into page objects. Cypress's command log, screenshots, and videos were the easiest evidence to follow, while Selenium's Surefire text report was precise but less visual.

### 5. Tool choice for Project 2

I would use Cypress for the automated UI tests of Project 2. First, its built-in retry behavior kept the test free of fixed pauses while still waiting reliably for QuickPizza's Svelte-rendered elements. Second, it produced screenshots and videos automatically, and both complete runs were faster than the Java suite. The custom `adminLogin` command also showed that repeated project actions can be kept in one readable place.

### 6. Part 2 - repairing a changed calories locator

If QuickPizza changes the calories-box locator, `QP01Test` must change the inline XPath that finds Max Calories per Slice. `QP01PageObjectTest` itself does not contain that locator; only the `MAX_CALORIES` constant in `HomePage` must change. The page-object test continues to call `setMaxCalories` and `maxCalories` with no knowledge of the repair. This makes the change smaller and prevents the same locator from being edited in every test that uses the page.

### 7. Part 4 - why QP-01 must remain one Cypress test

Cypress clears cookies before each test, and QuickPizza uses a cookie to keep a browser on the same server. Steps 6 and 7 must therefore remain in the same test as steps 1 to 5 so the admin page reaches the server that received the new pizza. A second test would also lose the in-memory `pizzaName` and the `@tool` alias created earlier. The admin check could then run on another server or search without the expected values, producing a failure unrelated to the feature under test.

