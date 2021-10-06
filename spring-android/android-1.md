# Getting Advanced Stuff Right

Starting off, we will begin with basic android concepts before jumping to the meaty parts. Starting off with the App.

Apps are a collection of components working together to produce what we intend to to mobile users? These components are mainly of 4 types:

- Activity: Which holds most logic for user interactivity. `A single focused thing a user can do`.
- Service:
- Broadcast Reciever:
- Content Provider:

## Activity

A typical app does look like a chain of activities, when pressing back the previous one is displayed or back to the 'homepage' of the device.

The manifest does hold more information than what it initially looks like. A few main confiugrations are included related to the general state of the app such as the icon location, label name and RTL support etc.., and including an activty component with an intent-filter to the main launcher which is the homepage of the device.

```xml
  <application
      android:allowBackup="true"
      android:icon="@mipmap/ic_launcher"
      android:label="@string/app_name"
      android:roundIcon="@mipmap/ic_launcher_round"
      android:supportsRtl="true"
      android:theme="@style/Theme.FavouriteToys">
      <activity android:name=".MainActivity">
          <intent-filter>
              <action android:name="android.intent.action.MAIN" />

              <category android:name="android.intent.category.LAUNCHER" />
          </intent-filter>
      </activity>
  </application>
```

### Displayed items

What is contained within an activity is dictated by a user interface `layout` file in the res folder of the project, its written also in XML.
Other 3 folders include:

- `drawable` folder that holds assets shown in layouts, and helps adding custom designs to your views.
- `mipmap` folder in the same res folder tells the app how to display different resource images on different screen sizes.
- `values` folder which holds some global variables of strings and numbers that are otherwise located in layout folder to help adjust to different locale configurations such as language translations. It also helps keeping common values of colors and themes centralized to ease the ability to change it.

In the `layout`, there are multiple main types of views to help organize and display different contents, inputs and outputs. These are:

- UI Components including but not limited to TextView, Button and etc..
- ViewGroup containers and is very similar to a div in HTML, they could arrage items `linearly` in a horizontal or vertical fashion, or be automatically placed depending on multiple `constraints`.

Of which all of these are defined with properties using `XML Attributes`, such as `width` and `height` attributes which can use fixed or relative values such as pixels (px) or scaleable pixels (sp) for text content or density-independant pixels (dp) for everything else, or even inherited from parent values to either fill or be a percentage of such views.

`setContentView()` method is what connects the activity to our XML file, and thus `inflates` it into existance.
