# TestRepos
Testing is a very important aspect of development and can largely determine the fate of an application. Good testing can catch application-killing issues early on, but poor testing invariably leads to failure and downtime.

While there are three main types of software testing: unit testing, functional testing, and integration testing, in this blog post, I am going to talk about developer-level unit testing. Before I dive into the specifics, let’s review – at a high level – what each type of testing entails.

Types of Software Development Tests
Unit tests are used to test individual code components and ensure that code works the way it was intended to. Unit tests are written and executed by developers. Most of the time a testing framework like JUnit or TestNG is used. Test cases are typically written at a method level and executed via automation.

Integration Tests check if the system as a whole works. Integration testing is also done by developers, but rather than testing individual components, it aims to test across components. A system consists of many separate components like code, database, web servers, etc. Integration tests are able to spot issues like wiring of components, network access, database issues, etc.

Functional tests check that each feature is implemented correctly by comparing the results for a given input against the specification. Typically, this is not done at a developer level. Functional tests are executed by a separate testing team. Test cases are written based on the specification and the actual results are compared with the expected results. Several tools are available for automated functional testing like Selenium and QTP.

As mentioned earlier, unit testing helps developers to determine whether the code works correctly. In this blog post, I will provide helpful tips for unit testing in Java.

Check out this blog post to learn more about the testing tools our development team uses and loves!

1. Use a framework for unit testing
Java provides several frameworks that for unit testing. TestNG and JUnit are the most popular testing frameworks. Some important features of JUnit and TestNG:

Easy to setup and run
Supports annotations
Allows certain tests to be ignored or grouped and executed together
Supports parameterized testing, i.e. running a unit test by specifying different values at run time
Supports automated test execution by integrating with build tools like Ant, Maven, and Gradle
EasyMock is a mocking framework that is complementary to a unit testing framework like JUnit and TestNG. EasyMock is not a full-fledged framework by itself. It simply adds the ability to create mock objects to facilitate testing. For example, a method we want to test may invoke a DAO class that gets data from the database. In this case, EasyMock can be used to create a MockDAO that returns hard-coded data. This allows us to easily test the method that we intend to without having to bother about the database access.

2. Use Test Driven Development – Judiciously!
Test-driven development (TDD) is a software development process in which tests are written based on the requirements before any coding begins. Since there is no code yet, the test will initially fail. The minimum amount of code is then written to pass the test. The code is then refactored until it is optimized.

The goal is to write tests that cover all the requirements as against simply writing code first that may not even meet the requirements. TDD is great as it leads to simple modular code that is easy to maintain. Overall development speeds up and defects are easily identified. Also, unit tests get created as a by-product of the TDD approach.

However, TDD may not be suitable in all situations. In projects where the design is complicated, focusing on the simplest design to pass the test cases and not thinking ahead can result in huge code changes. Also the TDD approach is difficult to use for systems which interact with legacy systems, GUI applications or applications that work with databases. Also, the tests need to be updated as the code changes.

So before deciding on TDD approach, the above factors should be kept in mind and a call should be taken based on the nature of the project.

3. Measure code coverage
Code coverage measures (in percentage) how much of the code is executed when the unit tests are run. Normally, code with high coverage has a decreased chance of containing undetected bugs, as more of its source code has been executed in the course of testing. Some best practices for measuring code coverage include:

Use a code coverage tool like Clover, Corbetura, JaCoCo, or Sonar. Using a tool can improve testing quality, as these tools can point out areas of the code that are untested, allowing you to develop additional tests to cover these areas.
Whenever new functionality is written, immediately write new tests to cover.
Ensure that there are test cases that cover all the branches of the code, i.e. if/else statements.
High code coverage does not guarantee the tests are perfect, so beware!

The concat method below accepts a boolean value as input, and appends the two strings passed in only if the boolean value is true:


     public String concat(boolean append, String a,String b) {

        String result = null;
        If (append) {
            result = a + b;
                            }
        return result.toLowerCase();

    }
     public String concat(boolean append, String a,String b) {
 
        String result = null;
        If (append) {
            result = a + b;
                            }
        return result.toLowerCase();
 
    }
The following is a test case for the above method:


         @Test
        public void testStringUtil() {
         String result = stringUtil.concat(true, "Hello ", "World");
         System.out.println("Result is "+result);

        }
         @Test
        public void testStringUtil() {
         String result = stringUtil.concat(true, "Hello ", "World");
         System.out.println("Result is "+result);
 
        }
In this case, the test is executed with a value of true. When the test is executed, it will pass. When a code coverage tool is run, it will show 100% code coverage as all the code in the concat method is executed. However, if the test is executed with a value of false, a NullPointerException will be thrown. So 100% code coverage is not really an indication of whether the test has covered all the scenarios and the test is good.

4. Externalize test data wherever possible
Prior to JUnit4, the data for which the test case was to be run has to be hardcoded into the test case. This created a restriction that in order to run the test with different data, the test case code had to be modified. However, JUnit4 as well as TestNG support externalizing the test data so that the test cases can be run for different datasets without having to change the source code.

The MathChecker class below has a method which checks if a number is odd:


    public class MathChecker {

        public Boolean isOdd(int n) {

            if (n%2 != 0) {
                return true;
            } else {
                return false;
                                         }
        }
    }


    public class MathChecker {
 
        public Boolean isOdd(int n) {
 
            if (n%2 != 0) {
                return true;
            } else {
                return false;
                                         }
        }
    }
The following is a TestNG test case for the MathChecker class:


    public class MathCheckerTest {

        private MathChecker checker;

        @BeforeMethod
        public void beforeMethod() {
          checker = new MathChecker();
        }

        @Test
        @Parameters("num")
        public void isOdd(int num) { 
          System.out.println("Running test for "+num);
          Boolean result = checker.isOdd(num);
          Assert.assertEquals(result, new Boolean(true));
        }
    }

    public class MathCheckerTest {
 
        private MathChecker checker;
 
        @BeforeMethod
        public void beforeMethod() {
          checker = new MathChecker();
        }
 
        @Test
        @Parameters("num")
        public void isOdd(int num) { 
          System.out.println("Running test for "+num);
          Boolean result = checker.isOdd(num);
          Assert.assertEquals(result, new Boolean(true));
        }
    }
TestNG

