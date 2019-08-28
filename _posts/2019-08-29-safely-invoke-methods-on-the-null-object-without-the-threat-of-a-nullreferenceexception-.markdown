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

So typically ...

## &nbsp;

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

public WebElement findElement(By by) \{<br>&nbsp; &nbsp; &nbsp; &nbsp; try \{<br>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; WebDriverWait wait = new WebDriverWait(driver, 5);<br>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; return wait.until(ExpectedConditions.visibilityOfElementLocated(by));<br>&nbsp; &nbsp; &nbsp; &nbsp; \} catch (Exception e) \{<br>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; return NullWebElement.getNull();<br>&nbsp; &nbsp; &nbsp; &nbsp; \}<br>&nbsp; &nbsp; \}

&nbsp; &nbsp; public List&lt;WebElement&gt; findElements(By by) \{<br>&nbsp; &nbsp; &nbsp; &nbsp; WebDriverWait wait = new WebDriverWait(driver, 5);<br>&nbsp; &nbsp; &nbsp; &nbsp; return wait.until(ExpectedConditions.presenceOfAllElementsLocatedBy(by));<br>&nbsp; &nbsp; \}

{% endhighlight %}

And thats all \! Now we can use the find methods without the nullpointer exception \!\!

&nbsp;