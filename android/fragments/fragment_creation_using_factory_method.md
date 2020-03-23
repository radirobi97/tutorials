# Fragment creation using factory method
- newInstance method implementation
- arguments should be checked if null, see in **onViewCreated** method

```java
class DetailFragment(): Fragment() {

    companion object {
        //TAG which holds the class name
        const val TAG="DetailFragment"

        //name of the parameter in the Bundle
        private const val NAME="NAME"

        fun newInstance(name:String):DetailFragment{
            val fragment=DetailFragment()
            val bundle=Bundle()
            bundle.putString(NAME,name)

            fragment.arguments=bundle
            return  fragment
        }
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
            super.onViewCreated(view, savedInstanceState)
            //THIS IS THE CHECKING STEP
            val name=arguments!!.getString(NAME)

            nameTextView.text="Hello $name"
    }
}
```
