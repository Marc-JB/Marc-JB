# Android project guidelines
**THIS IS A WIP**  
Based on https://github.com/ribot/android-guidelines/blob/master/project_and_code_guidelines.md

## 1 Project structure
New projects should follow the Android Gradle project structure that is defined on the [Android Gradle plugin user guide](http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-Project-Structure).

## 2 File naming
### 2.1 Class files
Class names are written in [UpperCamelCase](http://en.wikipedia.org/wiki/CamelCase).

For classes that extend an Android component, the name of the class should end with the name of the component; for example: `SignInActivity`, `SignInFragment`, `ImageUploaderService`, `ChangePasswordDialog`.

### 2.2 Resources files
Resources file names are written in __snake_case__.

#### 2.2.1 Drawable files
Naming conventions for drawables:

Asset Type   | Prefix            | Example               
------------ | ----------------- | ----------------------------
Action bar   | `ab_`             | `ab_stacked.9.png`          
Button       | `btn_`	         | `btn_send_pressed.9.png`    
Dialog       | `dialog_`         | `dialog_top.9.png`          
Divider      | `divider_`        | `divider_horizontal.9.png`  
Icon         | `ic_`	         | `ic_star.png`               
Menu         | `menu_	`        | `menu_submenu_bg.9.png`     
Notification | `notification_`	 | `notification_bg.9.png`     
Tabs         | `tab_`            | `tab_pressed.9.png`         

Naming conventions for icons (taken from [Android iconography guidelines](http://developer.android.com/design/style/iconography.html)):
Asset Type                      | Prefix             | Example                     
------------------------------- | ------------------ | ----------------------------
Icons                           | `ic_`              | `ic_star.png`               
Launcher icons                  | `ic_launcher`      | `ic_launcher_calendar.png`  
Menu icons and Action Bar icons | `ic_menu`          | `ic_menu_archive.png`       
Status bar icons                | `ic_stat_notify`   | `ic_stat_notify_msg.png`    
Tab icons                       | `ic_tab`           | `ic_tab_recent.png`         
Dialog icons                    | `ic_dialog`        | `ic_dialog_info.png`        

Naming conventions for selector states:
State	     | Suffix          | Example                    
------------ | --------------- | ---------------------------
Normal       | `_normal`       | `btn_order_normal.9.png`   
Pressed      | `_pressed`      | `btn_order_pressed.9.png`  
Focused      | `_focused`      | `btn_order_focused.9.png`  
Disabled     | `_disabled`     | `btn_order_disabled.9.png` 
Selected     | `_selected`     | `btn_order_selected.9.png` 

#### 2.2.2 Layout files
Layout files should match the name of the Android components that they are intended for but moving the top level component name to the beginning. For example, if we are creating a layout for the `SignInActivity`, the name of the layout file should be `activity_sign_in.xml`.

Component        | Class Name             | Layout Name                  
---------------- | ---------------------- | -----------------------------
Activity         | `UserProfileActivity`  | `activity_user_profile.xml`  
Fragment         | `SignUpFragment`       | `fragment_sign_up.xml`       
Dialog           | `ChangePasswordDialog` | `dialog_change_password.xml` 
AdapterView item | ---                    | `item_person.xml`            
Partial layout   | ---                    | `partial_stats_bar.xml`      

A slightly different case is when we are creating a layout that is going to be inflated by an `Adapter`, e.g to populate a `ListView`. In this case, the name of the layout should start with `item_`.

Note that there are cases where these rules will not be possible to apply. For example, when creating layout files that are intended to be part of other layouts. In this case you should use the prefix `partial_`.

#### 2.2.3 Menu files
Similar to layout files, menu files should match the name of the component. For example, if we are defining a menu file that is going to be used in the `UserActivity`, then the name of the file should be `activity_user.xml`

A good practice is to not include the word `menu` as part of the name because these files are already located in the `menu` directory.

#### 2.2.4 Values files
Resource files in the values folder should be __plural__, e.g. `strings.xml`, `styles.xml`, `colors.xml`, `dimens.xml`, `attrs.xml`

# 3 Code guidelines
## 3.1 Composables
Names of Composables are written in [UpperCamelCase](http://en.wikipedia.org/wiki/CamelCase).

## 3.2 Fields definition and naming
Fields should be defined at the __top of the file__ and they should follow the naming rules listed below.

* Static final fields (constants, in Kotlin: `const val`) are ALL_CAPS_WITH_UNDERSCORES.
* Other fields start with a lower case letter.

Example:
```Kotlin
class Phone(var batteryLevel: Int) {
    var isCharging = false

    companion object {
        const val BATTERY_FULL = 100

        const val BATTERY_EMPTY = 0
    }
}
```

## 3.3 Treat acronyms as words
Good                    | Bad           
----------------------- | -----------------------
`class XmlHttpRequest`  | `class XMLHTTPRequest`
`fun getCustomerId()`   | `fun getCustomerID()` 
`url: String`           | `URL: String`    
`id: Long`              | `ID: Long`       

## 3.4 Use spaces for indentation
Use __4 space__ indents for blocks:
```Kotlin
if (counter == 1) {
    counter++
}
```

Use __8 space__ indents for line wraps:
```Kotlin
val foo: Bar =
        someLongExpression(that, wouldNotFit, on, one, line)
```

## 3.5 Use standard brace style
Braces go on the same line as the code before them.

```Kotlin
class MyClass {
    fun executeSomeAction(): Int {
        if (something) {
            // ...
        } else if (somethingElse) {
            // ...
        } else {
            // ...
        }
    }
}
```

Braces around the statements are required unless the condition and the body fit on one line.

If the condition and the body fit on one line and that line is shorter than the max line length, then braces are not required, e.g.
```Kotlin
if (condition) body()
```

This is __not recommeded__:
```Kotlin
if (condition)
    body()
```

## 3.6 Annotation style
__Classes, Methods and Constructors__
When annotations are applied to a class, method, or constructor, they are listed after the documentation block and should appear as __one annotation per line__.
```Kotlin
/* This is the documentation block about the class */
@AnnotationA
@AnnotationB
class MyAnnotatedClass { }
```

__Fields__
Annotations applying to fields __should__ be listed __on the same line__, unless the line reaches the maximum line length.
```Kotlin
@Nullable @Mock val dataManager: DataManager
```

## 3.7 Limit variable scope
_The scope of local variables should be kept to a minimum (Effective Java Item 29). By doing so, you increase the readability and maintainability of your code and reduce the likelihood of error. Each variable should be declared in the innermost block that encloses all uses of the variable._

_Local variables should be declared at the point they are first used. Nearly every local variable declaration should contain an initializer. If you don't yet have enough information to initialize a variable sensibly, you should postpone the declaration until you do._ - ([Android code style guidelines](https://source.android.com/source/code-style.html#limit-variable-scope))

## 3.8 Order import statements
If you are using an IDE such as Android Studio, you don't have to worry about this because your IDE is already obeying these rules. If not, have a look below.

The ordering of import statements is:

1. Android imports
2. Imports from third parties (com, junit, net, org)
3. java and javax
4. Same project imports

To exactly match the IDE settings, the imports should be:

* Alphabetically ordered within each grouping, with capital letters before lower case letters (e.g. Z before a).
* There should be a blank line between each major grouping (android, com, junit, net, org, java, javax).

More info [here](https://source.android.com/source/code-style.html#limit-variable-scope)

## 3.9 Class member ordering
There is no single correct solution for this but using a __logical__ and __consistent__ order will improve code learnability and readability. It is recommendable to use the following order:

1. Fields
2. Constructors (may be inline with class definition, like ```class Foo(val bar: Baz)```)
3. Override methods and callbacks (public or private)
4. Public methods
5. Private methods
6. Inner classes, interfaces and (companion) objects

If your class is extending an __Android component__ such as an Activity or a Fragment, it is a good practice to order the override methods so that they __match the component's lifecycle__. For example, if you have an Activity that implements `onCreate()`, `onDestroy()`, `onPause()` and `onResume()`, then the correct order is:

```Kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate() {}

    override fun onResume() {}

    override fun onPause() {}

    override fun onDestroy() {}
}
```

## 3.10 Parameter ordering in methods
When programming for Android, it is quite common to define methods that take a `Context`. If you are writing a method like this, then the __Context__ must be the __first__ parameter.

The opposite case are __callback__ interfaces that should always be the __last__ parameter.

Examples:
```Kotlin
// Context always goes first
suspend fun loadUser(context: Context, userId: Int): User

// Callbacks always go last
fun loadUserCallback(context: Context, userId: Int, callback: (User) -> Unit)
```

## 3.11 String constants, naming, and values
Many elements of the Android SDK such as `SharedPreferences`, `Bundle`, or `Intent` use a key-value pair approach so it's very likely that even for a small app you end up having to write a lot of String constants.

When using one of these components, you __must__ define the keys as a `const val` field and they should be prefixed as indicated below.
Element            | Field Name Prefix
------------------ | -----------------
SharedPreferences  | `PREF_`            
Bundle             | `BUNDLE_`          
Fragment Arguments | `ARGUMENT_`        
Intent Extra       | `EXTRA_`           
Intent Action      | `ACTION_`          

Note that the arguments of a Fragment - `Fragment.arguments` - are also a Bundle. However, because this is a quite common use of Bundles, we define a different prefix for them.

Example:

```Kotlin
// Note the value of the field is the same as the name to avoid duplication issues
const val PREF_EMAIL = "PREF_EMAIL"
const val BUNDLE_AGE = "BUNDLE_AGE"
const val ARGUMENT_USER_ID = "ARGUMENT_USER_ID"

// Intent-related items use full package name as value
const val EXTRA_SURNAME = "com.myapp.extras.EXTRA_SURNAME"
const val ACTION_OPEN_USER = "com.myapp.action.ACTION_OPEN_USER"
```

## 3.12 Arguments in Fragments and Activities
When data is passed into an `Activity` or `Fragment` via an `Intent` or a `Bundle`, the keys for the different values __must__ follow the rules described in the section above.

When an `Activity` or `Fragment` expects arguments, it should provide a `companion object` method that facilitates the creation of the relevant `Intent` or `Fragment`.

In the case of Activities the method is usually called `getStartIntent()`:

```Kotlin
class ThisActivity {
    companion object {
        fun getStartIntent(context: Context, user: User): Intent {
            return Intent(context, ThisActivity::class.java).also {
                it.putParcelableExtra(EXTRA_USER, user)
            }
        }
    }
}
```

For Fragments it is named `newInstance()` and handles the creation of the Fragment with the right arguments:

```Kotlin
class UserFragment {
    companion object {
        fun newInstance(user: User): UserFragment {
            return UserFragment().also { fragment -> 
                fragment.arguments = Bundle().also { args ->
                    args.putParcelable(ARGUMENT_USER, user)
                }
            }
        }
    }
}
```

__Note__: If we provide the methods described above, the keys for extras and arguments should be `private` because there is not need for them to be exposed outside the class.

## 3.13 Line-wrapping strategies
There isn't an exact formula that explains how to line-wrap and quite often different solutions are valid. However there are a few rules that can be applied to common cases.

__Break at operators__
When the line is broken at an operator, the break comes __before__ the operator. For example:

```Kotlin
val longName: Int = anotherVeryLongVariable + anEvenLongerOne - thisRidiculousLongOne
        + theFinalOne
```

__Assignment Operator Exception__
An exception to the `break at operators` rule is the assignment operator `=`, where the line break should happen __after__ the operator.

```Kotlin
val longName: Int =
        anotherVeryLongVariable + anEvenLongerOne - thisRidiculousLongOne + theFinalOne
```

__Method chain case__
When multiple methods are chained in the same line - for example when using Builders - every call to a method should go in its own line, breaking the line before the `.`

```Kotlin
Picasso.with(context).load("https://example.com/favicon.ico").into(imageView)
```

```Kotlin
Picasso.with(context)
        .load("https://example.com/favicon.ico")
        .into(imageView);
```

Or: 

```Kotlin
Picasso
        .with(context)
        .load("https://example.com/favicon.ico")
        .into(imageView);
```

__Long parameters case__
When a method has many parameters or its parameters are very long, each parameter should be indented on a new line.

```Kotlin
loadPicture(context, "https://example.com/favicon.ico", imageViewProfilePicture, clickListener, "Title of the picture");
```

```Kotlin
loadPicture(
    context,
    "https://example.com/favicon.ico",
    imageViewProfilePicture,
    clickListener,
    "Title of the picture"
)
```
