# Incubyte Automation Assessment Task (BDD + POM)

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

# Step Definitions

package stepDefinitions;

import io.cucumber.java.en.*;
import org.openqa.selenium.WebDriver;
import pages.HomePage;
import pages.LoginPage;
import pages.RegisterPage;
import utils.BaseClass;

public class RegisterSteps {
    WebDriver driver;
    HomePage homePage;
    RegisterPage registerPage;
    LoginPage loginPage;

    @Given("User is on the home page")
    public void user_is_on_home_page() {
        BaseClass.initializeDriver();
        driver = BaseClass.driver;
        driver.get("https://magento.softwaretestingboard.com/");
        homePage = new HomePage(driver);
    }

    @When("User navigates to the Registration page")
    public void user_navigates_to_registration_page() {
        homePage.clickCreateAccount();
        registerPage = new RegisterPage(driver);
    }

    @When("User enters valid registration details")
    public void user_enters_registration_details() {
        registerPage.enterFirstName("Sushmitha");
        registerPage.enterLastName("M K");
        registerPage.enterEmail("sushmithamk9295@gmail.com");
        registerPage.enterPassword("Jull@2612");
    }

    @When("User submits the registration form")
    public void user_submits_registration_form() {
        registerPage.clickCreateAccount();
    }

    @Then("User should see the account creation confirmation page")
    public void user_sees_confirmation_page() {
        System.out.println("Account Created Successfully!!!");
        BaseClass.tearDown();
    }

    @Given("User is on the login page")
    public void user_is_on_login_page() {
        BaseClass.initializeDriver();
        driver = BaseClass.driver;
        driver.get("https://magento.softwaretestingboard.com/customer/account/login/");
        loginPage = new LoginPage(driver);
    }

    @When("User enters valid credentials")
    public void user_enters_valid_credentials() {
        loginPage.enterEmail("sushmithamk9295@gmail.com");
        loginPage.enterPassword("Jull@2612");
    }

    @When("User submits the login form")
    public void user_submits_login_form() {
        loginPage.clickSignIn();
    }

    @Then("User should be logged in successfully")
    public void user_logged_in_successfully() {
        System.out.println("Account Signed In Successfully!!!");
        BaseClass.tearDown();
    }
}

# Runner Class

package runner;

import io.cucumber.junit.Cucumber;
import io.cucumber.junit.CucumberOptions;
import org.junit.runner.RunWith;

@RunWith(Cucumber.class)
@CucumberOptions(
        features = "src/test/java/features",
        glue = "stepDefinitions",
        plugin = {
        		"pretty", 
        		"html:target/cucumber-reports.html", 
        		"json:target/cucumber-reports/cucumber.json"
        		},
        monochrome = true
)
public class TestRunner {
}
