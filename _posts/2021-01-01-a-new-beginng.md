---
layout: post
title: "Creating Custom Views in Android "
subtitle: "Pros for starting as mobile developer"
date: 2021-01-01 23:45:13 -0400
background: '/img/posts/mobile.jpg'
---

<div class="container">
        <div class="row without-margins">
            <div class="centering">
                <div class="content-software centering">
                    <div class="service-text">
                        <p>The Android platform offers a large range of user interface widgets that are sufficient for the needs of most applications. These widgets are great and certainly provide us with functional and appealing end products, but sometimes us software developers like to think outside of the box and develop our own custom interfaces. What is the best way to approach this type of creativity? By building a custom View!</p>
<p>In this blog, we’ll define and discuss custom Views before moving onto the fun part-the demonstration!</p>
<h2>Defining Important Terms</h2>
<p>To begin, we’ll define some basic terminology for a better understanding.</p>
<p><strong>What is Android View?</strong></p>
<p>Android View is the base class for building a user interface giving developers the opportunity to create complex&nbsp;designs. The View occupies a rectangular area on the screen, where it’s responsible for measuring, laying out and drawing itself along with its child elements. In addition, a View handles all user event inputs.</p>
<p><img src="/media/1535/graph-1-1x.jpg" alt="Flowchart explaining Android View." data-udi="umb://media/68cebb7dbbd94ade98102a7bb6a6fa15"></p>
<p><strong>What is a ViewGroup?</strong></p>
<p>A ViewGroup is a special view that is able to contain other Views (children) and define its own layout properties. It is also a place where each subview can draw itself.</p>
<p><strong>What is a Custom View?</strong></p>
<p>Any View created outside of the Android base widget set can be referred to as a Custom View. This will be the main focus of the blog post.</p>
<h2>Implementation Methods&nbsp;</h2>
<p>There are a lot of different ways to implement custom Views and the approach that is chosen depends on your needs.&nbsp;Let's check out some methods:</p>
<p><strong>Extending the existing Android widget-</strong>This method is useful when a large amount of setup code is required for your View and you want to reuse it in multiple locations. To avoid all of the messy code inside of your activity/fragments, you can extend the base widget and do all of the setup inside the constructor, therefore, it can be easily reused. This method is arguably the simplest approach to implementing custom Views.</p>
<p><strong>Extending the Android base View-</strong> If you want to get innovative and do everything from scratch, this method is&nbsp;ideal. You will be drawing, measuring and planning all of the behaviour logic on your own.</p>
<p><span><strong>Grouping existing Views together-</strong> Sometimes you have a set of widgets&nbsp;that you want to group together to create a whole new View. For instance, you have the Textview and the Button and you want to group them inside the LinearLayout. This is usually referred to as the Compound View. The benefits of doing this are:</span></p>
<ul>
<li>An encapsulated and centralized logic</li>
<li>Ability to avoid code duplication</li>
<li>Reusability and modularity</li>
</ul>
<h2>How Android Draws Views</h2>
<p>Let’s talk about how Android draws the Views. To begin, there are three phases that have to happen before the View ends up on the screen. These three phases are <strong>measure, layout,</strong> and <strong>draw.</strong> Each of these phases is the depth-first traversal of the View hierarchy going from parent to children. For each phase, there is a method that we can override and change, depending on our needs. The methods are <strong>onMeasure(), onLayout()&nbsp;</strong>and&nbsp;<strong>onDraw().</strong></p>
<p>This process can be divided into two stages:</p>
<ul>
<li>The Measuring &amp; Layout Stage</li>
<li>The Drawing stage</li>
</ul>
<p><img src="/media/1536/graph-2-1x.jpg" alt="Diagram of measure, layout and draw stages when Android draws Views." data-udi="umb://media/ffa5db384e084c91801bdeed582344f3"></p>
<p><strong>The Measuring &amp; Layout Stage</strong></p>
<p>In this stage, we have the opportunity to tell the Android system the size we’d like our custom View to be, depending on the constraints provided by the parent.</p>
<p>The following numbered diagram displays how each View is measured by showing each step:</p>
<p><img src="/media/1564/graph-6-together-1x.jpg" alt="Numbered diagram showing steps for measuring and layout stage for creating Android Views." data-udi="umb://media/990d98c820f74042a6c1489ccb5edbdb"></p>
<p>1. The child View defines the <strong>LayoutParams</strong> programmatically or in the&nbsp;<strong>XML</strong> and the parent retrieves these values using the&nbsp;<strong>getLayoutParams().</strong></p>
<p>2. The parent calculates the <strong>MeasureSpecs</strong> and passes it down using the&nbsp;<strong>child.measure().</strong> The <strong>Measurespecs</strong> contain the mode and the value.</p>
<p>The three modes of measurement are:</p>
<p><strong>EXACTLY</strong>- A precise size such as setting the width/height to 50dp or <strong>match_parent.</strong><br><strong>AT_MOST</strong>- The parent gives maximum size and the child adapts to it. This is the case for setting the width/height to the&nbsp;<strong>wrap_content.</strong><br><strong>UNSPECIFIED</strong>- There is no clear size, the child is free to play.</p>
<p>3. The&nbsp;<strong>onMeasure()</strong> method is called with the&nbsp;<strong>MeasureSpecs</strong> parameters. In this method, the View calculates its desired width/height and sets it using the <strong>setMeasuredDimension</strong>. Keep in mind that the <strong>setMeasuredDimension&nbsp;</strong>method must be called inside measure otherwise it will cause a runtime exception.</p>
<p>4.&nbsp;The next and final phase is the layout phase. In this phase, the parent calls the&nbsp;<strong>child.layout()</strong> and sets the final size and position of the child. When implementing your custom View, you should only override the&nbsp;<strong>onLayout()&nbsp;</strong>method if your View has other subviews.</p>
<p>To conclude, the measuring process is like a negotiation between a parent and child. The child calculates its desired width &amp; height, but the parent is the one who makes the final call setting the position and size of its child.</p>
<p><img src="/media/1540/graph-4-2x.jpg" alt="Diagram of how android draws views." data-udi="umb://media/2ce51367c8f5468e837ecd38e5da5d5c"></p>
<p><strong>Drawing Stage</strong></p>
<p>The last and most important step in drawing a custom View is to override the <strong>onDraw()</strong> method. The Canvas is a base class that defines many methods for drawing text, bitmaps, lines and other graphic primitives.</p>
<p>Each parent will draw itself and then will request that each child do the same. An interesting side effect is when the parent draws itself first and it ends up on the bottom as its children are drawn on the top covering it.</p>
<h2>Creating a Custom View</h2>
<p><img src="/media/1546/meme-1x.jpg" alt="Philip J. Fry from Futurama saying &quot;Show me the code and stop talking!&quot;" data-udi="umb://media/3823a67c07e34da68ff1d0d73fbfe166"></p>
<p>Now for the part that we’ve all been waiting for: the code. Let’s take a look at how to create a custom View using Kotlin. For this demonstration, we’ll be creating a Battery Meter to show the current status of a battery. The following diagram displays the three different statuses of a battery:</p>
<p><img src="/media/1597/app-1x.jpg" alt="Diagram of the three different statuses of a battery." data-udi="umb://media/0f321d04f0a64a3b8c0ff4932ede55b8"></p>
<p>We can follow these steps in order to create a BatteryMeterView:</p>
<p>Create a new <a rel="noopener noreferrer" href="https://developer.android.com/studio" target="_blank" title="https://developer.android.com/studio">Android Studio</a> project and add a new class called the BatteryMeterView.</p>
<p>Extend it with the View class and add constructors matching super&nbsp;</p>
<pre><code class="{{scope.language}} hljs typescript"><span class="hljs-keyword">class</span> BatteryMeterView <span class="hljs-meta">@JvmOverloads</span> <span class="hljs-keyword">constructor</span>(<span class="hljs-params">
context: Context, attrs: AttributeSet? = <span class="hljs-literal">null</span>, defStyleAttr: Int = 0
</span>) : View(<span class="hljs-params">context, attrs, defStyleAttr</span>)<br></code></pre>
<p>To prepare our drawing, we will declare some paint objects, colours and shapes. <br>Just like a basic widget, we want our View to have as little setup needed to initialize all of the properties with some default value.</p>
<p><span style="font-weight: 400;">Let’s create a companion object inside BatteryMeterView and add some constants to it.</span></p>
<pre><code class="{{scope.language}} hljs scala">companion <span class="hljs-class"><span class="hljs-keyword">object</span> </span>{
    <span class="hljs-keyword">private</span> const <span class="hljs-keyword">val</span> <span class="hljs-type">DEFAULT_CHARGING_STATE</span> = <span class="hljs-literal">false</span>
    <span class="hljs-keyword">private</span> const <span class="hljs-keyword">val</span> <span class="hljs-type">DEFAULT_BATTERY_LEVEL</span> = <span class="hljs-number">70</span>
    <span class="hljs-keyword">private</span> const <span class="hljs-keyword">val</span> <span class="hljs-type">DEFAULT_WARNING_LEVEL</span> = <span class="hljs-number">30</span>
    <span class="hljs-keyword">private</span> const <span class="hljs-keyword">val</span> <span class="hljs-type">DEFAULT_BATTERY_LEVEL_COLOR</span> = <span class="hljs-type">Color</span>.<span class="hljs-type">GREEN</span>
    <span class="hljs-keyword">private</span> const <span class="hljs-keyword">val</span> <span class="hljs-type">DEFAULT_WARNING_COLOR</span> = <span class="hljs-type">Color</span>.<span class="hljs-type">RED</span>
    <span class="hljs-keyword">private</span> const <span class="hljs-keyword">val</span> <span class="hljs-type">DEFAULT_BACKGROUND_COLOR</span> = <span class="hljs-type">Color</span>.<span class="hljs-type">LTGRAY</span>
    <span class="hljs-keyword">private</span> const <span class="hljs-keyword">val</span> <span class="hljs-type">DEFAULT_BATTERY_HEAD_COLOR</span> = <span class="hljs-type">Color</span>.<span class="hljs-type">DKGRAY</span>
    <span class="hljs-keyword">private</span> const <span class="hljs-keyword">val</span> <span class="hljs-type">DEFAULT_TEXT_COLOR</span> = <span class="hljs-type">Color</span>.<span class="hljs-type">DKGRAY</span>
    <span class="hljs-keyword">private</span> const <span class="hljs-keyword">val</span> <span class="hljs-type">DEFAULT_CHARGING_COLOR</span> = <span class="hljs-type">Color</span>.<span class="hljs-type">DKGRAY</span>
    <span class="hljs-keyword">private</span> const <span class="hljs-keyword">val</span> <span class="hljs-type">TEXT_SIZE_RATIO</span> = <span class="hljs-number">0.5</span>f
}

​<span class="hljs-comment">// Battery dimensions</span>
<span class="hljs-keyword">private</span> <span class="hljs-keyword">var</span> contentHeight: <span class="hljs-type">Int</span> = <span class="hljs-number">0</span>
<span class="hljs-keyword">private</span> <span class="hljs-keyword">var</span> contentWidth: <span class="hljs-type">Int</span> = <span class="hljs-number">0</span>
<span class="hljs-keyword">private</span> <span class="hljs-keyword">var</span> batteryHeadWidth = <span class="hljs-number">0</span>
<span class="hljs-keyword">private</span> <span class="hljs-keyword">var</span> mainContentOffset: <span class="hljs-type">Int</span> = <span class="hljs-number">20</span>

<span class="hljs-comment">// Shapes</span>
<span class="hljs-keyword">private</span> <span class="hljs-keyword">val</span> backgroundRect: <span class="hljs-type">Rect</span> = <span class="hljs-type">Rect</span>()

<span class="hljs-keyword">private</span> <span class="hljs-keyword">val</span> batteryLevelRect: <span class="hljs-type">Rect</span> = <span class="hljs-type">Rect</span>()

<span class="hljs-keyword">private</span> <span class="hljs-keyword">val</span> batteryHeadRect: <span class="hljs-type">Rect</span> = <span class="hljs-type">Rect</span>()

<span class="hljs-keyword">private</span> <span class="hljs-keyword">val</span> chargingLogoPath: <span class="hljs-type">Path</span> = <span class="hljs-type">Path</span>()

<span class="hljs-comment">// Paints</span>
<span class="hljs-keyword">private</span> <span class="hljs-keyword">val</span> backgroundPaint: <span class="hljs-type">Paint</span> = <span class="hljs-type">Paint</span>(<span class="hljs-type">ANTI_ALIAS_FLAG</span>)

<span class="hljs-keyword">private</span> <span class="hljs-keyword">val</span> backgroundPaintStroke: <span class="hljs-type">Paint</span> = <span class="hljs-type">Paint</span>(<span class="hljs-type">ANTI_ALIAS_FLAG</span>)

<span class="hljs-keyword">private</span> <span class="hljs-keyword">val</span> textValuePaint: <span class="hljs-type">Paint</span> = <span class="hljs-type">Paint</span>(<span class="hljs-type">ANTI_ALIAS_FLAG</span>)

<span class="hljs-keyword">private</span> <span class="hljs-keyword">val</span> batteryHeadPaint: <span class="hljs-type">Paint</span> = <span class="hljs-type">Paint</span>(<span class="hljs-type">ANTI_ALIAS_FLAG</span>)

<span class="hljs-keyword">private</span> <span class="hljs-keyword">val</span> batteryLevelPaint: <span class="hljs-type">Paint</span> = <span class="hljs-type">Paint</span>(<span class="hljs-type">ANTI_ALIAS_FLAG</span>)

<span class="hljs-keyword">private</span> <span class="hljs-keyword">val</span> chargingLogoPaint: <span class="hljs-type">Paint</span> = <span class="hljs-type">Paint</span>(<span class="hljs-type">ANTI_ALIAS_FLAG</span>)

<span class="hljs-comment">// Colors</span>
<span class="hljs-keyword">var</span> batteryLevelColor: <span class="hljs-type">Int</span> = <span class="hljs-type">DEFAULT_BATTERY_LEVEL_COLOR</span>
    set(<span class="hljs-meta">@ColorInt</span> color) {
    field = color
    batteryLevelPaint.color = color
    invalidate()
}

<span class="hljs-keyword">var</span> warningColor: <span class="hljs-type">Int</span> = <span class="hljs-type">DEFAULT_WARNING_COLOR</span>
    set(<span class="hljs-meta">@ColorInt</span> color) {
    field = color
    batteryLevelPaint.color = color
    invalidate()
}

<span class="hljs-keyword">var</span> backgroundRectColor: <span class="hljs-type">Int</span> = <span class="hljs-type">DEFAULT_BACKGROUND_COLOR</span>
    set(<span class="hljs-meta">@ColorInt</span> color) {
    field = color
    backgroundPaint.color = color
    invalidate()
}

<span class="hljs-keyword">var</span> batteryHeadColor: <span class="hljs-type">Int</span> = <span class="hljs-type">DEFAULT_BATTERY_HEAD_COLOR</span>
    set(<span class="hljs-meta">@ColorInt</span> color) {
    field = color
    batteryHeadPaint.color = color
    invalidate()
}

<span class="hljs-keyword">var</span> chargingColor: <span class="hljs-type">Int</span> = <span class="hljs-type">DEFAULT_CHARGING_COLOR</span>
    set(<span class="hljs-meta">@ColorInt</span> color) {
    field = color
    chargingLogoPaint.color = color
    invalidate()
}

<span class="hljs-keyword">var</span> textColor: <span class="hljs-type">Int</span> = <span class="hljs-type">DEFAULT_TEXT_COLOR</span>
    set(<span class="hljs-meta">@ColorInt</span> color) {
    field = color
    textValuePaint.color = color
    invalidate()
}

init {
    parseAttr(attrs)

    batteryLevelPaint.apply {
        style = <span class="hljs-type">Paint</span>.<span class="hljs-type">Style</span>.<span class="hljs-type">FILL</span>
        color = batteryLevelColor
    }

    backgroundPaint.apply {
        style = <span class="hljs-type">Paint</span>.<span class="hljs-type">Style</span>.<span class="hljs-type">FILL</span>
        color = backgroundRectColor
    }

    backgroundPaintStroke.apply {
        style = <span class="hljs-type">Paint</span>.<span class="hljs-type">Style</span>.<span class="hljs-type">STROKE</span>
        strokeWidth = <span class="hljs-number">20</span>f
        color = <span class="hljs-type">Color</span>.<span class="hljs-type">BLACK</span>
    }


    batteryHeadPaint.apply {
        style = <span class="hljs-type">Paint</span>.<span class="hljs-type">Style</span>.<span class="hljs-type">FILL</span>
        color = batteryHeadColor
    }

    chargingLogoPaint.apply {
        style = <span class="hljs-type">Paint</span>.<span class="hljs-type">Style</span>.<span class="hljs-type">FILL_AND_STROKE</span>
        color = chargingColor
        strokeWidth = <span class="hljs-number">5</span>f
    }

    textValuePaint.apply {
        textAlign = <span class="hljs-type">Paint</span>.<span class="hljs-type">Align</span>.<span class="hljs-type">CENTER</span>
        color = textColor
    }
}</code></pre>
<p>Before drawing the battery on the screen, we&nbsp;have to update its size and position. The best place to handle any size changes is inside the onSizeChanged method. We can follow these steps:</p>
<ol>
<li>Set the width and height of the content.</li>
<li>Set the text size of battery value to half of the content height.</li>
<li><span>Set the width of the battery head to 1/12 of the total width.&nbsp;</span></li>
<li>Set the background rect position.</li>
<li>Set the battery head rect position</li>
<li>Set the battery level rect position.&nbsp;</li>
</ol>
<p><span>Note: For the purpose of this example, we will use some hardcoded values for the padding and content offset.</span></p>
<pre><code class="{{scope.language}} hljs swift"><span class="hljs-keyword">override</span> fun onSizeChanged(width: <span class="hljs-type">Int</span>, height: <span class="hljs-type">Int</span>, oldw: <span class="hljs-type">Int</span>, oldh: <span class="hljs-type">Int</span>) {
    contentWidth = width - paddingLeft - paddingRight
    contentHeight = height - paddingTop - paddingBottom
    textValuePaint.textSize = contentHeight * <span class="hljs-type">TEXT_SIZE_RATIO</span>
    batteryHeadWidth = (1f / 12f * contentWidth).toInt()
    backgroundRect.<span class="hljs-keyword">set</span>(
        <span class="hljs-number">15</span>,
        <span class="hljs-number">15</span>,
        contentWidth - batteryHeadWidth - <span class="hljs-number">15</span>,
        contentHeight - <span class="hljs-number">15</span>
    )
    batteryHeadRect.<span class="hljs-keyword">set</span>(
        backgroundRect.<span class="hljs-keyword">right</span> + <span class="hljs-number">20</span>,
        backgroundRect.top + contentHeight / <span class="hljs-number">4</span>,
        backgroundRect.<span class="hljs-keyword">left</span> + contentWidth - <span class="hljs-number">20</span>,
        backgroundRect.top + contentHeight * <span class="hljs-number">3</span> / <span class="hljs-number">4</span>
    )
    batteryLevelRect.<span class="hljs-keyword">set</span>(
        backgroundRect.<span class="hljs-keyword">left</span> + mainContentOffset,
        backgroundRect.top + mainContentOffset,
        ((backgroundRect.<span class="hljs-keyword">right</span> - mainContentOffset) *<br>        (this.batteryLevel.toDouble() / <span class="hljs-number">100</span>.toDouble())).toInt(),
        backgroundRect.bottom - mainContentOffset
    )
}</code></pre>
<p>&nbsp;Now to draw the BatteryMeter we’ll start by overriding the onDraw() method.</p>
<ol>
<li>Draw the background of the View.&nbsp;</li>
<li>Draw the battery head.&nbsp;</li>
<li>Draw the container where our battery level will be placed.</li>
<li>Now if the battery is charging, we will draw a charging logo, otherwise, we will draw the text of the current battery value.</li>
</ol>
<p>Keep in mind that the<span>&nbsp;</span><strong>onDraw</strong><span>&nbsp;</span>method is called 60 times per second (60fps) and putting any heavy operations and object creation inside it can cause bad performance in your app. To avoid this, we can create all of the objects inside constructors and if needed we can change the properties later on.</p>
<pre><code class="{{scope.language}} hljs cs"><span class="hljs-function"><span class="hljs-keyword">override</span> fun <span class="hljs-title">onDraw</span>(<span class="hljs-params">canvas: Canvas</span>) </span>{
    <span class="hljs-comment">// Draw the background body of battery view</span>
    drawBackground(canvas)

    <span class="hljs-comment">// Draw the head of battery</span>
    drawBatteryHead(canvas)

    <span class="hljs-comment">// Draw the current battery level</span>
    drawBatteryLevel(canvas)

    <span class="hljs-keyword">if</span> (isCharging) {
        drawChargingLogo(canvas)
    } <span class="hljs-keyword">else</span> {
        drawCurrentBatteryValueText(canvas)
    }
}

<span class="hljs-function"><span class="hljs-keyword">private</span> fun <span class="hljs-title">drawBackground</span>(<span class="hljs-params">canvas: Canvas</span>) </span>{
    canvas.drawRect(backgroundRect, backgroundPaint)
    canvas.drawRoundRect(RectF(backgroundRect), <span class="hljs-number">50</span>f, <span class="hljs-number">50</span>f, backgroundPaintStroke)
}

<span class="hljs-function"><span class="hljs-keyword">private</span> fun <span class="hljs-title">drawBatteryHead</span>(<span class="hljs-params">canvas: Canvas</span>) </span>{
    <span class="hljs-comment">// Draw the head of battery view</span>
    canvas.drawRoundRect(RectF(batteryHeadRect), <span class="hljs-number">10</span>f, <span class="hljs-number">10</span>f, batteryHeadPaint)
}

<span class="hljs-function"><span class="hljs-keyword">private</span> fun <span class="hljs-title">drawBatteryLevel</span>(<span class="hljs-params">canvas: Canvas</span>) </span>{
    <span class="hljs-keyword">if</span> (batteryLevel &lt;= warningLevel) {
        batteryLevelPaint.color = warningColor
    } <span class="hljs-keyword">else</span> {
        batteryLevelPaint.color = batteryLevelColor
    }

    <span class="hljs-keyword">if</span> (batteryLevel == <span class="hljs-number">0</span>) {
        drawEmptyText(canvas)
    } <span class="hljs-keyword">else</span> {
        canvas.drawRoundRect(RectF(batteryLevelRect), <span class="hljs-number">25</span>f, <span class="hljs-number">25</span>f, batteryLevelPaint)
    }
}

<span class="hljs-function"><span class="hljs-keyword">private</span> fun <span class="hljs-title">drawChargingLogo</span>(<span class="hljs-params">canvas: Canvas</span>) </span>{
    VectorDrawableCompat.create(
        context.resources,
        R.drawable.ic_charging_bolt,
        <span class="hljs-literal">null</span>
    )?.apply {
        setBounds(
            backgroundRect.left + contentWidth/<span class="hljs-number">4</span>,
            backgroundRect.top + contentHeight/<span class="hljs-number">4</span>,
            backgroundRect.right - contentWidth/<span class="hljs-number">4</span>,
            backgroundRect.bottom - contentHeight/<span class="hljs-number">4</span>
        )
        setColorFilter(chargingColor,PorterDuff.Mode.SRC_IN)
        draw(canvas)
    }
}

<span class="hljs-function"><span class="hljs-keyword">private</span> fun <span class="hljs-title">drawCurrentBatteryValueText</span>(<span class="hljs-params">canvas: Canvas</span>) </span>{
    val text = <span class="hljs-keyword">if</span> (batteryLevel == <span class="hljs-number">0</span>) <span class="hljs-string">"Empty"</span> <span class="hljs-keyword">else</span> batteryLevel.toString()
    canvas.drawText(
    text,
    (contentWidth * <span class="hljs-number">0.45</span>).toFloat(),
    (contentHeight * <span class="hljs-number">0.7</span>).toFloat(),
    textValuePaint
    )
}

<span class="hljs-function"><span class="hljs-keyword">private</span> fun <span class="hljs-title">drawEmptyText</span>(<span class="hljs-params">canvas: Canvas</span>) </span>{
    canvas.drawText(
        <span class="hljs-string">"Empty"</span>,
        (contentWidth * <span class="hljs-number">0.45</span>).toFloat(),
        (contentHeight * <span class="hljs-number">0.7</span>).toFloat(),
        textValuePaint
    )
}</code></pre>
<p>Now to give our battery the ability to change at runtime we need to call the <strong>invalidate() method</strong> every time we update the View state. What invalidate does is it lets Android know that the view is dirty and that it needs to be redrawn. It is important to note that you need to be careful since calling the&nbsp;<strong>invalidate()</strong> too many times can cause problems.</p>
<pre><code class="{{scope.language}} hljs sql">var isCharging: Boolean = DEFAULT_CHARGING_STATE
    @CheckResult
    get() = field
    <span class="hljs-keyword">set</span>(<span class="hljs-keyword">value</span>) {
    <span class="hljs-keyword">field</span> = <span class="hljs-keyword">value</span>
    <span class="hljs-keyword">invalidate</span>()
}<br>
<span class="hljs-keyword">var</span> batteryLevel: <span class="hljs-built_in">Int</span> = DEFAULT_BATTERY_LEVEL
    @CheckResult
    <span class="hljs-keyword">get</span>() = <span class="hljs-keyword">field</span>
    <span class="hljs-keyword">set</span>(<span class="hljs-keyword">level</span>) {
        <span class="hljs-keyword">field</span> = <span class="hljs-keyword">when</span> {
        <span class="hljs-keyword">level</span> &gt; <span class="hljs-number">100</span> -&gt; <span class="hljs-number">100</span>
        <span class="hljs-keyword">level</span> &lt; <span class="hljs-number">0</span> -&gt; <span class="hljs-number">0</span>
        <span class="hljs-keyword">else</span> -&gt; <span class="hljs-keyword">level</span>
    }
    <span class="hljs-keyword">if</span> (<span class="hljs-keyword">field</span> &lt;= warningLevel) {
        fillPaint.color = warningFillColor
    } <span class="hljs-keyword">else</span> {
        fillPaint.color = normalFillColor
    }
    <span class="hljs-keyword">invalidate</span>()
}</code></pre>
<p>The final step is to add the battery View to your layout like this:</p>
<pre><code class="{{scope.language}} hljs stylus">&lt;ba<span class="hljs-selector-class">.rubicon</span><span class="hljs-selector-class">.widget</span><span class="hljs-selector-class">.BatteryMeterView</span> android:layout_width=<span class="hljs-string">"200dp"</span>
    android:layout_height=<span class="hljs-string">"90dp"</span>
    app:battery_level=<span class="hljs-string">"30"</span>
    android:layout_gravity=<span class="hljs-string">"center"</span>
    app:charging=<span class="hljs-string">"true"</span>/&gt;</code></pre>
<p>All done. There you have it, a Battery Meter that you've created yourself.</p>
<p>For the project source code, you can check out my <a rel="noopener noreferrer" href="https://github.com/rubiconba/android-sample-batterymeter-custom-view" target="_blank" title="https://github.com/rubiconba/android-sample-batterymeter-custom-view">Github.</a></p>
<p>Now that we’ve walked through creating the Battery Meter, I encourage you to try creating your own custom View.&nbsp;It will definitely be fun!</p>
<h2>The Pros &amp; Cons of Implementing Custom Views</h2>
<p>Before concluding, I’d like to share both the pros and cons of implementing custom Views. Just like any other implementation process, there are always both pros and cons but this shouldn’t discourage you from giving it a try.</p>
<p><strong>THE PROS:&nbsp;</strong></p>
<p><strong>Custom look/behaviour</strong></p>
<p>Custom view = Customization. The Android platform is vast but there are specific scenarios where the features or Views of Android don’t meet your needs, therefore, Custom View gives you the opportunity to build something of your own. When it comes to design and interaction, you have complete control since custom View provides endless options.</p>
<p><strong>Reusability/Readability</strong><br>When developing large scale applications, code reusability is always welcome. Once you create a custom View, it can easily be reused in multiple locations across the application.</p>
<p><strong>Performance</strong><br>In specific scenarios, building custom view can squeeze some performance.</p>
<p><strong>THE CONS:</strong></p>
<p><strong>Time &amp; effort</strong><br>Custom views are time-consuming and they can definitely be difficult to use until you get the hang of them.</p>
<p><strong>Diligence</strong><br>There are a number of things you need to be aware of when implementing custom Views. Firstly, you have to ensure that you handle the font, text size, colour, shadows, highlight and style properly.</p>
<p>You also need to make sure that it works properly on all screen densities because Android canvas class draws in pixels not DP. Lastly, if you’re working with images, you’ll have to keep in mind the aspect ratio, zoom, scaling, etc.</p>
<p><strong>Lots to manage.</strong><br>You’ll also have to handle all kinds of click listeners and user interactions — single click, double click, long press, swipe and fling.</p>
<h2>Final Words</h2>
<p>I hope this tutorial has encouraged you to get creative with Android and make your own custom UI. If you have any questions or would like to discuss this topic, I’d be happy to do so.</p>
<p>Good luck coding!</p>
                    </div>

<div class="social-share" style="visibility: hidden; opacity: 0;">
    <span>SHARE</span>

    <a id="facebook">
        <svg width="30px" height="30px" viewBox="0 0 30 30" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
            <!-- Generator: Sketch 46.2 (44496) - http://www.bohemiancoding.com/sketch -->
            <title>facebook</title>
            <desc>Created with Sketch.</desc>
            <defs></defs>
            <g id="Desktop" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd">
                <g id="Blog-Single-Post" transform="translate(-245.000000, -523.000000)" fill-rule="nonzero" fill="#C2C2C2">
                    <g id="Share" transform="translate(238.000000, 494.000000)">
                        <path d="M22,29 C13.7290341,29 7,35.7290341 7,44 C7,52.2703617 13.7290341,59 22,59 C30.2703617,59 37,52.2703617 37,44 C37,35.7290341 30.2715701,29 22,29 Z M25.7303633,44.5280754 L23.2899782,44.5280754 C23.2899782,48.427012 23.2899782,53.2262145 23.2899782,53.2262145 L19.6738097,53.2262145 C19.6738097,53.2262145 19.6738097,48.4735358 19.6738097,44.5280754 L17.9548457,44.5280754 L17.9548457,41.453879 L19.6738097,41.453879 L19.6738097,39.4654395 C19.6738097,38.0413276 20.3505196,35.8160396 23.3232095,35.8160396 L26.0028599,35.8263111 L26.0028599,38.8104809 C26.0028599,38.8104809 24.3745267,38.8104809 24.0579231,38.8104809 C23.7413196,38.8104809 23.2911867,38.9687827 23.2911867,39.6479094 L23.2911867,41.4544832 L26.0463627,41.4544832 L25.7303633,44.5280754 Z" id="facebook"></path>
                    </g>
                </g>
            </g>
        </svg>
    </a>

    <a id="twitter" href="https://twitter.com/intent/tweet?text=Creating Custom Views in Android&amp;via=rubicon_dev&amp;url=https://www.rubicon-world.com/blog/2019/05/creating-custom-views-in-android/&amp;hashtags=rubicondev">
        <svg width="30px" height="30px" viewBox="0 0 30 30" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
            <!-- Generator: Sketch 46.2 (44496) - http://www.bohemiancoding.com/sketch -->
            <title>twitter</title>
            <desc>Created with Sketch.</desc>
            <defs></defs>
            <g id="Desktop" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd">
                <g id="Blog-Single-Post" transform="translate(-245.000000, -568.000000)" fill-rule="nonzero" fill="#C2C2C2">
                    <g id="Share" transform="translate(238.000000, 494.000000)">
                        <path d="M22,74 C13.7290341,74 7,80.7290341 7,89 C7,97.2703617 13.7290341,104 22,104 C30.2703617,104 37,97.2703617 37,89 C37,80.7290341 30.2715701,74 22,74 Z M28.6915734,85.5669057 C28.6982196,85.7155402 28.7018448,85.8653831 28.7018448,86.015226 C28.7018448,90.57758 25.2300814,95.8365826 18.8780714,95.8365826 C16.928301,95.8365826 15.1132683,95.266817 13.5858374,94.2867961 C13.8559172,94.318819 14.1308306,94.3351325 14.4093692,94.3351325 C16.0274309,94.3351325 17.5155885,93.7828889 18.697414,92.8572464 C17.1869008,92.829453 15.9114235,91.8313059 15.4721663,90.4591557 C15.6824297,90.4990333 15.8993394,90.5213889 16.1210827,90.5213889 C16.4358737,90.5213889 16.7416015,90.4803029 17.0310159,90.401152 C15.4516233,90.0845485 14.2619431,88.6894385 14.2619431,87.0157899 C14.2619431,87.001289 14.2619431,86.9861838 14.2625473,86.9722871 C14.7277854,87.2302828 15.2600902,87.3861677 15.8256264,87.4036897 C14.8999839,86.7855877 14.2903408,85.7282285 14.2903408,84.5306936 C14.2903408,83.8974865 14.4601225,83.3041569 14.7573914,82.7942077 C16.4594377,84.8835495 19.0043503,86.2575123 21.8731169,86.4025215 C21.8139048,86.1493595 21.7842987,85.8865303 21.7842987,85.6152421 C21.7842987,83.7089745 23.3298558,82.1628132 25.2361234,82.1628132 C26.2294369,82.1628132 27.1254733,82.5821316 27.7562636,83.2527995 C28.5435431,83.0981229 29.2806735,82.8117296 29.9495287,82.4147668 C29.6897205,83.2213808 29.1441231,83.8974865 28.4293483,84.3252638 C29.1284138,84.2418835 29.7954564,84.0569967 30.41235,83.7820833 C29.9519455,84.4732941 29.3664706,85.0817288 28.6915734,85.5669057 Z" id="twitter"></path>
                    </g>
                </g>
            </g>
        </svg>
    </a>

    <a href="https://www.linkedin.com/shareArticle?mini=true&amp;url=https://www.rubicon-world.com/blog/2019/05/creating-custom-views-in-android/&amp;title=Creating Custom Views in Android&amp;source=/blog/2019/05/creating-custom-views-in-android/" id="linkedin" target="_blank">
        <svg width="30px" height="30px" viewBox="0 0 30 30" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
            <!-- Generator: Sketch 46.2 (44496) - http://www.bohemiancoding.com/sketch -->
            <title>linkedin</title>
            <desc>Created with Sketch.</desc>
            <defs></defs>
            <g id="Desktop" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd">
                <g id="Blog-Single-Post" transform="translate(-245.000000, -613.000000)" fill-rule="nonzero" fill="#C2C2C2">
                    <g id="Share" transform="translate(238.000000, 494.000000)">
                        <path d="M22,119 C13.7162494,119 7,125.716277 7,134.000061 C7,142.283846 13.7162494,149.000122 22,149.000122 C30.2837506,149.000122 37,142.283846 37,134.000061 C37,125.714426 30.2837506,119 22,119 Z M18.25,141.265712 L14.5,141.265712 L14.5,128.140628 L18.25,128.140628 L18.25,141.265712 Z M16.4912334,127.336273 C15.5199798,127.336273 14.7343789,126.548757 14.7343789,125.577499 C14.7343789,124.606242 15.5218918,123.818726 16.4912334,123.818726 C17.462487,123.820576 18.25,124.608092 18.25,125.577499 C18.25,126.548757 17.462487,127.336273 16.4912334,127.336273 Z M31.3749692,141.265712 L27.6249692,141.265712 L27.6249692,133.150681 C27.6249692,132.200024 27.3530897,131.534447 26.1849578,131.534447 C24.2480633,131.534447 23.8749692,133.150681 23.8749692,133.150681 L23.8749692,141.265712 L20.1249692,141.265712 L20.1249692,128.140628 L23.8749692,128.140628 L23.8749692,129.394991 C24.4112033,128.984395 25.7499383,128.142478 27.6249692,128.142478 C28.8399768,128.142478 31.3749692,128.869981 31.3749692,133.264972 L31.3749692,141.265712 Z" id="linkedin"></path>
                    </g>
                </g>
            </g>
        </svg>
    </a>
</div>


                        <div class="tags-container">
                            <ul class="tags">
                                    <li><a href="/blog/tag/android/">Android</a></li>
                                    <li><a href="/blog/tag/kotlin/">Kotlin</a></li>
                            </ul>
                        </div>
                </div>
            </div>
        </div>
    </div>
