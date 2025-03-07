# Incubyte Automation Assessment Task

# Feature File

Feature: User Registration and Login

  Scenario: Register a new user
    Given User is on the home page
    When User navigates to the Registration page
    And User enters valid registration details
    And User submits the registration form
    Then User should see the account creation confirmation page

  Scenario: User Login
    Given User is on the login page
    When User enters valid credentials
    And User submits the login form
    Then User should be logged in successfully

   # Pages

   1. Home Page

      package pages;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.FindBy;
import org.openqa.selenium.support.PageFactory;

public class HomePage {
    WebDriver driver;

    @FindBy(xpath = "(//a[contains(@href, 'customer/account/create')])[1]")
    WebElement createAccountLink;

    @FindBy(xpath = "(//a[contains(@href, 'customer/account/login')])[1]")
    WebElement signInLink;

    public HomePage(WebDriver driver) {
        this.driver = driver;
        PageFactory.initElements(driver, this);
    }

    public void clickCreateAccount() {
        createAccountLink.click();
    }

    public void clickSignIn() {
        signInLink.click();
    }
}

2. Register Page

   package pages;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.FindBy;
import org.openqa.selenium.support.PageFactory;

public class RegisterPage {
    WebDriver driver;

    @FindBy(id = "firstname")
    WebElement firstName;

    @FindBy(id = "lastname")
    WebElement lastName;

    @FindBy(id = "email_address")
    WebElement email;

    @FindBy(id = "password")
    WebElement password;

    @FindBy(id = "password-confirmation")
    WebElement confirmPassword;

    @FindBy(xpath = "(//span[text()='Create an Account'])[1]")
    WebElement createAccountButton;

    public RegisterPage(WebDriver driver) {
        this.driver = driver;
        PageFactory.initElements(driver, this);
    }

    public void enterFirstName(String fname) {
        firstName.sendKeys(fname);
    }

    public void enterLastName(String lname) {
        lastName.sendKeys(lname);
    }

    public void enterEmail(String emailText) {
        email.sendKeys(emailText);
    }

    public void enterPassword(String pwd) {
        password.sendKeys(pwd);
        confirmPassword.sendKeys(pwd);
    }

    public void clickCreateAccount() {
        createAccountButton.click();
    }
}
    
3. Login Page

   package pages;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.FindBy;
import org.openqa.selenium.support.PageFactory;

public class LoginPage {
    WebDriver driver;

    @FindBy(id = "email")
    WebElement email;

    @FindBy(id = "pass")
    WebElement password;

    @FindBy(id = "send2")
    WebElement signInButton;

    public LoginPage(WebDriver driver) {
        this.driver = driver;
        PageFactory.initElements(driver, this);
    }

    public void enterEmail(String emailText) {
        email.sendKeys(emailText);
    }

    public void enterPassword(String pwd) {
        password.sendKeys(pwd);
    }

    public void clickSignIn() {
        signInButton.click();
    }
}

# Utils (Base Class)

package utils;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import java.util.concurrent.TimeUnit;

public class BaseClass {
    public static WebDriver driver;

    public static void initializeDriver() {
        System.setProperty("webdriver.chrome.driver", "C:\\Users\\thala\\Downloads\\chromedriver-win64 (2)\\chromedriver-win64\\chromedriver.exe");
        driver = new ChromeDriver();
        driver.manage().timeouts().implicitlyWait(10, TimeUnit.SECONDS);
        driver.manage().window().maximize();
    }

    public static void tearDown() {
        if (driver != null) {
            driver.quit();
        }
    }
}

