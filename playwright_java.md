# Complete Guide to Playwright with Java

## 1. Introduction to Playwright
### What is Playwright?
Playwright is an open-source automation framework developed by Microsoft that supports cross-browser testing across Chromium, Firefox, and WebKit. It provides features such as:
- Fast execution with WebSocket-based communication.
- Auto-waiting for elements, reducing flaky tests.
- Parallel execution with multiple browser contexts.
- Network interception and API testing capabilities.

### Why Use Playwright Over Selenium?
| Feature | Playwright | Selenium |
|---------|-----------|----------|
| Speed | Fast | Slower |
| Auto-waiting | Yes | No |
| Network Interception | Yes | No |
| Mobile Emulation | Yes | Yes |
| Parallel Execution | Easy | Complex |

## 2. Installing Playwright for Java
### Pre-requisites
- **Java 11+ (Java 21 recommended)**
- **Maven or Gradle**
- **IDE (IntelliJ IDEA, Eclipse, VS Code)**

### Installation via Maven
1. Create a **Maven project**.
2. Add the Playwright dependency in `pom.xml`:
   ```xml
   <dependencies>
       <dependency>
           <groupId>com.microsoft.playwright</groupId>
           <artifactId>playwright</artifactId>
           <version>1.41.0</version>
       </dependency>
   </dependencies>
   ```
3. Install browsers:
   ```sh
   mvn exec:java -e -Dexec.mainClass=com.microsoft.playwright.CLI -Dexec.args="install"
   ```

## 3. Playwright Basics in Java
### Creating a Simple Test
```java
import com.microsoft.playwright.*;

public class PlaywrightDemo {
    public static void main(String[] args) {
        try (Playwright playwright = Playwright.create()) {
            Browser browser = playwright.chromium().launch(new BrowserType.LaunchOptions().setHeadless(false));
            Page page = browser.newPage();
            page.navigate("https://example.com");
            System.out.println("Page Title: " + page.title());
            browser.close();
        }
    }
}
```
---

# **Playwright Class Methods**
```java
Playwright playwright = Playwright.create();
```
| Method | Description |
|--------|-------------|
| `Playwright.create()` | Creates an instance of Playwright. |
| `playwright.chromium()` | Returns a **Chromium** browser instance. |
| `playwright.firefox()` | Returns a **Firefox** browser instance. |
| `playwright.webkit()` | Returns a **WebKit** browser instance. |

---

# **Browser Methods**
```java
Browser browser = playwright.chromium().launch();
```
| Method | Description |
|--------|-------------|
| `browser.newPage()` | Creates a new **Page** (tab). |
| `browser.newContext()` | Creates a new **BrowserContext**. |
| `browser.close()` | Closes the browser. |
| `browser.isConnected()` | Checks if the browser is connected. |
| `browser.version()` | Returns the browser version. |

---

# **BrowserContext Methods**
```java
BrowserContext context = browser.newContext();
```
| Method | Description |
|--------|-------------|
| `context.newPage()` | Creates a new page inside the context. |
| `context.close()` | Closes the browser context. |
| `context.cookies()` | Retrieves cookies in the context. |
| `context.clearCookies()` | Clears all cookies. |
| `context.storageState()` | Captures storage state (useful for authentication). |

---

# **Page Methods**
```java
Page page = browser.newPage();
```
| Method | Description |
|--------|-------------|
| `page.navigate("https://example.com")` | Opens a URL. |
| `page.reload()` | Reloads the current page. |
| `page.goBack()` | Goes back to the previous page. |
| `page.goForward()` | Moves forward in history. |
| `page.title()` | Returns the page title. |
| `page.url()` | Returns the current page URL. |
| `page.screenshot(new Page.ScreenshotOptions().setPath(Paths.get("screenshot.png")))` | Takes a screenshot. |
| `page.pdf(new Page.PdfOptions().setPath(Paths.get("output.pdf")))` | Saves page as a PDF. |
| `page.evaluate("document.title")` | Executes JavaScript inside the page. |
| `page.close()` | Closes the current tab. |

---

# **Locator Methods (Element Handling)**
```java
Locator element = page.locator("#username");
```
| Method | Description |
|--------|-------------|
| `locator.click()` | Clicks on the element. |
| `locator.fill("text")` | Fills an input field. |
| `locator.type("text")` | Types text into the field (character by character). |
| `locator.textContent()` | Gets the text inside an element. |
| `locator.isVisible()` | Checks if an element is visible. |
| `locator.isEnabled()` | Checks if an element is enabled. |
| `locator.isChecked()` | Checks if a checkbox/radio is checked. |
| `locator.hover()` | Hovers over an element. |
| `locator.press("Enter")` | Presses a keyboard key. |
| `locator.selectOption("value")` | Selects an option from a dropdown. |
| `locator.setInputFiles(Paths.get("file.txt"))` | Uploads a file. |

---

# **Frame Methods (Handling iFrames)**
```java
Frame frame = page.frameLocator("#iframe").first();
```
| Method | Description |
|--------|-------------|
| `frame.locator("selector")` | Finds an element inside the frame. |
| `frame.evaluate("document.title")` | Runs JavaScript inside the frame. |
| `frame.waitForSelector("selector")` | Waits for an element to appear. |

---

# **Keyboard Methods**
```java
page.keyboard().press("Enter");
```
| Method | Description |
|--------|-------------|
| `keyboard.press("Enter")` | Presses a key. |
| `keyboard.type("Hello")` | Types text. |
| `keyboard.down("Shift")` | Holds down a key. |
| `keyboard.up("Shift")` | Releases a key. |

---

# **Mouse Methods**
```java
page.mouse().click(100, 200);
```
| Method | Description |
|--------|-------------|
| `mouse.click(x, y)` | Clicks at given coordinates. |
| `mouse.move(x, y)` | Moves the mouse to given coordinates. |
| `mouse.down()` | Presses the mouse button down. |
| `mouse.up()` | Releases the mouse button. |

---

# **Network Interception & API Handling**
```java
page.route("**/*", route -> route.abort());
```
| Method | Description |
|--------|-------------|
| `page.route("url", handler)` | Intercepts network requests. |
| `page.unroute("url")` | Removes request interception. |
| `page.waitForResponse("url")` | Waits for a response from a specific URL. |

---

# **Screenshot & Recording Methods**
```java
page.screenshot(new Page.ScreenshotOptions().setPath(Paths.get("screenshot.png")));
```
| Method | Description |
|--------|-------------|
| `page.screenshot()` | Takes a screenshot. |
| `context.tracing().start()` | Starts tracing. |
| `context.tracing().stop()` | Stops tracing. |
| `context.tracing().export(Paths.get("trace.zip"))` | Exports tracing data. |

---

# **Assertions (Using Playwright Test)**
```java
Assertions.assertThat(page.locator("#element")).isVisible();
```
| Method | Description |
|--------|-------------|
| `assertThat(locator).isVisible()` | Asserts visibility. |
| `assertThat(locator).isEnabled()` | Asserts enabled state. |
| `assertThat(locator).containsText("Hello")` | Asserts text content. |

---

# **Event Listeners**
```java
page.onConsoleMessage(msg -> System.out.println(msg.text()));
```
| Method | Description |
|--------|-------------|
| `page.onConsoleMessage()` | Listens to console logs. |
| `page.onDialog(dialog -> dialog.accept())` | Handles alerts/dialogs. |
| `page.onRequest(req -> System.out.println(req.url()))` | Listens to network requests. |

---

# **Test Execution & Parallelization**
### **Using JUnit 5**
```java
@ParameterizedTest
@ValueSource(strings = {"https://google.com", "https://example.com"})
void testMultipleUrls(String url) {
    page.navigate(url);
    Assertions.assertTrue(page.title().length() > 0);
}
```
| Method | Description |
|--------|-------------|
| `@Test` | Marks a test case. |
| `@BeforeAll` | Runs once before all tests. |
| `@AfterAll` | Runs after all tests. |
| `@BeforeEach` | Runs before each test. |
| `@AfterEach` | Runs after each test. |

---

# **Running Tests in Headless or Headed Mode**
```java
Browser browser = playwright.chromium().launch(new BrowserType.LaunchOptions().setHeadless(true));
```
| Option | Description |
|--------|-------------|
| `setHeadless(true)` | Runs in headless mode (no UI). |
| `setHeadless(false)` | Runs in headed mode (with UI). |

---

# **Playwright vs Selenium Comparison**
| Feature | Playwright | Selenium |
|---------|-----------|----------|
| Speed | Fast | Slower |
| Auto-waiting | Yes | No |
| Network Interception | Yes | No |
| Mobile Emulation | Yes | Yes |
| Parallel Execution | Easy | Complex |

---
## 5. Handling Alerts, Frames, and Keyboard Actions

### Handling Alerts
```java
page.onDialog(dialog -> {
    System.out.println(dialog.message());
    dialog.accept();
});
```

### Handling iFrames
```java
Frame frame = page.frameLocator("#iframe").first();
frame.locator("button").click();
```

### Keyboard Actions
```java
page.keyboard().press("Enter");
```

## 6. Assertions in Playwright (JUnit 5 & TestNG)
### Using JUnit 5
```java
import org.junit.jupiter.api.*;

public class PlaywrightJUnitTest {
    @Test
    void testExample() {
        page.navigate("https://example.com");
        Assertions.assertEquals("Example Domain", page.title());
    }
}
```

### Using TestNG
```java
import org.testng.Assert;
import org.testng.annotations.*;

public class PlaywrightTestNGTest {
    @Test
    public void testExample() {
        page.navigate("https://example.com");
        Assert.assertEquals(page.title(), "Example Domain");
    }
}
```

## 7. Parallel Execution & CI/CD Integration
### Running Tests in Parallel
```java
BrowserContext context1 = browser.newContext();
BrowserContext context2 = browser.newContext();
```

## 8. Advanced Playwright Features
### API Testing with Playwright
```java
APIRequestContext request = playwright.request();
APIResponse response = request.get("https://api.example.com");
System.out.println(response.text());
```

### Network Interception
```java
page.route("**/*", route -> route.abort());
```

## 9. Playwright vs Selenium
| Feature | Playwright | Selenium |
|---------|-----------|----------|
| Speed | Fast | Slower |
| Auto-waiting | Yes | No |
| Network Mocking | Yes | No |
| Parallel Testing | Easy | Hard |

## 10. Conclusion
Playwright is a powerful alternative to Selenium for UI automation in Java. It provides features such as:
- **Fast execution and auto-waiting**
- **Cross-browser and mobile testing**
- **Easy integration with JUnit, TestNG, and CI/CD**
---
