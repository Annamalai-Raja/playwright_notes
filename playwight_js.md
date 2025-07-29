# 🎭 Playwright JavaScript Cheatsheet (Zero to Hero)

---

## 📦 1. Setup

```bash
npm init -y
npm i -D @playwright/test
npx playwright install
````

---

## 🧱 2. Basic Test Structure

```js
const { test, expect } = require('@playwright/test');

test('homepage loads', async ({ page }) => {
  await page.goto('https://example.com');
  await expect(page).toHaveTitle(/Example/);
});
```

```bash
npx playwright test
```

---

## 🧭 3. Navigation + Locators

```js
await page.goto('https://example.com');
await page.click('text=Login');
await page.fill('#username', 'admin');
await page.fill('#password', '12345');
await page.click('button[type="submit"]');
```

**Locator examples:**

```js
page.locator('text=Submit')
page.locator('[data-testid="submit"]')
page.locator('input[name="email"]')
```

---

## ✅ 4. Assertions

```js
await expect(page).toHaveURL(/dashboard/);
await expect(page.locator('h1')).toHaveText('Welcome');
await expect(page.locator('.alert')).toBeVisible();
```

---

## 📸 5. Screenshots & Tracing

```js
await page.screenshot({ path: 'screenshot.png' });

await page.context().tracing.start({ screenshots: true, snapshots: true });
// ... run test steps
await page.context().tracing.stop({ path: 'trace.zip' });
```

---

## 📂 6. Page Object Model (POM)

```js
// pages/LoginPage.js
class LoginPage {
  constructor(page) {
    this.page = page;
    this.username = page.locator('#username');
    this.password = page.locator('#password');
    this.loginBtn = page.locator('button[type="submit"]');
  }

  async login(user, pass) {
    await this.username.fill(user);
    await this.password.fill(pass);
    await this.loginBtn.click();
  }
}
module.exports = { LoginPage };
```

```js
// tests/login.spec.js
const { LoginPage } = require('../pages/LoginPage');

test('user can login', async ({ page }) => {
  const login = new LoginPage(page);
  await page.goto('https://app.com/login');
  await login.login('admin', 'adminpass');
});
```

---

## ⏳ 7. Waits & Timeouts

```js
await page.waitForSelector('.loader', { state: 'hidden' });
await page.waitForTimeout(2000); // use only if needed
```

> Playwright auto-waits for most actions.

---

## ⚙️ 8. Config & Hooks

### `playwright.config.js`

```js
module.exports = {
  testDir: './tests',
  timeout: 30 * 1000,
  use: {
    headless: true,
    baseURL: 'https://app.com',
    screenshot: 'only-on-failure',
    trace: 'retain-on-failure',
  },
};
```

### Hooks

```js
test.beforeEach(async ({ page }) => {
  await page.goto('/');
});
```

---

## 🚀 9. Parallel Testing

```bash
npx playwright test --workers=4
```

---

## 🔁 10. Data-Driven Tests

```js
const users = ['admin', 'guest'];

for (const user of users) {
  test(`Login with ${user}`, async ({ page }) => {
    // login logic
  });
}
```

---

## 🧪 11. API Testing

```js
const response = await request.post('/api/login', {
  data: { username: 'admin', password: '1234' },
});
expect(response.status()).toBe(200);
```

---

## 🧱 12. Storage State & Auth Reuse

```bash
# Save auth state
npx playwright codegen --save-storage=auth.json
```

```js
test.use({ storageState: 'auth.json' });
```

---

## 🧪 13. CI/CD (GitHub Actions)

```yaml
# .github/workflows/playwright.yml
name: Playwright Tests

on: [push]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
      - run: npm ci
      - run: npx playwright install --with-deps
      - run: npx playwright test
```

---

## 🛠 14. Code Generation

```bash
npx playwright codegen https://example.com
```

---

## 🐞 15. Debugging & Isolation

```bash
PWDEBUG=1 npx playwright test
```

```js
test.only('debug this test', async ({ page }) => {
  // debug this test only
});
```

---

## ✅ 16. Best Practices

* Use **Page Object Model**
* Avoid hard waits; use auto-waits
* Use `test.step()` for grouping steps
* Use `tracing`, `screenshot`, and `video` for flaky tests
* Prefer `data-testid` or role-based selectors

---

## 📚 17. Official Resources

* [Playwright Docs](https://playwright.dev)
* [Playwright GitHub](https://github.com/microsoft/playwright)
* [Playwright Test Reporter](https://playwright.dev/docs/test-reporters)
* [Playwright Test Generator](https://playwright.dev/docs/codegen)

---

Great — here's the **continuation** of anything missing from the original cheatsheet, including a few **advanced features**, **reporting**, **component testing**, and **VS Code tips**.

---

````markdown
---

## 📊 18. HTML Reporting

```bash
# Run with built-in HTML reporter
npx playwright test --reporter=html

# View the report
npx playwright show-report
````

Custom reporter setup in `playwright.config.js`:

```js
reporter: [['html', { outputFolder: 'playwright-report', open: 'never' }]],
```

---

## 🧩 19. Component Testing (Experimental)

For React, Vue, or other frameworks:

```bash
npm install -D @playwright/experimental-ct-react
```

```js
// Example: React component test
import { test, expect } from '@playwright/experimental-ct-react';
import Button from './Button';

test('button renders correctly', async ({ mount }) => {
  const component = await mount(<Button label="Click me" />);
  await expect(component).toContainText('Click me');
});
```

Docs: [https://playwright.dev/docs/component-testing](https://playwright.dev/docs/component-testing)

---

## 🧠 20. Advanced Test Annotations

```js
test.skip('not implemented yet', async () => {});
test.fixme('known bug, to fix later');
test.only('run only this test');
test.describe('Admin Tests', () => { ... });
```

You can conditionally skip tests:

```js
test.skip(browserName === 'firefox', 'Not supported in Firefox');
```

---

## 🧪 21. Fixtures & Custom Hooks

```js
// test-fixtures.js
const base = require('@playwright/test');

exports.test = base.test.extend({
  adminPage: async ({ page }, use) => {
    await page.goto('/admin');
    await use(page);
  },
});
```

```js
// admin.spec.js
const { test } = require('../test-fixtures');

test('dashboard loads', async ({ adminPage }) => {
  // use adminPage directly
});
```

---

## 🧩 22. Plugin Ecosystem & Extras

* **playwright/test-runner** – rich CLI & parallelism
* **playwright/test-reporters** – Allure, HTML, JSON, JUnit
* **@playwright/experimental-ct-**\* – for component testing

---

## 💡 23. Useful VS Code Extensions

* [Playwright Test for VSCode](https://marketplace.visualstudio.com/items?itemName=ms-playwright.playwright)
* Features:

  * Test runner UI
  * Auto-detection of tests
  * Debug with breakpoints

---

## 🔐 24. Auth Flow & Session Management

Use storage snapshots to avoid repeated login:

```js
// Save state manually
await page.context().storageState({ path: 'auth.json' });

// Reuse
test.use({ storageState: 'auth.json' });
```

---

## 🧭 25. Test Generator UI

```bash
npx playwright codegen
```

This opens an interactive browser to record actions and auto-generate Playwright tests in JS, TS, Python, or C#.

---

## 🧼 26. Cleanup & Isolation Tips

* Use `test.use()` for test-specific settings
* Use `test.describe.serial()` to run tests in sequence
* Always cleanup data (DB, cookies) if tests mutate state

---

## 🌐 27. Cross-Browser Testing

Run tests in Chromium, Firefox, and WebKit:

```bash
npx playwright test --project=firefox
```

In config:

```js
projects: [
  { name: 'chromium', use: { browserName: 'chromium' } },
  { name: 'firefox', use: { browserName: 'firefox' } },
  { name: 'webkit', use: { browserName: 'webkit' } },
],
```

---

## 🗂 28. Organizing Tests

```
/tests
  ├── login.spec.js
  ├── dashboard.spec.js
/pages
  ├── LoginPage.js
  ├── DashboardPage.js
/playwright.config.js
```

Keep tests isolated and readable. Use `test.describe` to group suites.

---

## 📦 29. Mocking APIs & Network Intercepts

```js
await page.route('**/api/data', route =>
  route.fulfill({
    status: 200,
    body: JSON.stringify({ message: 'Mocked!' }),
  })
);
```

---

## 📚 30. Recommended Learning Resources

* [Playwright Official Docs](https://playwright.dev)
* [Microsoft Learn Module](https://learn.microsoft.com/en-us/training/modules/build-with-playwright/)
* [Automation Bro YouTube](https://www.youtube.com/c/AutomationBro)
* [Playwright GitHub Repo](https://github.com/microsoft/playwright)
* [TestingBestPractices](https://github.com/jorgebucaran/testing-best-practices)

---

## 🔁 31. Update Playwright

```bash
npx playwright install --force
```

---

## 📎 Final Tips

✅ Use `data-testid` for selectors
✅ Debug tests with `PWDEBUG=1`
✅ Integrate tests early in CI/CD
✅ Write short, atomic, and self-contained tests
✅ Leverage `tracing` + `reporters` to debug flakes

---

Happy testing!!
