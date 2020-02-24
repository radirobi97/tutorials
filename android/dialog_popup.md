# Creating dialog activity above another given activity

```xml
<activity
    android:name=".[ActivityName]"
    android:label="@string/title_activity"
    android:parentActivityName=".[ParentActivity]"
    android:theme="@style/Theme.AppCompat.Light.Dialog">
    <meta-data
        android:name="android.support.PARENT_ACTIVITY"
        android:value="[package.To.MainActivity]" />
</activity>
```
Theme should be **@style/Theme.AppCompat.Light.Dialog**.
