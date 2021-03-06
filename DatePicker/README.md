# DatePicker plugin

Forked from https://github.com/phonegap/phonegap-plugins/tree/master/Android/DatePicker

Modified for Steroids by AppGyver.

##Configuration in Steroids

The DatePicker plugin is bundled in with AppGyver Scanner for Android, so there's no need to install it separately. Simply copy the JavaScript files in the `www` directory to your project and load them in your app, e.g. with a `<script src="/plugins/datepicker.js"></script>" tag.

You also need to make sure that your `config.android.xml` file has the correct tag defined:

* config.android.xml:
  `<plugin name="DatePickerPlugin" value="com.phonegap.plugin.DatePickerPlugin"/>`

##Usage

1. Copy the .js file to your 'www' folder
2. Create a package com.phonegap.plugins
3. Copy the .java file into this package
4. Update your res/xml/plugins.xml file with the following line:

   `<plugin name="DatePickerPlugin" value="com.phonegap.plugins.DatePickerPlugin"/>`

5. Add the JavaScript file to your HTML page where you want to use the DatePicker:

   `<script type="text/javascript" charset="utf-8" src="datePickerPlugin.js"></script>`

6. Add a class="nativedatepicker" to your <input> element for which you want the native datepicker
7. Add the following jQuery fragment to handle the click on these input elements:

```
	$('.nativedatepicker').focus(function(event) {
		var currentField = $(this);
		var myNewDate = Date.parse(currentField.val()) || new Date();

		// Same handling for iPhone and Android
		window.plugins.datePicker.show({
			date : myNewDate,
			mode : 'date', // date or time or blank for both
			allowOldDates : true
		}, function(returnDate) {
			var newDate = new Date(returnDate);
			currentField.val(newDate.toString("dd/MMM/yyyy"));

			// This fixes the problem you mention at the bottom of this script with it not working a second/third time around, because it is in focus.
			currentField.blur();
		});
	});

	$('.nativetimepicker').focus(function(event) {
		var currentField = $(this);
		var time = currentField.val();
		var myNewTime = new Date();

		myNewTime.setHours(time.substr(0, 2));
		myNewTime.setMinutes(time.substr(3, 2));

		// Same handling for iPhone and Android
		plugins.datePicker.show({
			date : myNewTime,
			mode : 'time', // date or time or blank for both
			allowOldDates : true
		}, function(returnDate) {
		  // returnDate is generated by .toLocaleString() in Java so it will be relative to the current time zone
			var newDate = new Date(returnDate);
			currentField.val(newDate.toString("HH:mm"));

			currentField.blur();
		});
	});
```

8. It is recommended to prefil these input fields and make these fields read only with a standard date format, preferably with three letter month so it can always be parsed.

9. You may need to convert the result of date.parse() back to an object to get the picker to work a second or third time around. If so, try inserting this code after the myNewDate declaration:

``` 	if(typeof myNewDate === "number"){
		myNewDate = new Date (myNewDate);
	}
	```