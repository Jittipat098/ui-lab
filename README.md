# UI Lab - Automated UI Testing

This repository contains the required Part 0 work and two selected tool parts:

- **Part 2 - Selenium WebDriver with Java:** `selenium-java/` contains JUnit tests for Selenium's web form, explicit waits, QP-01, page objects, screenshots, and the saved Surefire report.
- **Part 4 - Cypress:** `cypress/` contains JavaScript specs for Selenium's web form and QP-01, a custom admin-login command, screenshots, videos, and captured run data.

`RESULTS.md` records the Part 0 browser work, both full runs for each selected tool, the deliberate failure messages, and all seven reflection answers. Evidence from both runs is preserved under each tool's `evidence/run1` and `evidence/run2` folders.

## Run the tests

### Selenium WebDriver with Java

Requirements: Java 21 or newer, Maven, Google Chrome, and a matching ChromeDriver. Set `CHROMEDRIVER` to the full path of `chromedriver.exe`, then run:

```powershell
cd selenium-java
mvn test -Dheadless=true
```

### Cypress

Requirements: Node.js 22 or newer and Google Chrome. Install the locked dependencies, then run:

```powershell
cd cypress
npm install
npx cypress run --browser chrome
```

The tests use the shared QuickPizza practice site only once per requested run and never clear shared ratings.

