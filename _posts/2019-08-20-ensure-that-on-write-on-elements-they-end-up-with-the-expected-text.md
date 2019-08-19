---
layout: post
title: Ensure that on write on elements they end up with the expected text
date: 2019-08-20 10:32:00
description: >-
  How to avoid type too fast on inputs and text areas ends up with incomplete
  text.
img: current-focused-element.png
tags:
  - Selenium
  - Java
---

When working with selenium sometimes type on elements that are not loaded ends up with cut text on them, this can be solved waiting for the element to have the expected value.

#### Ensure element have the expected text value

First, we need a method to validate that the element is displayed.

{% highlight java %}
  public static void waitForElementToDisplay(WebElement webElement, long secondsToWait) {
    WebDriverWait wait = new WebDriverWait(driver(), secondsToWait);
    wait.until(ExpectedConditions.visibilityOf(webElement));
  }
{% endhighlight %}

Now we define a method that ensures that the element has the expected text and retry type again in case it is incomplete.

{% highlight java %}<br>&nbsp; public static void type(String text, WebElement webElement) \{<br>&nbsp; &nbsp; if (text == null) \{<br>&nbsp; &nbsp; &nbsp; return;<br>&nbsp; &nbsp; \}

&nbsp; &nbsp; waitForElementToDisplay(webElement);

&nbsp; &nbsp; new WebDriverWait(driver(), 30).until((WebDriver) -&gt; \{<br>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; webElement.clear();<br>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; webElement.sendKeys(text);<br>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; return webElement.getAttribute("value").equals(text);<br>&nbsp; &nbsp; &nbsp; &nbsp; \}<br>&nbsp; &nbsp; );<br>&nbsp; \}<br>{% endhighlight %}

So that is all\! Now we have a method that ensures an element is visible and have the expected text on it.

Try it and comment if it worked for you.