---
layout: post
title: >-
  Safely invoke methods on the null object without the threat of a
  NullReferenceException.
date: 2019-08-29 10:32:00
description: >-
  The idea is to make code safe by skipping null reference checks and
  implementing an interface with methods that do nothing making the null object
  predictable and safe.
img: nullexception.png
tags:
  - Selenium
  - Java
---

The idea is to make code safe by skipping null reference checks and implementing an interface with methods that do nothing making the null object predictable and safe.

So typically when we search for an element and the element is not found we get a null, for make is more easy work with not found elements we implement the nullWebElement.

## Implementing the null webElement&nbsp;

{% highlight java %}
  public class NullWebElement implements WebElement {

    private NullWebElement() {}

    private static NullWebElement instance;

    public static NullWebElement getNull() {
        if (instance == null)
          instance = new NullWebElement();

        return instance;
    }

    public static boolean isNull(WebElement element) {
        return getNull().equals(element);
    }

    @Override
    public void click() {}

    @Override
    public void submit() {}

    @Override
    public void sendKeys(CharSequence... charSequences) {}

    @Override
    public void clear() {}

    @Override
    public String getTagName() {
        return "";
    }

    @Override
    public String getAttribute(String s) {
        return "";
    }

    @Override
    public boolean isSelected() {
        return false;
    }

    @Override
    public boolean isEnabled() {
        return false;
    }

    @Override
    public String getText() {
        return "";
    }

    @Override
    public List<WebElement> findElements(By by) {
        return new ArrayList<>();
    }

    @Override
    public WebElement findElement(By by) {
        return NullWebElement.getNull();
    }

    @Override
    public boolean isDisplayed() {
        return false;
    }

    @Override
    public Point getLocation() {
        return new Point(0, 0);
    }

    @Override
    public Dimension getSize() {
        return new Dimension(0, 0);
    }

    @Override
    public Rectangle getRect() {
        return new Rectangle(0, 0, 0, 0);
    }

    @Override
    public String getCssValue(String s) {
        return "";
    }

    @Override
    public <X> X getScreenshotAs(OutputType<X> outputType) throws WebDriverException {
        return null;
    }
}
{% endhighlight %}

Now, all we need to do is redefine the findby methods to use this new WebElement.

{% highlight java %}

public WebElement findElement(By by) {
  try {
    WebDriverWait wait = new WebDriverWait(driver, 5);
    return wait.until(ExpectedConditions.visibilityOfElementLocated(by));
  } catch (Exception e) {
    return NullWebElement.getNull();
  }
}

public List<WebElement> findElements(By by) {
  WebDriverWait wait = new WebDriverWait(driver, 5);
  return wait.until(ExpectedConditions.presenceOfAllElementsLocatedBy(by));
}
{% endhighlight %}

And thats all \! Now we can use the find methods without the nullpointer exception \!\!