# Choice layout using selectors with custom attributes

#### Selectors
Selectors are kind of drawable resources which depend on state of the view. In many LayoutGroups `android:clickable="true"` should be placed.

**Step 1.**
- create an xml file in *res/drawable* folder, this describe which color has to be used in which state
- between changes states animation can be defined with `enterFadeDuration/exitFadeDuration`
```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android" android:enterFadeDuration="100" android:exitFadeDuration="100">
    <item android:drawable="@color/choiceItemPressedBackground" android:state_pressed="true" />
    <item android:drawable="@color/choiceItemActiveBackground" android:state_selected="true" />
    <item android:drawable="@color/choiceItemBackground" />
</selector>
```

**Step 2.**
- creating a style which uses the selector above as background
```xml
<style name="ChoiceOptionStyle">
    <item name="android:gravity">center</item>
    <item name="android:textSize">16sp</item>
    <item name="android:paddingTop">10dp</item>
    <item name="android:paddingBottom">10dp</item>
    <item name="android:background">@drawable/selector_choice_item</item>
</style>
```

**Step 3.**
- creating an attribute which can be modifed in the XML file.
- creating **attrs.xml** in res/values

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <declare-styleable name="ChoiceLayout">
        <attr name="multiple" format="integer" />
    </declare-styleable>
</resources>
```

**Step 4.**
- class code that implements the logic
- choices are just elements under eachother, so we extends from **LinearLayout**
- parent' constructors called
- `init` is the function where we:
  - define orientation of the linear layout
  - read out attributes using `context.obtainStyledAttributes(attrs, R.styleable.ChoiceLayout)`
  - we can get out the proper attribute with `attributes.getInt(R.styleable.ChoiceLayout_multiple, 1)`
  - after that calling `attributes.recycle()` is a must which frees up the used attributes
- `addView`: this is the function which is called when we add a new element to the layout
- `refreshAfterAdd`: makes the views clickable

  ```java
  class ChoiceLayout : LinearLayout {

    private var multiple: Int = 1

    constructor(context: Context) : super(context, null) {
        init(context, null)
    }

    constructor(context: Context, attrs: AttributeSet?) : super(context, attrs) {
        init(context, attrs)
    }

    constructor(context: Context, attrs: AttributeSet?, defStyleAttr: Int) : super(context, attrs, defStyleAttr) {
        init(context, attrs)
    }

    private fun init(context: Context, attrs: AttributeSet?) {
        orientation = VERTICAL

        if (attrs == null) {
            return
        }

        val attributes = context.obtainStyledAttributes(attrs, R.styleable.ChoiceLayout)
        try {
            multiple = attributes.getInt(R.styleable.ChoiceLayout_multiple, 1)
        } finally {
            attributes.recycle()
        }
    }

    override fun addView(child: View) {
        super.addView(child)
        refreshAfterAdd(child)
    }

    override fun addView(child: View, params: ViewGroup.LayoutParams?) {
        super.addView(child, params)
        refreshAfterAdd(child)
    }

    private fun getSelectedCount(): Int {
        var selectedCount = 0
        for (i in 0 until childCount) {
            if (getChildAt(i).isSelected) {
                selectedCount++
            }
        }
        return selectedCount
    }

    private fun refreshAfterAdd(child: View) {
        child.isClickable = true
        child.setOnClickListener { view ->
            if (multiple > 1) {
                if (view.isSelected || getSelectedCount() < multiple) {
                    view.isSelected = !view.isSelected
                }
            } else {
                for (i in 0 until childCount) {
                    val v = getChildAt(i)
                    v.isSelected = (v == view)
                }
            }
        }
    }
}
  ```
