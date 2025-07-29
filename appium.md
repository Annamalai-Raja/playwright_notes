# 📱 Appium Java Cheatsheet (Zero to Hero)

---

## 📦 1. Setup

### Install Requirements

- Java (JDK 8+)
- Android Studio / Xcode
- Node.js & npm
- Appium Server

```bash
npm install -g appium
appium doctor
````

### Install Appium Inspector (GUI)

Download: [https://github.com/appium/appium-inspector/releases](https://github.com/appium/appium-inspector/releases)

---

## ⚙️ 2. Create Maven Project with Dependencies

### `pom.xml`

```xml
<dependencies>
  <dependency>
    <groupId>io.appium</groupId>
    <artifactId>java-client</artifactId>
    <version>9.0.0</version>
  </dependency>
  <dependency>
    <groupId>org.testng</groupId>
    <artifactId>testng</artifactId>
    <version>7.9.0</version>
    <scope>test</scope>
  </dependency>
</dependencies>
```

---

## 🔧 3. Desired Capabilities

### Android Example

```java
DesiredCapabilities caps = new DesiredCapabilities();
caps.setCapability("platformName", "Android");
caps.setCapability("deviceName", "emulator-5554");
caps.setCapability("automationName", "UiAutomator2");
caps.setCapability("app", "/path/to/app.apk");

AndroidDriver driver = new AndroidDriver(new URL("http://localhost:4723/wd/hub"), caps);
```

### iOS Example

```java
caps.setCapability("platformName", "iOS");
caps.setCapability("deviceName", "iPhone 14");
caps.setCapability("automationName", "XCUITest");
```

---

## 🧪 4. Basic Actions

```java
driver.findElement(By.id("com.app:id/login")).click();
driver.findElement(By.xpath("//android.widget.EditText")).sendKeys("text");
driver.findElement(MobileBy.AccessibilityId("Continue")).click();
```

---

## 📍 5. Locator Strategies

* `By.id("id")`
* `By.className("android.widget.Button")`
* `By.xpath("//hierarchy/...")`
* `MobileBy.AccessibilityId("value")`
* `MobileBy.AndroidUIAutomator("new UiSelector().text(\"Text\")")`

---

## ✍️ 6. Writing Test Cases (TestNG)

```java
public class LoginTest {
  AppiumDriver driver;

  @BeforeClass
  public void setup() { /* init code */ }

  @Test
  public void loginTest() {
    driver.findElement(By.id("username")).sendKeys("admin");
    driver.findElement(By.id("password")).sendKeys("1234");
    driver.findElement(By.id("loginBtn")).click();
  }

  @AfterClass
  public void tearDown() {
    driver.quit();
  }
}
```

---

## 🔁 7. Waits

```java
driver.manage().timeouts().implicitlyWait(10, TimeUnit.SECONDS);

WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
wait.until(ExpectedConditions.visibilityOfElementLocated(By.id("login")));
```

---

## 🔄 8. Scrolling

```java
driver.findElement(MobileBy.AndroidUIAutomator(
  "new UiScrollable(new UiSelector()).scrollIntoView(text(\"Target Text\"));"
));
```

---

## 🧭 9. Swipe & Tap

```java
TouchAction action = new TouchAction(driver);
action.press(PointOption.point(500, 1200))
      .waitAction(WaitOptions.waitOptions(Duration.ofSeconds(1)))
      .moveTo(PointOption.point(500, 300))
      .release()
      .perform();
```

---

## 📷 10. Screenshots

```java
File src = driver.getScreenshotAs(OutputType.FILE);
FileUtils.copyFile(src, new File("screenshot.png"));
```

---

## 🔌 11. App Management

```java
driver.installApp("/path/to/app.apk");
driver.removeApp("com.app.package");
System.out.println(driver.isAppInstalled("com.app.package"));
```

---

## 🧾 12. App Package & Activity

Find via `adb`:

```bash
adb shell dumpsys window | grep -E 'mCurrentFocus'
```

OR use Appium Inspector to locate:

* `appPackage`
* `appActivity`

---

## 🔐 13. Unlocking Device / Permissions

```java
driver.unlockDevice();
driver.openNotifications();
driver.toggleWifi();
```

---

## 📲 14. Native + Web (Hybrid Apps)

Switch to WebView:

```java
Set<String> contexts = driver.getContextHandles();
for (String context : contexts) {
  if (context.contains("WEBVIEW")) {
    driver.context(context);
  }
}
```

Switch back:

```java
driver.context("NATIVE_APP");
```

---

## 🌐 15. Parallel Execution (TestNG + Devices)

Use separate XML `<test>` blocks per device. Example:

```xml
<suite name="Appium Suite" parallel="tests">
  <test name="Pixel4 Test">...</test>
  <test name="iPhone14 Test">...</test>
</suite>
```

---

## 🧠 16. Page Object Model (POM)

```java
public class LoginPage {
  AppiumDriver driver;

  @AndroidFindBy(id = "username") MobileElement username;
  @AndroidFindBy(id = "password") MobileElement password;
  @AndroidFindBy(id = "login") MobileElement loginBtn;

  public LoginPage(AppiumDriver driver) {
    PageFactory.initElements(new AppiumFieldDecorator(driver), this);
  }

  public void login(String user, String pass) {
    username.sendKeys(user);
    password.sendKeys(pass);
    loginBtn.click();
  }
}
```

---

## 🧪 17. CI/CD Integration

Use command line to run tests:

```bash
mvn clean test
```

For Jenkins/GitHub Actions:

* Start Appium with `appium &`
* Connect devices/emulators
* Run Maven project

---

## 🧰 18. Appium Server Commands

```bash
appium --port 4723
appium --log-level error
```

---

## 🧪 19. Useful Tools

* **Appium Inspector** – view elements, get locators
* **UIAutomatorViewer** – inspect Android elements
* **adb** – Android debug bridge
* **Genymotion / Android Emulator** – virtual devices

---

## 🧠 20. Tips & Best Practices

✅ Use Page Object Model
✅ Avoid hard waits (`Thread.sleep`)
✅ Prefer accessibility ids or `resource-id`
✅ Run on real devices for accurate results
✅ Log failures with screenshots

---

## 🔗 21. Official Resources

* [Appium Official Docs](https://appium.io/docs/en/about-appium/intro/)
* [Java Client GitHub](https://github.com/appium/java-client)
* [Appium Inspector](https://github.com/appium/appium-inspector)
* [Appium Pro Tutorials](https://appiumpro.com/)

---

## 🔄 22. Data-Driven Testing (TestNG + Excel)

### Example: Apache POI

```java
FileInputStream fis = new FileInputStream("data.xlsx");
XSSFWorkbook wb = new XSSFWorkbook(fis);
XSSFSheet sheet = wb.getSheetAt(0);
String username = sheet.getRow(1).getCell(0).getStringCellValue();
````

### With @DataProvider:

```java
@DataProvider(name = "loginData")
public Object[][] data() {
  return new Object[][] {
    {"user1", "pass1"},
    {"user2", "pass2"}
  };
}

@Test(dataProvider = "loginData")
public void testLogin(String username, String password) {
  // use credentials in test
}
```

---

## 🛠 23. Logging & Reporting

### Logging with Log4j

```java
Logger log = Logger.getLogger("AppiumLog");
PropertyConfigurator.configure("log4j.properties");
log.info("App Started");
```

### Reporting with ExtentReports

```java
ExtentReports report = new ExtentReports("report.html", true);
ExtentTest test = report.startTest("Login Test");
test.log(LogStatus.PASS, "Login successful");
```

---

## 🔧 24. Appium Advanced Capabilities

```java
caps.setCapability("autoGrantPermissions", true);
caps.setCapability("noReset", true);
caps.setCapability("fullReset", false);
caps.setCapability("newCommandTimeout", 60);
```

---

## 🧠 25. Debugging Tips

* Use `driver.getPageSource()` to troubleshoot element visibility
* Use `adb logcat` to view real-time logs
* Use `.wait()` with expected conditions, not `Thread.sleep()`

---

## 🚀 26. Real Device Testing (Cloud)

### Tools:

* **BrowserStack**
* **Sauce Labs**
* **TestingBot**

### BrowserStack Capabilities

```java
caps.setCapability("browserstack.user", "your_user");
caps.setCapability("browserstack.key", "your_key");
caps.setCapability("device", "Google Pixel 6");
caps.setCapability("app", "bs://<app-id>");
```

---

## 🧪 27. API + Mobile Automation Combo (Hybrid Framework)

Use **RestAssured** for backend + Appium for frontend in same project:

```java
Response res = given().when().get("/api/data");
String apiValue = res.jsonPath().get("key");
driver.findElement(By.id("input")).sendKeys(apiValue);
```

---

## 🔁 28. CI/CD with Appium (Sample Jenkins Pipeline)

```groovy
pipeline {
  agent any
  stages {
    stage('Install') {
      steps {
        sh 'npm install -g appium'
        sh 'appium --log-level error &'
      }
    }
    stage('Test') {
      steps {
        sh 'mvn clean test'
      }
    }
  }
}
```

---

## 🔍 29. Mobile Gestures

```java
new TouchAction(driver)
  .longPress(PointOption.point(300, 500))
  .waitAction(WaitOptions.waitOptions(Duration.ofSeconds(2)))
  .release()
  .perform();
```

---

## 🧑‍🏫 30. Interview-Ready Concepts

* Native vs Hybrid vs Web Apps
* Difference between `driver.quit()` and `driver.closeApp()`
* PageFactory vs PageObject
* Why UiAutomator2 is preferred over Appium 1's default
* Command timeout, session override, implicit vs explicit waits

---

## 🎓 31. Learning Projects Ideas

* Automate Flipkart / Amazon app login & cart
* Build a test suite for your company’s Android app
* Create a keyword-driven test engine (Excel based)
* Create a Jenkins CI pipeline for mobile tests
* Run same suite across emulator + real device

---

```

---

Would you like all 31 sections compiled into a `.md` file now?

✅ Just say "**generate file**" and I’ll export it immediately for download.
```
