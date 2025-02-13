# Convert a Google Spreadsheet to a localization file


## Installation
	npm install localize-with-spreadsheet-pro

Supports
1. iOS, Android, json, js(i18n import file) 
2. file splits from inline comments (only supports android)

## Example
Given a Google Spreadsheet like this:  
![Spreadsheet example](https://github.com/JerrySir/localize-spreadsheet-bot-pro/raw/master/doc/spreadsheet-example.png)

Right now only good for android XML files where there comments with xxxx.xml and they will be split automatically.

The tool fetch the spreadsheet and write the result to a file in the Android or iOS format:

![Result android](https://github.com/JerrySir/localize-spreadsheet-bot-pro/raw/master/doc/result-android.png) ![Result iOS](https://github.com/JerrySir/localize-spreadsheet-bot-pro/raw/master/doc/result-ios.png)

Create a file update-localization.js

	var Localize = require("localize-with-spreadsheet-pro");
    var transformer = Localize.fromGoogleSpreadsheet("0Aq6WlQdq71FydDZlaWdmMEUtc2tUb1k2cHRBS2hzd2c", '*');
    transformer.setKeyCol('KEY');

    transformer.save("values/strings.xml", { valueCol: "NL", format: "android" });
    transformer.save("values-fr/strings.xml", { valueCol: "FR", format: "android" });

    transformer.save("nl.lproj/Localizable.strings", { valueCol: "NL", format: "ios" });
    transformer.save("fr.lproj/Localizable.strings", { valueCol: "FR", format: "ios" });

    transformer.save("values/strings.xml", { valueCol: "NL", format: "android" }, function(){
            transformer.programSplit ("values/" + "strings.xml", "values", "ZH");
    });
    
    // for i18n.js
    transformer.save('values/en.js', { valueCol: 'EN', format: 'js' });

Run it with

    node update-localization.js

## Advanced
You can filter the worksheets to include with the second parameter of 'fromGoogleSpreadsheet'
Ex:

    Localize.fromGoogleSpreadsheet("<Key>", '*');
    Localize.fromGoogleSpreadsheet("<Key>", ['HomeScreen, 'ContactScreen']);
    Localize.fromGoogleSpreadsheet("<Key>", [0, 2]);

## Notes
- The script will preserve everything that is above the tags: < !-- AUTO-GENERATED --> or // AUTO-GENERATED
- Your spreadsheet should be "Published" for this to work
- You need to have git installed for the installation