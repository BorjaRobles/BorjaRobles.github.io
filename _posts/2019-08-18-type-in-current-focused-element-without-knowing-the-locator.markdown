---
layout: post
title: Type in current focused element without knowing the locator
date: 2019-08-18 10:32:00
description: How to type in current focused element without knowing the locator
img: current-focused-element.png
tags:
  - Selenium
  - Java
---

Today I want to discuss this helper method that avoids declaring extra locators, lets put an example of a login form where users have to type username and password.

* Type "username" in usernameInput
* Type "password" in passwordInput
* Click "Login button"

So typically we declare 3 locators (username, password, login button), but let us see how this form usually works.

* Users clicks on the username
* Users writes the username and press enter
* The focus is redirected to the password field
* Users types the password
* User press enter or clicks on the login button

We can emulate this simple behavior with the following method.

## Working with focused elements

{% highlight java %}
  public static void elementFocused() {
    boolean isFocusSet = new WebDriverWait(driver(), 30).until((WebDriver) ->
        driver().switchTo().activeElement().getLocation().getX() > 0 &&
            driver().switchTo().activeElement().getLocation().getY() > 0
    );

    if(!isFocusSet)
      throw new WebDriverException("No element is focused");
  }

  public static void type(CharSequence... keys) {
    elementFocused();
    driver().switchTo().activeElement().sendKeys(keys);
  }
{% endhighlight %}

Now we can do the following.

* Type on username input and press enter key (1 declared locator)
* Type on the password input and press enter ( The focus is on the element, so it does not need to declare the locator)

Great\! We have avoided declaring 2 locators in a single test, so remember that declare extra locators and wait for them to be ready and fully loaded when you don't need them only makes your tests challenging to read, maintain and slower.