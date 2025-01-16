# UnusedResources is not being run on library modules

This project has two modules `:app` and `:mylibrary`. The former applies `com.android.application` plugin; the later `com.application.library`


Each module has an unused string resource.

`./gradlew app:lint` detects the error in `app` module.


```
> Task ':app:lintDebug'.
> Lint found errors in the project; aborting build.

  ROOT_DIR/app/src/main/res/values/strings.xml:3: Error: The resource R.string.some_res appears to be unused [UnusedResources]
    <string name="some_res">Some Res</string>
```

`/.gradlew mylibrary:lint` does not report anything. 

Just to ensure lint is running _at all_ on the library module, I include a HardcodedText violation as well. That lint error is reported just fine:

```
> Task :mylibrary:lintDebug FAILED
ROOT_DIR/mylibrary/src/main/res/layout/some_layout.xml:10: Error: Hardcoded string "some hard coded string", should use @string resource [HardcodedText]
      android:text="some hard coded string"
```

To further narrow down the issue, I add `checkDependencies = true` to the `app` module and run lint again on the `app` module. This time lint reports the `app` and `mylibrary` unused resources ?!?!
