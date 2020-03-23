# Fragment2Fragment communication
- create layouts files of fragments
- create fragment classes which inherits from `Fragment`
- create interfaces in fragments
- activity must override the interface methods

#### Layout of `mainActivity`
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <EditText
        android:id="@+id/etMessage"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"/>

    <Button
        android:id="@+id/btStartFragment"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="START FRAGMENT"
        app:layout_constraintTop_toBottomOf="@id/etMessage"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"/>

    <FrameLayout
        android:id="@+id/fragmentAContainer"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        app:layout_constraintTop_toBottomOf="@id/btStartFragment"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintBottom_toTopOf="@id/fragmentBContainer"/>

    <FrameLayout
        android:id="@+id/fragmentBContainer"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        app:layout_constraintTop_toBottomOf="@id/fragmentAContainer"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintBottom_toBottomOf="parent"/>


</androidx.constraintlayout.widget.ConstraintLayout>
```

##### Layout of `fragmentFirst`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:background="@color/colorAccent">

    <TextView
        android:id="@+id/tvFirst"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="FIRST FRAGMENT"
        android:textSize="40dp"
        android:layout_gravity="center"/>

    <EditText
        android:id="@+id/etFirst"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:hint="type in here"
        android:layout_gravity="center_horizontal"/>

    <Button
        android:id="@+id/btFirst"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="SEND"
        android:layout_gravity="center_horizontal"/>
</LinearLayout>
```

##### Layout of `fragmentSecond`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:background="@color/colorPrimaryDark">

    <TextView
        android:id="@+id/tvSec"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="SECOND FRAGMENT"
        android:textSize="40dp"
        android:layout_gravity="center"/>

    <EditText
        android:id="@+id/etSec"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:hint="type in here"
        android:layout_gravity="center_horizontal"/>

    <Button
        android:id="@+id/btSec"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="SEND"
        android:layout_gravity="center_horizontal"/>
</LinearLayout>
```

##### Kotlin file of `fragmentFirst`
```java
class FirstFragment: Fragment(){

    private var mFirstFragmentListener: FragmentFirstListener? = null

    companion object{
        const val TAG = "FIRST_FRAGMENT"

        fun newInstance(): FirstFragment = FirstFragment()
    }

    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {

        return inflater.inflate(R.layout.fragment_first, container, false)
    }

    override fun onAttach(context: Context) {
        super.onAttach(context)
        if (context is FragmentFirstListener) {
            mFirstFragmentListener = context
        } else {
            throw RuntimeException(context!!.toString() + " must implement FragmentFirstListener")
        }
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        btFirst.setOnClickListener {
            mFirstFragmentListener?.textSentFromFragmentA(etFirst.text.toString())
        }
    }

    override fun onDetach() {
        super.onDetach()
        mFirstFragmentListener = null
    }

    fun setText(text: String){
        tvFirst.text = text
    }

    interface FragmentFirstListener{
        fun textSentFromFragmentA(input: String)
    }
}
```
##### Kotlin file of `fragmentSecond`
```java
class SecondFragment: Fragment(){

    private var mSecondFragmentListener: FragmentSecondListener? = null

    companion object{
        const val TAG = "SECOND_FRAGMENT"

        fun newInstance(): SecondFragment = SecondFragment()
    }

    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {

        return inflater.inflate(R.layout.fragment_second, container, false)
    }

    override fun onAttach(context: Context) {
        super.onAttach(context)
        if (context is FragmentSecondListener) {
            mSecondFragmentListener = context
        } else {
            throw RuntimeException(context!!.toString() + " must implement FragmentSecondListener")
        }
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        btSec.setOnClickListener {
            mSecondFragmentListener?.textSentFromFragmentB(etSec.text.toString())
        }
    }

    override fun onDetach() {
        super.onDetach()
        mSecondFragmentListener = null
    }

    fun setText(text: String){
        tvSec.text = text
    }

    interface FragmentSecondListener {
        fun textSentFromFragmentB(input: String)
    }
}
```

##### MainActivity class
```java
class MainActivity : AppCompatActivity(), FirstFragment.FragmentFirstListener, SecondFragment.FragmentSecondListener{

    var mSecondFragment: SecondFragment? = null
    var mFirstFragment: FirstFragment? = null

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        btStartFragment.setOnClickListener {
            Toast.makeText(this, "was clicked", Toast.LENGTH_SHORT).show()
            var ft = supportFragmentManager.beginTransaction()
            mFirstFragment = FirstFragment()
            ft.replace(R.id.fragmentAContainer, mFirstFragment as Fragment, FirstFragment.TAG).commit()

            ft = supportFragmentManager.beginTransaction()
            mSecondFragment = SecondFragment()
            ft.replace(R.id.fragmentBContainer, mSecondFragment as Fragment, SecondFragment.TAG).commit()
        }
    }

    override fun textSentFromFragmentA(input: String) {
        mSecondFragment?.setText(input)
    }

    override fun textSentFromFragmentB(input: String) {
        mFirstFragment?.setText(input)
    }
}

```
