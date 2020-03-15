#change style of a given view

**Step 1.**
- go in **styles.xml**

**Step 2.**
- Overwrite in the Theme the proper view, in the following we overwrite `textViewStyle`
```xml
<style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
    <item name="android:textViewStyle">@style/TextView</item>
</style>
```

**Step 3.**
```xml
<style name="TextView" parent="android:Widget.TextView">
    <item name="android:textColor">@color/primary</item>
</style>
```
