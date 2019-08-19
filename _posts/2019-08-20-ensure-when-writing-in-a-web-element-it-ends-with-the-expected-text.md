---
layout: post
title: Ensure when writing in a web element it ends with the expected text
date: 2019-08-20 10:32:00
description: >-
  How to avoid type too fast on inputs and text areas ends up with incomplete
  text.
img: retrytype.png
tags:
  - Selenium
  - Java
---

When working with selenium sometimes write on elements that are not loaded ends up with cut text on them, this can be solved waiting for the element to have the expected value.

#### Ensure element have the expected text value

First, we need a method to validate that the element is displayed.

{% highlight java %}
  public static void waitForElementToDisplay(WebElement webElement, long secondsToWait) {
    WebDriverWait wait = new WebDriverWait(driver(), secondsToWait);
    wait.until(ExpectedConditions.visibilityOf(webElement));
  }
{% endhighlight %}

Now we define a method that ensures that the element has the expected text and retry type again in case it is incomplete.

{% highlight java %}
public static void type(String text, WebElement webElement) {
  if (text == null)
     return;<br>

  waitForElementToDisplay(webElement);
  new WebDriverWait(driver(), 30).until((WebDriver) -> {
    webElement.clear();
    webElement.sendKeys(text);
    return webElement.getAttribute("value").equals(text);
  });
}
{% endhighlight %}

So that is all\! Now we have a method that ensures an element is visible and have the expected text on it.

Try it and comment if it worked for you.