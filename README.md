This Article explains the best practices for selenium scripting to be
followed by all UI Automation Test Engineers. Kindly ensure that all
UI Automation Test Engineers understand this document properly.

In general for java automation scripting, it is recommended to use
java best practices of ***APaSS-PLUS** project and [Google java style
guide](https://google.github.io/styleguide/javaguide.html)*.

1.  [Follow Page Object Model](#follow-page-object-model)

2.  [Avoid Condition Implicit wait.](#avoid-condition-implicit-wait.)

3.  [Confirm whether the page is
    loaded](#confirm-whether-the-page-is-loaded)

4.  [Object Repositories](#object-repositories)

5.  [Web Location Strategies](#web-location-strategies)

6.  [Test Dependency](#test-dependency)

# Follow Page Object Model

All our automation code is in the test method. This is good for a
shortest test, but becomes a problem for larger tests. Imagine a test
that books a flight ticket.

opening the site(Ex: www.xxx.xom) Login Page

Flight Search Screen

filter only the available flights round trip/one way trip entering the
destination/origin passenger Details

Payment

ticket confirmation

So there will be more line of codes in a single class. It is hard to
have all the automation code in a single class.

When writing Selenium test automation scripts, you must keep a check
on its maintainability and scalability. This is possible if changes in
the web page UI requires minimal (or no) changes in the test script.

Suppose the scripts are not appropriately maintained, i.e. different
scripts using the same web element. In that case, whenever there is a
change in the web element, corresponding changes must be made at
multiple places in the test script.

So the best practice is to follow the Page Object Model design pattern
in the selenium, where web pages are represented as classes and the
various elements on the page are defined as variables on the class.
This class file consists of different web elements present on the web
page. Moreover, the test scripts then use these elements to perform
different actions. Since each page\'s web elements are in a separate
class file, the code becomes easy to maintain and reduces code
duplicity.

Here we have a test class and various functions written inside the
class.

![](./images/seleniumBestPractices/image1.png){width="3.75in"
height="0.9791666666666666in"}

![](./images/seleniumBestPractices/image2.jpeg){width="6.739583333333333in"
height="4.65625in"}

Each screen/component is separated into each class.

![](./images/seleniumBestPractices/image3.png){width="1.4791666666666667in"
height="0.7708333333333334in"}

Now consider a scenario where I have to test login with different
users. I will be reusing the same web elements again and again in my
test script. If the locator value of the login button changes, I would
have to change it at different locations, and I might miss out on
changing in one place. Using the *Page Object Model* approach here
would simplify code management and makes it more maintainable and
reusable.

The best practice is to create page classes not only for pages but
also for page components. In the above example, if the flight search
component is having multiple search components, it is better to split
the java classes into the component level.

Since well-named methods in classes are easy to read, this works as an
elegant way to implement test routines that are both readable and
easier to maintain for the future updates.

# Avoid Condition Implicit wait.

Our test will look good with good names, good web locators, no long
class and etc. But when there is a situation where the site is slow,
the test fails finding the elements with the driver.findElement()
method. If the element in the browser\'s DOM, findElement() finds it.
Otherwise, findElement() generates an exception because, for the
slower pages, findElement() doesn\'t wait until the element is in the
page.

So, It is not advised to add Thread.sleep(), because we doesn\'t know
how many seconds to wait for. If a high number of wait is used, then
it is a waste of time if the site is fast. And for a low number, the
test fails if the site is very slow.

Implicit wait is also a bad practice, because if the implicit wait is
set, it is set for the life of the Web driver Object and it cannot be
changed, for example if the implicit wait is for 30 seconds, we can\'t
change the time to 10 seconds later.

*Best Practice*

To use elements with explicit waits and the expected condition.

We have used fuientWaitForElement which takes web locator as an
argument.

![](./images/seleniumBestPractices/image4.png){width="2.53125in"
height="0.125in"}

The web element is found by the until() method of the wait object and
saved in a variable.

![](./images/seleniumBestPractices/image5.png){width="5.8125in" height="0.125in"}

until() method waits for the visibilityofelementlocated condition to
be true (element is in the browser DOM with visible status).

The element in util() is found by

until() method checks if the element in the browser DOM with visible
status

if the element is visible and in DOM, the element is returned and the
until() method stops otherwise, until() sleeps 500 ms and tries again

until() method retires searching for the element until either the
element is found or the wait timeout is reached If the wait timeout is
reached, an exception is generated.

# Confirm whether the page is loaded

The test verifies the login page of the travel fusion booking by
opening the site and login page

filling in the user credentials clicking on the login button

and at last checking on the username displayed on the main page.

This works fine when the website is fast. But when the site is
slow(when the site loads in 15s), an error is generated on
findElement() method.

![](./images/seleniumBestPractices/image6.png){width="2.6041666666666665in"
height="0.125in"}

Since the page is not displayed, the test cannot find the given
locator, and exception is generated.

*Best Practice*

If the test loads a page, verify that the changes are done.

If the test displays a new page, verify that the new page is
displayed.

So, to check the page is loaded, assert that a page element is
displayed.

# Object Repositories

The tests uses page objects to interact with the sites.

If the pages has many features and elements , its corresponding
classes has many locators and methods so the class is quite big. So,
it is a good practice to move the locator details and other parameters
which are to be changed later into the properties file.

This idea can be used in a selenium project as well and locators get
moved from the page classes to the repository. For this purpose in
RTF, you can use properties file like Element Repository Properties
file.

![](./images/seleniumBestPractices/image7.png){width="5.59375in"
height="0.28125in"}

# Web Location Strategies

Finding the element and forming a unique locator query are the two
major aspects of web locators. If a locator is too broad, then it
could return false positives. However, if a locator is too specific,
then it could be susceptible to break whenever the DOM changes, and it
could also be difficult for others to read. The best philosophy is
that write the simplest locator query that uniquely identifies the
target element(s).

![](./images/seleniumBestPractices/image8.png){width="3.6666666666666665in"
height="0.3229166666666667in"}Unique IDs, names, and class names make
locators super easy to write: queries are short and don't need extra
anchors. In RTF, the web locator is specified in the ElementRepository
property files as shown below.

*Order of Preference for the Locator*

1.  By Id

2.  By the input name

3.  Using the class name

4.  By using CSS Selector

5.  XPath without the text

6.  Link text

7.  Partial Link text

8.  XPath with text and/or indexing

  ------------------------------------------------------------------------------------------------------
  **Locator**   **Description**                        **Syntax (in Java)**
  ------------- -------------------------------------- -------------------------------------------------
  ID            Identify the Web Element using the ID  driver.findElement(By.id("IdValue"));
                attribute                              

  Name          Identify the Web Element using the     driver.findElement(By.name("nameValue"));
                Name attribute                         

  ClassName     Use the Class attribute for            driver.findElement(By.className("classValue"));
                identifying the object                 

  LinkText      Use the text in hyperlinks to locate   driver.findElement(By.linkText("textofLink"));
                the WebElement                         

  Partial       Use a part of the text in hyperlinks   driver.findElement(By.partialLinkText
  LinkText      to locate the desired WebElement       ("PartialTextofLink"));

  TagName       Use the TagName to locate the desired  driver.findElement(By.tagName("htmlTag"));
                WebElement                             

  CssSelector   CSS used to create style rules in web  driver.findElement(By.cssSelector("cssValue"));
                page is leveraged to locate the        
                desired WebElement                     

  XPath         Use XPath to locate the WebElement     driver.findElement(By.xpath("xpathValue"));
  ------------------------------------------------------------------------------------------------------

# Test Dependency

Try to create a test script in such a way that it have least
dependency with the other test cases.
