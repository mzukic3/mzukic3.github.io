---
layout: post
title: "Creating Custom Views in Android "
subtitle: ""
date: 2021-01-01 23:45:13 -0400
background: '/img/posts/androidlogo.jpg'
---

The Android platform offers a large range of user interface widgets that are sufficient for the needs of most applications. These widgets are great and certainly provide us with functional and appealing end products, but sometimes us software developers like to think outside of the box and develop our own custom interfaces. What is the best way to approach this type of creativity? By building a custom View!

In this blog, we’ll define and discuss custom Views before moving onto the fun part-the demonstration!

## Defining Important Terms

To begin, we’ll define some basic terminology for a better understanding.

**What is Android View?**

Android View is the base class for building a user interface giving developers the opportunity to create complex designs. The View occupies a rectangular area on the screen, where it’s responsible for measuring, laying out and drawing itself along with its child elements. In addition, a View handles all user event inputs.

![Flowchart explaining Android View.](/img/posts/01.jpg)

**What is a ViewGroup?**

A ViewGroup is a special view that is able to contain other Views (children) and define its own layout properties. It is also a place where each subview can draw itself.

**What is a Custom View?**

Any View created outside of the Android base widget set can be referred to as a Custom View. This will be the main focus of the blog post.

## Implementation Methods 

There are a lot of different ways to implement custom Views and the approach that is chosen depends on your needs. Let's check out some methods:

**Extending the existing Android widget-**This method is useful when a large amount of setup code is required for your View and you want to reuse it in multiple locations. To avoid all of the messy code inside of your activity/fragments, you can extend the base widget and do all of the setup inside the constructor, therefore, it can be easily reused. This method is arguably the simplest approach to implementing custom Views.

**Extending the Android base View-** If you want to get innovative and do everything from scratch, this method is ideal. You will be drawing, measuring and planning all of the behaviour logic on your own.

<span>**Grouping existing Views together-** Sometimes you have a set of widgets that you want to group together to create a whole new View. For instance, you have the Textview and the Button and you want to group them inside the LinearLayout. This is usually referred to as the Compound View. The benefits of doing this are:</span>

*   An encapsulated and centralized logic
*   Ability to avoid code duplication
*   Reusability and modularity

## How Android Draws Views

Let’s talk about how Android draws the Views. To begin, there are three phases that have to happen before the View ends up on the screen. These three phases are **measure, layout,** and **draw.** Each of these phases is the depth-first traversal of the View hierarchy going from parent to children. For each phase, there is a method that we can override and change, depending on our needs. The methods are **onMeasure(), onLayout() **and **onDraw().**

This process can be divided into two stages:

*   The Measuring & Layout Stage
*   The Drawing stage

![Diagram of measure, layout and draw stages when Android draws Views.](/img/posts/02.jpg)

**The Measuring & Layout Stage**

In this stage, we have the opportunity to tell the Android system the size we’d like our custom View to be, depending on the constraints provided by the parent.

The following numbered diagram displays how each View is measured by showing each step:

![Numbered diagram showing steps for measuring and layout stage for creating Android Views.](/img/posts/03.jpg)

1\. The child View defines the **LayoutParams** programmatically or in the **XML** and the parent retrieves these values using the **getLayoutParams().**

2\. The parent calculates the **MeasureSpecs** and passes it down using the **child.measure().** The **Measurespecs** contain the mode and the value.

The three modes of measurement are:

**EXACTLY**- A precise size such as setting the width/height to 50dp or **match_parent.**  
**AT_MOST**- The parent gives maximum size and the child adapts to it. This is the case for setting the width/height to the **wrap_content.**  
**UNSPECIFIED**- There is no clear size, the child is free to play.

3\. The **onMeasure()** method is called with the **MeasureSpecs** parameters. In this method, the View calculates its desired width/height and sets it using the **setMeasuredDimension**. Keep in mind that the **setMeasuredDimension **method must be called inside measure otherwise it will cause a runtime exception.

4. The next and final phase is the layout phase. In this phase, the parent calls the **child.layout()** and sets the final size and position of the child. When implementing your custom View, you should only override the **onLayout() **method if your View has other subviews.

To conclude, the measuring process is like a negotiation between a parent and child. The child calculates its desired width & height, but the parent is the one who makes the final call setting the position and size of its child.

**Drawing Stage**

The last and most important step in drawing a custom View is to override the **onDraw()** method. The Canvas is a base class that defines many methods for drawing text, bitmaps, lines and other graphic primitives.

Each parent will draw itself and then will request that each child do the same. An interesting side effect is when the parent draws itself first and it ends up on the bottom as its children are drawn on the top covering it.

## Creating a Custom View

![Philip J. Fry from Futurama saying "Show me the code and stop talking!"](/img/posts/05.jpg)

Now for the part that we’ve all been waiting for: the code. Let’s take a look at how to create a custom View using Kotlin. For this demonstration, we’ll be creating a Battery Meter to show the current status of a battery. The following diagram displays the three different statuses of a battery:

![Diagram of the three different statuses of a battery.](/img/posts/06.jpg)

We can follow these steps in order to create a BatteryMeterView:

Create a new [Android Studio](https://developer.android.com/studio "https://developer.android.com/studio") project and add a new class called the BatteryMeterView.

Extend it with the View class and add constructors matching super 

    class BatteryMeterView @JvmOverloads constructor(
    context: Context, attrs: AttributeSet? = null, defStyleAttr: Int = 0
    ) : View(context, attrs, defStyleAttr)

To prepare our drawing, we will declare some paint objects, colours and shapes.  
Just like a basic widget, we want our View to have as little setup needed to initialize all of the properties with some default value.

<span style="font-weight: 400;">Let’s create a companion object inside BatteryMeterView and add some constants to it.</span>

    companion object {
        private const val DEFAULT_CHARGING_STATE = false
        private const val DEFAULT_BATTERY_LEVEL = 70
        private const val DEFAULT_WARNING_LEVEL = 30
        private const val DEFAULT_BATTERY_LEVEL_COLOR = Color.GREEN
        private const val DEFAULT_WARNING_COLOR = Color.RED
        private const val DEFAULT_BACKGROUND_COLOR = Color.LTGRAY
        private const val DEFAULT_BATTERY_HEAD_COLOR = Color.DKGRAY
        private const val DEFAULT_TEXT_COLOR = Color.DKGRAY
        private const val DEFAULT_CHARGING_COLOR = Color.DKGRAY
        private const val TEXT_SIZE_RATIO = 0.5f
    }

    ​// Battery dimensions
    private var contentHeight: Int = 0
    private var contentWidth: Int = 0
    private var batteryHeadWidth = 0
    private var mainContentOffset: Int = 20

    // Shapes
    private val backgroundRect: Rect = Rect()

    private val batteryLevelRect: Rect = Rect()

    private val batteryHeadRect: Rect = Rect()

    private val chargingLogoPath: Path = Path()

    // Paints
    private val backgroundPaint: Paint = Paint(ANTI_ALIAS_FLAG)

    private val backgroundPaintStroke: Paint = Paint(ANTI_ALIAS_FLAG)

    private val textValuePaint: Paint = Paint(ANTI_ALIAS_FLAG)

    private val batteryHeadPaint: Paint = Paint(ANTI_ALIAS_FLAG)

    private val batteryLevelPaint: Paint = Paint(ANTI_ALIAS_FLAG)

    private val chargingLogoPaint: Paint = Paint(ANTI_ALIAS_FLAG)

    // Colors
    var batteryLevelColor: Int = DEFAULT_BATTERY_LEVEL_COLOR
        set(@ColorInt color) {
        field = color
        batteryLevelPaint.color = color
        invalidate()
    }

    var warningColor: Int = DEFAULT_WARNING_COLOR
        set(@ColorInt color) {
        field = color
        batteryLevelPaint.color = color
        invalidate()
    }

    var backgroundRectColor: Int = DEFAULT_BACKGROUND_COLOR
        set(@ColorInt color) {
        field = color
        backgroundPaint.color = color
        invalidate()
    }

    var batteryHeadColor: Int = DEFAULT_BATTERY_HEAD_COLOR
        set(@ColorInt color) {
        field = color
        batteryHeadPaint.color = color
        invalidate()
    }

    var chargingColor: Int = DEFAULT_CHARGING_COLOR
        set(@ColorInt color) {
        field = color
        chargingLogoPaint.color = color
        invalidate()
    }

    var textColor: Int = DEFAULT_TEXT_COLOR
        set(@ColorInt color) {
        field = color
        textValuePaint.color = color
        invalidate()
    }

    init {
        parseAttr(attrs)

        batteryLevelPaint.apply {
            style = Paint.Style.FILL
            color = batteryLevelColor
        }

        backgroundPaint.apply {
            style = Paint.Style.FILL
            color = backgroundRectColor
        }

        backgroundPaintStroke.apply {
            style = Paint.Style.STROKE
            strokeWidth = 20f
            color = Color.BLACK
        }

        batteryHeadPaint.apply {
            style = Paint.Style.FILL
            color = batteryHeadColor
        }

        chargingLogoPaint.apply {
            style = Paint.Style.FILL_AND_STROKE
            color = chargingColor
            strokeWidth = 5f
        }

        textValuePaint.apply {
            textAlign = Paint.Align.CENTER
            color = textColor
        }
    }

Before drawing the battery on the screen, we have to update its size and position. The best place to handle any size changes is inside the onSizeChanged method. We can follow these steps:

1.  Set the width and height of the content.
2.  Set the text size of battery value to half of the content height.
3.  <span>Set the width of the battery head to 1/12 of the total width. </span>
4.  Set the background rect position.
5.  Set the battery head rect position
6.  Set the battery level rect position. 

<span>Note: For the purpose of this example, we will use some hardcoded values for the padding and content offset.</span>

    override fun onSizeChanged(width: Int, height: Int, oldw: Int, oldh: Int) {
        contentWidth = width - paddingLeft - paddingRight
        contentHeight = height - paddingTop - paddingBottom
        textValuePaint.textSize = contentHeight * TEXT_SIZE_RATIO
        batteryHeadWidth = (1f / 12f * contentWidth).toInt()
        backgroundRect.set(
            15,
            15,
            contentWidth - batteryHeadWidth - 15,
            contentHeight - 15
        )
        batteryHeadRect.set(
            backgroundRect.right + 20,
            backgroundRect.top + contentHeight / 4,
            backgroundRect.left + contentWidth - 20,
            backgroundRect.top + contentHeight * 3 / 4
        )
        batteryLevelRect.set(
            backgroundRect.left + mainContentOffset,
            backgroundRect.top + mainContentOffset,
            ((backgroundRect.right - mainContentOffset) *        (this.batteryLevel.toDouble() / 100.toDouble())).toInt(),
            backgroundRect.bottom - mainContentOffset
        )
    }

 Now to draw the BatteryMeter we’ll start by overriding the onDraw() method.

1.  Draw the background of the View. 
2.  Draw the battery head. 
3.  Draw the container where our battery level will be placed.
4.  Now if the battery is charging, we will draw a charging logo, otherwise, we will draw the text of the current battery value.

Keep in mind that the<span> </span>**onDraw**<span> </span>method is called 60 times per second (60fps) and putting any heavy operations and object creation inside it can cause bad performance in your app. To avoid this, we can create all of the objects inside constructors and if needed we can change the properties later on.

    override fun onDraw(canvas: Canvas) {
        // Draw the background body of battery view
        drawBackground(canvas)

        // Draw the head of battery
        drawBatteryHead(canvas)

        // Draw the current battery level
        drawBatteryLevel(canvas)

        if (isCharging) {
            drawChargingLogo(canvas)
        } else {
            drawCurrentBatteryValueText(canvas)
        }
    }

    private fun drawBackground(canvas: Canvas) {
        canvas.drawRect(backgroundRect, backgroundPaint)
        canvas.drawRoundRect(RectF(backgroundRect), 50f, 50f, backgroundPaintStroke)
    }

    private fun drawBatteryHead(canvas: Canvas) {
        // Draw the head of battery view
        canvas.drawRoundRect(RectF(batteryHeadRect), 10f, 10f, batteryHeadPaint)
    }

    private fun drawBatteryLevel(canvas: Canvas) {
        if (batteryLevel <= warningLevel) {
            batteryLevelPaint.color = warningColor
        } else {
            batteryLevelPaint.color = batteryLevelColor
        }

        if (batteryLevel == 0) {
            drawEmptyText(canvas)
        } else {
            canvas.drawRoundRect(RectF(batteryLevelRect), 25f, 25f, batteryLevelPaint)
        }
    }

    private fun drawChargingLogo(canvas: Canvas) {
        VectorDrawableCompat.create(
            context.resources,
            R.drawable.ic_charging_bolt,
            null
        )?.apply {
            setBounds(
                backgroundRect.left + contentWidth/4,
                backgroundRect.top + contentHeight/4,
                backgroundRect.right - contentWidth/4,
                backgroundRect.bottom - contentHeight/4
            )
            setColorFilter(chargingColor,PorterDuff.Mode.SRC_IN)
            draw(canvas)
        }
    }

    private fun drawCurrentBatteryValueText(canvas: Canvas) {
        val text = if (batteryLevel == 0) "Empty" else batteryLevel.toString()
        canvas.drawText(
        text,
        (contentWidth * 0.45).toFloat(),
        (contentHeight * 0.7).toFloat(),
        textValuePaint
        )
    }

    private fun drawEmptyText(canvas: Canvas) {
        canvas.drawText(
            "Empty",
            (contentWidth * 0.45).toFloat(),
            (contentHeight * 0.7).toFloat(),
            textValuePaint
        )
    }

Now to give our battery the ability to change at runtime we need to call the **invalidate() method** every time we update the View state. What invalidate does is it lets Android know that the view is dirty and that it needs to be redrawn. It is important to note that you need to be careful since calling the **invalidate()** too many times can cause problems.

    var isCharging: Boolean = DEFAULT_CHARGING_STATE
        @CheckResult
        get() = field
        set(value) {
        field = value
        invalidate()
    }
    var batteryLevel: Int = DEFAULT_BATTERY_LEVEL
        @CheckResult
        get() = field
        set(level) {
            field = when {
            level > 100 -> 100
            level < 0 -> 0
            else -> level
        }
        if (field <= warningLevel) {
            fillPaint.color = warningFillColor
        } else {
            fillPaint.color = normalFillColor
        }
        invalidate()
    }

The final step is to add the battery View to your layout like this:

    <ba.rubicon.widget.BatteryMeterView android:layout_width="200dp"
        android:layout_height="90dp"
        app:battery_level="30"
        android:layout_gravity="center"
        app:charging="true"/>

All done. There you have it, a Battery Meter that you've created yourself.

For the project source code, you can check out my [Github.](https://github.com/rubiconba/android-sample-batterymeter-custom-view "https://github.com/rubiconba/android-sample-batterymeter-custom-view")

Now that we’ve walked through creating the Battery Meter, I encourage you to try creating your own custom View. It will definitely be fun!

## The Pros & Cons of Implementing Custom Views

Before concluding, I’d like to share both the pros and cons of implementing custom Views. Just like any other implementation process, there are always both pros and cons but this shouldn’t discourage you from giving it a try.

**THE PROS: **

**Custom look/behaviour**

Custom view = Customization. The Android platform is vast but there are specific scenarios where the features or Views of Android don’t meet your needs, therefore, Custom View gives you the opportunity to build something of your own. When it comes to design and interaction, you have complete control since custom View provides endless options.

**Reusability/Readability**  
When developing large scale applications, code reusability is always welcome. Once you create a custom View, it can easily be reused in multiple locations across the application.

**Performance**  
In specific scenarios, building custom view can squeeze some performance.

**THE CONS:**

**Time & effort**  
Custom views are time-consuming and they can definitely be difficult to use until you get the hang of them.

**Diligence**  
There are a number of things you need to be aware of when implementing custom Views. Firstly, you have to ensure that you handle the font, text size, colour, shadows, highlight and style properly.

You also need to make sure that it works properly on all screen densities because Android canvas class draws in pixels not DP. Lastly, if you’re working with images, you’ll have to keep in mind the aspect ratio, zoom, scaling, etc.

**Lots to manage.**  
You’ll also have to handle all kinds of click listeners and user interactions — single click, double click, long press, swipe and fling.

## Final Words

I hope this tutorial has encouraged you to get creative with Android and make your own custom UI. If you have any questions or would like to discuss this topic, I’d be happy to do so.

Good luck coding!
