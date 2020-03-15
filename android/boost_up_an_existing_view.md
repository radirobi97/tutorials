# Boosted view
### Boosting demonstrated via password edittext with eye button

**Step 1.**
- define needed elements in an xml file
- `merge` tag helps avoid duplicates viewgroups
- parent element is defined in the kotlin code using inheritance

```xml
<?xml version="1.0" encoding="utf-8"?>
<merge xmlns:android="http://schemas.android.com/apk/res/android">

    <ImageView
        android:id="@+id/ivPassword"
        android:layout_width="50dp"
        android:layout_height="50dp"
        android:layout_alignParentEnd="true"
        android:layout_centerVertical="true"
        android:src="@android:drawable/ic_menu_view" />

    <EditText
        android:id="@+id/etPassword"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_alignParentStart="true"
        android:layout_centerVertical="true"
        android:layout_toStartOf="@+id/ivPassword" />

```

**Step 2.**
- kotlin file our custom view
- views can be instanciated via many consructors so we also use them
- inflate the layout in the `init`
- set transformation method in `setTransformationMethod`
- keep track of selection of the text in `setTransformationMethod`

```java
@SuppressLint("ClickableViewAccessibility")
class PasswordEditText : RelativeLayout {

    constructor(context: Context) : super(context)
    constructor(context: Context?, attrs: AttributeSet?) : super(context, attrs)
    constructor(context: Context?, attrs: AttributeSet?, defStyleAttr: Int) : super(context, attrs, defStyleAttr)

    init {
        LayoutInflater.from(context).inflate(R.layout.view_password_edittext, this, true)

        ivPassword.setOnTouchListener { view, motionEvent ->
            when (motionEvent.action) {
                MotionEvent.ACTION_DOWN -> {
                    setTransformationMethod(null)
                    true
                }
                MotionEvent.ACTION_UP, MotionEvent.ACTION_CANCEL -> {
                    setTransformationMethod(PasswordTransformationMethod.getInstance())
                    true
                }
                else -> false
            }
        }

        setTransformationMethod(PasswordTransformationMethod.getInstance())
    }

    private fun setTransformationMethod(method: TransformationMethod?) {
        val ss = etPassword.selectionStart
        val se = etPassword.selectionEnd
        etPassword.transformationMethod = method
        etPassword.setSelection(ss, se)
    }
}
```
