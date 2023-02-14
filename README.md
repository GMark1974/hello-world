Skip to content
Search or jump to…
Pull requests
Issues
Codespaces
Marketplace
Explore
 
@GMark1974 
kerrongordon
/
treminal
Public
Fork your own copy of kerrongordon/treminal
Code
Issues
Pull requests
Actions
Projects
Security
Insights
mvp
 main
@kerrongordon
kerrongordon committed 2 weeks ago 
1 parent e2b5868
commit b722d97
Show file tree Hide file tree
Showing 77 changed files with 661 additions and 244 deletions.
Filter changed files
  38  
android/app/build.gradle
@@ -6,6 +6,12 @@ if (localPropertiesFile.exists()) {
    }
}

def keystoreProperties = new Properties()
def keystorePropertiesFile = rootProject.file('key.properties')
if (keystorePropertiesFile.exists()) {
    keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
}

def flutterRoot = localProperties.getProperty('flutter.sdk')
if (flutterRoot == null) {
    throw new GradleException("Flutter SDK not found. Define location with flutter.sdk in the local.properties file.")
@@ -43,21 +49,39 @@ android {
    }

    defaultConfig {
        // TODO: Specify your own unique Application ID (https://developer.android.com/studio/build/application-id.html).
        applicationId "com.kgpord.busterminal.prototypes"
        // You can update the following values to match your application needs.
        // For more information, see: https://docs.flutter.dev/deployment/android#reviewing-the-build-configuration.
        applicationId "com.kgpord.busterminal"
        minSdkVersion flutter.minSdkVersion
        targetSdkVersion flutter.targetSdkVersion
        versionCode flutterVersionCode.toInteger()
        versionName flutterVersionName
        multiDexEnabled true
    }

    if (keystorePropertiesFile.exists()) {
        signingConfigs {
            release {
                keyAlias keystoreProperties['keyAlias']
                keyPassword keystoreProperties['keyPassword']
                storeFile file(keystoreProperties['storeFile'])
                storePassword keystoreProperties['storePassword']
            }
        }
    }

    buildTypes {
        release {
            // TODO: Add your own signing config for the release build.
            // Signing with the debug keys for now, so `flutter run --release` works.
            signingConfig signingConfigs.debug
            if (keystorePropertiesFile.exists()) {
                signingConfig signingConfigs.release
                println '🎉️ <== Release ==> Signing with the release keys'
            } else {
                signingConfig signingConfigs.debug
                println '🤪️ <== Debug ==> Signing with the debug keys'
            }

            minifyEnabled true
            // useProguard true

            // proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}
 7  
android/app/proguard-rules.pro
@@ -0,0 +1,7 @@
#Flutter Wrapper
-keep class io.flutter.app.** { *; }
-keep class io.flutter.plugin.**  { *; }
-keep class io.flutter.util.**  { *; }
-keep class io.flutter.view.**  { *; }
-keep class io.flutter.**  { *; }
-keep class io.flutter.plugins.**  { *; }
 BIN -3.56 KB (20%) 
android/app/src/main/res/drawable-hdpi/ic_launcher_background.png

 BIN +3.07 KB (150%) 
android/app/src/main/res/drawable-hdpi/ic_launcher_foreground.png

 BIN -1.02 KB (41%) 
android/app/src/main/res/drawable-mdpi/ic_launcher_background.png

 BIN +1.82 KB (150%) 
android/app/src/main/res/drawable-mdpi/ic_launcher_foreground.png

 BIN -6.41 KB (14%) 
android/app/src/main/res/drawable-xhdpi/ic_launcher_background.png

 BIN +4.41 KB (150%) 
android/app/src/main/res/drawable-xhdpi/ic_launcher_foreground.png

 BIN -20 KB (6.5%) 
android/app/src/main/res/drawable-xxhdpi/ic_launcher_background.png

 BIN +8.33 KB (150%) 
android/app/src/main/res/drawable-xxhdpi/ic_launcher_foreground.png

 BIN -28.6 KB (6.7%) 
android/app/src/main/res/drawable-xxxhdpi/ic_launcher_background.png

 BIN +13.5 KB (150%) 
android/app/src/main/res/drawable-xxxhdpi/ic_launcher_foreground.png

 BIN -3.18 KB (45%) 
android/app/src/main/res/mipmap-hdpi/launcher_icon.png

 BIN -1.6 KB (52%) 
android/app/src/main/res/mipmap-mdpi/launcher_icon.png

 BIN -4.88 KB (43%) 
android/app/src/main/res/mipmap-xhdpi/launcher_icon.png

 BIN -9.89 KB (39%) 
android/app/src/main/res/mipmap-xxhdpi/launcher_icon.png

 BIN -14.1 KB (40%) 
android/app/src/main/res/mipmap-xxxhdpi/launcher_icon.png

 1  
android/gradle.properties
@@ -1,3 +1,4 @@
org.gradle.jvmargs=-Xmx1536M
android.useAndroidX=true
android.enableJetifier=true
android.enableR8=true
 BIN -399 KB 
assets/icon/1024.png
Binary file not shown.
 BIN +12.4 KB 
assets/icon/adaptive_icon_background.png

 BIN +202 KB 
assets/icon/adaptive_icon_foreground.png

 BIN -25.9 KB 
assets/icon/ic_launcher.png
Binary file not shown.
 BIN -28.8 KB 
assets/icon/ic_launcher_adaptive_back.png
Binary file not shown.
 BIN -35.7 KB 
assets/icon/ic_launcher_adaptive_fore.png
Binary file not shown.
 BIN +175 KB 
assets/icon/image_path.png

 1  
assets/lottie/39612-location-animation.json
Large diffs are not rendered by default.

 BIN -41.7 KB (78%) 
ios/Runner/Assets.xcassets/AppIcon.appiconset/Icon-App-1024x1024@1x.png

 BIN -47 Bytes (95%) 
ios/Runner/Assets.xcassets/AppIcon.appiconset/Icon-App-20x20@1x.png

 BIN -755 Bytes (64%) 
ios/Runner/Assets.xcassets/AppIcon.appiconset/Icon-App-20x20@2x.png

 BIN -1.57 KB (56%) 
ios/Runner/Assets.xcassets/AppIcon.appiconset/Icon-App-20x20@3x.png
Unable to render rich display

 BIN -334 Bytes (76%) 
ios/Runner/Assets.xcassets/AppIcon.appiconset/Icon-App-29x29@1x.png
Unable to render rich display

 BIN -1.41 KB (58%) 
ios/Runner/Assets.xcassets/AppIcon.appiconset/Icon-App-29x29@2x.png
Unable to render rich display

 BIN -2.73 KB (54%) 
ios/Runner/Assets.xcassets/AppIcon.appiconset/Icon-App-29x29@3x.png
Unable to render rich display

 BIN -755 Bytes (64%) 
ios/Runner/Assets.xcassets/AppIcon.appiconset/Icon-App-40x40@1x.png
Unable to render rich display

 BIN -2.54 KB (52%) 
ios/Runner/Assets.xcassets/AppIcon.appiconset/Icon-App-40x40@2x.png
Unable to render rich display

 BIN -5.14 KB (48%) 
ios/Runner/Assets.xcassets/AppIcon.appiconset/Icon-App-40x40@3x.png
Unable to render rich display

 BIN -1.03 KB (62%) 
ios/Runner/Assets.xcassets/AppIcon.appiconset/Icon-App-50x50@1x.png
Unable to render rich display

 BIN -3.56 KB (51%) 
ios/Runner/Assets.xcassets/AppIcon.appiconset/Icon-App-50x50@2x.png
Unable to render rich display

 BIN -1.35 KB (59%) 
ios/Runner/Assets.xcassets/AppIcon.appiconset/Icon-App-57x57@1x.png
Unable to render rich display

 BIN -4.69 KB (48%) 
ios/Runner/Assets.xcassets/AppIcon.appiconset/Icon-App-57x57@2x.png
Unable to render rich display

 BIN -5.14 KB (48%) 
ios/Runner/Assets.xcassets/AppIcon.appiconset/Icon-App-60x60@2x.png
Unable to render rich display

 BIN -9.81 KB (46%) 
ios/Runner/Assets.xcassets/AppIcon.appiconset/Icon-App-60x60@3x.png
Unable to render rich display

 BIN -2.11 KB (54%) 
ios/Runner/Assets.xcassets/AppIcon.appiconset/Icon-App-72x72@1x.png
Unable to render rich display

 BIN -6.92 KB (47%) 
ios/Runner/Assets.xcassets/AppIcon.appiconset/Icon-App-72x72@2x.png
Unable to render rich display

 BIN -2.32 KB (53%) 
ios/Runner/Assets.xcassets/AppIcon.appiconset/Icon-App-76x76@1x.png
Unable to render rich display

 BIN -7.65 KB (46%) 
ios/Runner/Assets.xcassets/AppIcon.appiconset/Icon-App-76x76@2x.png
Unable to render rich display

 BIN -8.84 KB (46%) 
ios/Runner/Assets.xcassets/AppIcon.appiconset/Icon-App-83.5x83.5@2x.png
Unable to render rich display

  140  
lib/components/appbarsearch.com.dart
@@ -1,13 +1,16 @@
import 'package:flutter/material.dart';
import 'package:hooks_riverpod/hooks_riverpod.dart';
import 'package:prototypes/data/location.data.dart';
import 'package:prototypes/repositories/terminal.repository.dart';

class AppBarSearchCom extends StatelessWidget {
class AppBarSearchCom extends HookConsumerWidget {
  const AppBarSearchCom({
    Key? key,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
  Widget build(BuildContext context, WidgetRef ref) {
    final data = ref.watch(TerminalRepositoryNotifier.provider.notifier);
    return SizedBox(
      height: 230,
      child: Stack(children: [
@@ -28,75 +31,76 @@ class AppBarSearchCom extends StatelessWidget {
          child: Container(
            margin: const EdgeInsets.only(left: 30, right: 30),
            child: Card(
                elevation: 30,
                shape: const RoundedRectangleBorder(
                  borderRadius: BorderRadius.all(Radius.circular(10)),
                ),
                // color: Colors.yellow,
                child: Autocomplete(
                  optionsBuilder: (TextEditingValue textEditingValue) {
                    if (textEditingValue.text == '') {
                      return const Iterable<String>.empty();
                    } else {
                      List<String> matches = <String>[];
                      matches.addAll(locationList);
              elevation: 30,
              shape: const RoundedRectangleBorder(
                borderRadius: BorderRadius.all(Radius.circular(10)),
              ),
              child: Autocomplete(
                optionsBuilder: (TextEditingValue textEditingValue) {
                  if (textEditingValue.text == '') {
                    return const Iterable<String>.empty();
                  } else {
                    List<String> matches = <String>[];
                    matches.addAll(locationList);

                      matches.retainWhere((s) {
                        return s
                            .toLowerCase()
                            .contains(textEditingValue.text.toLowerCase());
                      });
                      return matches;
                    }
                  },
                  optionsViewBuilder: (context, onSelected, options) {
                    return Align(
                      alignment: Alignment.topLeft,
                      child: Material(
                        elevation: 4.0,
                        child: ConstrainedBox(
                          constraints: const BoxConstraints(
                              maxHeight: 200, maxWidth: 325),
                          child: ListView.builder(
                            padding: EdgeInsets.zero,
                            shrinkWrap: true,
                            itemCount: options.length,
                            itemBuilder: (BuildContext context, int index) {
                              final String option = options.elementAt(index);
                              return InkWell(
                                onTap: () {
                                  onSelected(option);
                                },
                                child: Container(
                                  color: Theme.of(context).focusColor,
                                  padding: const EdgeInsets.all(16.0),
                                  child: Text(option),
                                ),
                              );
                            },
                          ),
                    matches.retainWhere((s) {
                      return s
                          .toLowerCase()
                          .contains(textEditingValue.text.toLowerCase());
                    });
                    return matches;
                  }
                },
                optionsViewBuilder: (context, onSelected, options) {
                  return Align(
                    alignment: Alignment.topLeft,
                    child: Material(
                      elevation: 4.0,
                      child: ConstrainedBox(
                        constraints:
                            const BoxConstraints(maxHeight: 200, maxWidth: 325),
                        child: ListView.builder(
                          padding: EdgeInsets.zero,
                          shrinkWrap: true,
                          itemCount: options.length,
                          itemBuilder: (BuildContext context, int index) {
                            final String option = options.elementAt(index);
                            return InkWell(
                              onTap: () {
                                onSelected(option);
                              },
                              child: Container(
                                color: Theme.of(context).focusColor,
                                padding: const EdgeInsets.all(16.0),
                                child: Text(option),
                              ),
                            );
                          },
                        ),
                      ),
                    );
                  },
                  fieldViewBuilder: (context, textEditingController, focusNode,
                      onFieldSubmitted) {
                    return TextField(
                      focusNode: focusNode,
                      controller: textEditingController,
                      onEditingComplete: onFieldSubmitted,
                      decoration: const InputDecoration(
                          prefixIcon: Icon(Icons.search),
                          suffixIcon: Icon(Icons.location_on),
                          border: InputBorder.none,
                          hintText: 'Search Destination'),
                      autofillHints: locationList,
                    );
                  },
                  onSelected: (String selection) {
                    print('You just selected $selection');
                  },
                )),
                    ),
                  );
                },
                fieldViewBuilder: (context, textEditingController, focusNode,
                    onFieldSubmitted) {
                  return TextField(
                    focusNode: focusNode,
                    controller: textEditingController,
                    onEditingComplete: onFieldSubmitted,
                    decoration: const InputDecoration(
                        prefixIcon: Icon(Icons.search),
                        suffixIcon: Icon(Icons.location_on),
                        border: InputBorder.none,
                        hintText: 'Search Destination'),
                    autofillHints: locationList,
                  );
                },
                onSelected: (String selection) {
                  data.search(selection);
                  FocusManager.instance.primaryFocus?.unfocus();
                },
              ),
            ),
          ),
        ),
      ]),
 22  
lib/components/namecard.com.dart
@@ -1,23 +1,33 @@
import 'package:fluentui_system_icons/fluentui_system_icons.dart';
import 'package:flutter/material.dart';
import 'package:prototypes/enums/busloading.enum.dart';

class NameCardCom extends StatelessWidget {
  const NameCardCom({
    Key? key,
    required this.title,
    required this.state,
  }) : super(key: key);

  final String title;
  final BusLoading state;

  @override
  Widget build(BuildContext context) {
    final theme = Theme.of(context);
    return Card(
      elevation: 10,
      child: ListTile(
        title: Text(title),
        trailing:
            Icon(FluentIcons.vehicle_bus_24_filled, color: theme.primaryColor),
    return Padding(
      padding: const EdgeInsets.only(top: 10),
      child: Card(
        elevation: 4,
        child: ListTile(
          title: Text(title),
          subtitle: Text(busLoadingToString(state)),
          trailing: Icon(
            FluentIcons.vehicle_bus_24_filled,
            color: theme.primaryColor,
            size: 30,
          ),
        ),
      ),
    );
  }
 52  
lib/data/gen.dart
@@ -0,0 +1,52 @@
import 'dart:math';

import 'package:prototypes/data/location.data.dart';
import 'package:prototypes/enums/busloading.enum.dart';
import 'package:prototypes/models/bus/bus.model.dart';
import 'package:prototypes/models/terminal/terminal.model.dart';

List<String> busNames = [
  'Red Express',
  'Green Line',
  'Yellow Cruiser',
  'Bluebird',
  'Purple Rider',
  'Orange Shuttle',
  'Pink Tourist',
  'Black Bullet',
  'Silver Sprinter',
  'Gold Cruiser',
];

String priceRange(String location) {
  if (location == 'Westerhall' ||
      location == 'Birchgrove' ||
      location == 'Grand Etang') {
    return '\$4.50';
  } else if (location == 'Grenville') {
    return '\$5.00';
  } else if (location == 'Sauteurs') {
    return '\$7.50';
  }
  return '\$2.50';
}

List<Terminal> appData = locationList.map((location) {
  var random = Random();
  List<Bus> buses = List.generate(4, (index) {
    final busLoading =
        index == 0 ? BusLoading.boardingInProgress : BusLoading.queue;
    BusLoading.values[random.nextInt(BusLoading.values.length)];
    return Bus(
      name: busNames[random.nextInt(busNames.length)],
      locations: location,
      price: priceRange(location),
      loadtime: random.nextInt(15) + 1,
      isLoading: index == 0 ? true : false,
      inQueue: index == 0 ? false : true,
      loading: busLoading,
      timeStamp: DateTime.now(),
    );
  });
  return Terminal(location: location, bus: buses);
}).toList();
 45  
lib/data/terminal.data.dart
@@ -0,0 +1,45 @@
// import 'package:prototypes/enums/busloading.enum.dart';
// import 'package:prototypes/models/bus/bus.model.dart';
// import 'package:prototypes/models/terminal/terminal.model.dart';

// final appData = [
//   Terminal(
//     location: 'Belmont',
//     bus: [
//       Bus(
//         name: 'Bus Name',
//         locations: 'Belmont',
//         price: '\$2.50',
//         inQueue: false,
//         isLoading: true,
//         loadtime: 10,
//         loading: BusLoading.boardingInProgress,
//         timeStamp: DateTime.now(),
//       ),
//       Bus(
//         name: 'Bus Name',
//         locations: 'Belmont',
//         price: '\$2.50',
//         loadtime: 15,
//         loading: BusLoading.queue,
//         timeStamp: DateTime.now(),
//       ),
//       Bus(
//         name: 'Bus Name',
//         locations: 'Belmont',
//         price: '\$2.50',
//         loadtime: 1,
//         loading: BusLoading.queue,
//         timeStamp: DateTime.now(),
//       ),
//       Bus(
//         name: 'Bus Name',
//         locations: 'Belmont',
//         price: '\$2.50',
//         loadtime: 2,
//         loading: BusLoading.queue,
//         timeStamp: DateTime.now(),
//       ),
//     ],
//   ),
// ];
 32  
lib/enums/busloading.enum.dart
@@ -0,0 +1,32 @@
enum BusLoading {
  // departingSoon,
  boardingInProgress,
  // leavingTheTerminal,
  queue,
}

String busLoadingToString(BusLoading data) {
  switch (data) {
    // case BusLoading.departingSoon:
    //   return 'Departing Soon';
    case BusLoading.boardingInProgress:
      return 'Boarding in Progress';
    // case BusLoading.leavingTheTerminal:
    //   return 'Leaving the Terminal';
    case BusLoading.queue:
      return 'In Queue';
  }
}

bool busLoadingToBool(BusLoading data) {
  switch (data) {
    // case BusLoading.departingSoon:
    //   return false;
    case BusLoading.boardingInProgress:
      return true;
    // case BusLoading.leavingTheTerminal:
    //   return false;
    case BusLoading.queue:
      return false;
  }
}
 12  
lib/enums/docks.enum.dart
This file was deleted.

 7  
lib/enums/locations.enum.dart
This file was deleted.

 3  
lib/main.dart
@@ -1,9 +1,10 @@
import 'package:flex_color_scheme/flex_color_scheme.dart';
import 'package:flutter/material.dart';
import 'package:hooks_riverpod/hooks_riverpod.dart';
import 'package:prototypes/pages/home.page.dart';

void main() {
  runApp(const MyApp());
  runApp(const ProviderScope(child: MyApp()));
}

class MyApp extends StatelessWidget {
  11  
lib/models/bus/bus.model.dart
@@ -1,6 +1,5 @@
import 'package:freezed_annotation/freezed_annotation.dart';
import 'package:prototypes/enums/docks.enum.dart';
import 'package:prototypes/enums/locations.enum.dart';
import 'package:prototypes/enums/busloading.enum.dart';

part 'bus.model.freezed.dart';

@@ -9,11 +8,11 @@ class Bus with _$Bus {
  const Bus._();
  const factory Bus({
    String? name,
    @Default([]) List<Location?> locations,
    Location? comingForm,
    Location? goingTo,
    String? locations,
    String? price,
    BusLoading? loading,
    DateTime? timeStamp,
    Dock? dockLocation,
    int? loadtime,
    @Default(false) bool isLoading,
    @Default(true) bool inQueue,
  }) = _Bus;
  167  
lib/models/bus/bus.model.freezed.dart
@@ -17,11 +17,11 @@ final _privateConstructorUsedError = UnsupportedError(
/// @nodoc
mixin _$Bus {
  String? get name => throw _privateConstructorUsedError;
  List<Location?> get locations => throw _privateConstructorUsedError;
  Location? get comingForm => throw _privateConstructorUsedError;
  Location? get goingTo => throw _privateConstructorUsedError;
  String? get locations => throw _privateConstructorUsedError;
  String? get price => throw _privateConstructorUsedError;
  BusLoading? get loading => throw _privateConstructorUsedError;
  DateTime? get timeStamp => throw _privateConstructorUsedError;
  Dock? get dockLocation => throw _privateConstructorUsedError;
  int? get loadtime => throw _privateConstructorUsedError;
  bool get isLoading => throw _privateConstructorUsedError;
  bool get inQueue => throw _privateConstructorUsedError;

@@ -36,11 +36,11 @@ abstract class $BusCopyWith<$Res> {
  @useResult
  $Res call(
      {String? name,
      List<Location?> locations,
      Location? comingForm,
      Location? goingTo,
      String? locations,
      String? price,
      BusLoading? loading,
      DateTime? timeStamp,
      Dock? dockLocation,
      int? loadtime,
      bool isLoading,
      bool inQueue});
}
@@ -58,11 +58,11 @@ class _$BusCopyWithImpl<$Res, $Val extends Bus> implements $BusCopyWith<$Res> {
  @override
  $Res call({
    Object? name = freezed,
    Object? locations = null,
    Object? comingForm = freezed,
    Object? goingTo = freezed,
    Object? locations = freezed,
    Object? price = freezed,
    Object? loading = freezed,
    Object? timeStamp = freezed,
    Object? dockLocation = freezed,
    Object? loadtime = freezed,
    Object? isLoading = null,
    Object? inQueue = null,
  }) {
@@ -71,26 +71,26 @@ class _$BusCopyWithImpl<$Res, $Val extends Bus> implements $BusCopyWith<$Res> {
          ? _value.name
          : name // ignore: cast_nullable_to_non_nullable
              as String?,
      locations: null == locations
      locations: freezed == locations
          ? _value.locations
          : locations // ignore: cast_nullable_to_non_nullable
              as List<Location?>,
      comingForm: freezed == comingForm
          ? _value.comingForm
          : comingForm // ignore: cast_nullable_to_non_nullable
              as Location?,
      goingTo: freezed == goingTo
          ? _value.goingTo
          : goingTo // ignore: cast_nullable_to_non_nullable
              as Location?,
              as String?,
      price: freezed == price
          ? _value.price
          : price // ignore: cast_nullable_to_non_nullable
              as String?,
      loading: freezed == loading
          ? _value.loading
          : loading // ignore: cast_nullable_to_non_nullable
              as BusLoading?,
      timeStamp: freezed == timeStamp
          ? _value.timeStamp
          : timeStamp // ignore: cast_nullable_to_non_nullable
              as DateTime?,
      dockLocation: freezed == dockLocation
          ? _value.dockLocation
          : dockLocation // ignore: cast_nullable_to_non_nullable
              as Dock?,
      loadtime: freezed == loadtime
          ? _value.loadtime
          : loadtime // ignore: cast_nullable_to_non_nullable
              as int?,
      isLoading: null == isLoading
          ? _value.isLoading
          : isLoading // ignore: cast_nullable_to_non_nullable
@@ -111,11 +111,11 @@ abstract class _$$_BusCopyWith<$Res> implements $BusCopyWith<$Res> {
  @useResult
  $Res call(
      {String? name,
      List<Location?> locations,
      Location? comingForm,
      Location? goingTo,
      String? locations,
      String? price,
      BusLoading? loading,
      DateTime? timeStamp,
      Dock? dockLocation,
      int? loadtime,
      bool isLoading,
      bool inQueue});
}
@@ -130,11 +130,11 @@ class __$$_BusCopyWithImpl<$Res> extends _$BusCopyWithImpl<$Res, _$_Bus>
  @override
  $Res call({
    Object? name = freezed,
    Object? locations = null,
    Object? comingForm = freezed,
    Object? goingTo = freezed,
    Object? locations = freezed,
    Object? price = freezed,
    Object? loading = freezed,
    Object? timeStamp = freezed,
    Object? dockLocation = freezed,
    Object? loadtime = freezed,
    Object? isLoading = null,
    Object? inQueue = null,
  }) {
@@ -143,26 +143,26 @@ class __$$_BusCopyWithImpl<$Res> extends _$BusCopyWithImpl<$Res, _$_Bus>
          ? _value.name
          : name // ignore: cast_nullable_to_non_nullable
              as String?,
      locations: null == locations
          ? _value._locations
      locations: freezed == locations
          ? _value.locations
          : locations // ignore: cast_nullable_to_non_nullable
              as List<Location?>,
      comingForm: freezed == comingForm
          ? _value.comingForm
          : comingForm // ignore: cast_nullable_to_non_nullable
              as Location?,
      goingTo: freezed == goingTo
          ? _value.goingTo
          : goingTo // ignore: cast_nullable_to_non_nullable
              as Location?,
              as String?,
      price: freezed == price
          ? _value.price
          : price // ignore: cast_nullable_to_non_nullable
              as String?,
      loading: freezed == loading
          ? _value.loading
          : loading // ignore: cast_nullable_to_non_nullable
              as BusLoading?,
      timeStamp: freezed == timeStamp
          ? _value.timeStamp
          : timeStamp // ignore: cast_nullable_to_non_nullable
              as DateTime?,
      dockLocation: freezed == dockLocation
          ? _value.dockLocation
          : dockLocation // ignore: cast_nullable_to_non_nullable
              as Dock?,
      loadtime: freezed == loadtime
          ? _value.loadtime
          : loadtime // ignore: cast_nullable_to_non_nullable
              as int?,
      isLoading: null == isLoading
          ? _value.isLoading
          : isLoading // ignore: cast_nullable_to_non_nullable
@@ -180,35 +180,27 @@ class __$$_BusCopyWithImpl<$Res> extends _$BusCopyWithImpl<$Res, _$_Bus>
class _$_Bus extends _Bus {
  const _$_Bus(
      {this.name,
      final List<Location?> locations = const [],
      this.comingForm,
      this.goingTo,
      this.locations,
      this.price,
      this.loading,
      this.timeStamp,
      this.dockLocation,
      this.loadtime,
      this.isLoading = false,
      this.inQueue = true})
      : _locations = locations,
        super._();
      : super._();

  @override
  final String? name;
  final List<Location?> _locations;
  @override
  @JsonKey()
  List<Location?> get locations {
    if (_locations is EqualUnmodifiableListView) return _locations;
    // ignore: implicit_dynamic_type
    return EqualUnmodifiableListView(_locations);
  }

  final String? locations;
  @override
  final Location? comingForm;
  final String? price;
  @override
  final Location? goingTo;
  final BusLoading? loading;
  @override
  final DateTime? timeStamp;
  @override
  final Dock? dockLocation;
  final int? loadtime;
  @override
  @JsonKey()
  final bool isLoading;
@@ -218,7 +210,7 @@ class _$_Bus extends _Bus {

  @override
  String toString() {
    return 'Bus(name: $name, locations: $locations, comingForm: $comingForm, goingTo: $goingTo, timeStamp: $timeStamp, dockLocation: $dockLocation, isLoading: $isLoading, inQueue: $inQueue)';
    return 'Bus(name: $name, locations: $locations, price: $price, loading: $loading, timeStamp: $timeStamp, loadtime: $loadtime, isLoading: $isLoading, inQueue: $inQueue)';
  }

  @override
@@ -227,31 +219,22 @@ class _$_Bus extends _Bus {
        (other.runtimeType == runtimeType &&
            other is _$_Bus &&
            (identical(other.name, name) || other.name == name) &&
            const DeepCollectionEquality()
                .equals(other._locations, _locations) &&
            (identical(other.comingForm, comingForm) ||
                other.comingForm == comingForm) &&
            (identical(other.goingTo, goingTo) || other.goingTo == goingTo) &&
            (identical(other.locations, locations) ||
                other.locations == locations) &&
            (identical(other.price, price) || other.price == price) &&
            (identical(other.loading, loading) || other.loading == loading) &&
            (identical(other.timeStamp, timeStamp) ||
                other.timeStamp == timeStamp) &&
            (identical(other.dockLocation, dockLocation) ||
                other.dockLocation == dockLocation) &&
            (identical(other.loadtime, loadtime) ||
                other.loadtime == loadtime) &&
            (identical(other.isLoading, isLoading) ||
                other.isLoading == isLoading) &&
            (identical(other.inQueue, inQueue) || other.inQueue == inQueue));
  }

  @override
  int get hashCode => Object.hash(
      runtimeType,
      name,
      const DeepCollectionEquality().hash(_locations),
      comingForm,
      goingTo,
      timeStamp,
      dockLocation,
      isLoading,
      inQueue);
  int get hashCode => Object.hash(runtimeType, name, locations, price, loading,
      timeStamp, loadtime, isLoading, inQueue);

  @JsonKey(ignore: true)
  @override
@@ -263,27 +246,27 @@ class _$_Bus extends _Bus {
abstract class _Bus extends Bus {
  const factory _Bus(
      {final String? name,
      final List<Location?> locations,
      final Location? comingForm,
      final Location? goingTo,
      final String? locations,
      final String? price,
      final BusLoading? loading,
      final DateTime? timeStamp,
      final Dock? dockLocation,
      final int? loadtime,
      final bool isLoading,
      final bool inQueue}) = _$_Bus;
  const _Bus._() : super._();

  @override
  String? get name;
  @override
  List<Location?> get locations;
  String? get locations;
  @override
  Location? get comingForm;
  String? get price;
  @override
  Location? get goingTo;
  BusLoading? get loading;
  @override
  DateTime? get timeStamp;
  @override
  Dock? get dockLocation;
  int? get loadtime;
  @override
  bool get isLoading;
  @override
 13  
lib/models/terminal/terminal.model.dart
@@ -0,0 +1,13 @@
import 'package:freezed_annotation/freezed_annotation.dart';
import 'package:prototypes/models/bus/bus.model.dart';

part 'terminal.model.freezed.dart';

@freezed
class Terminal with _$Terminal {
  const Terminal._();
  const factory Terminal({
    String? location,
    List<Bus>? bus,
  }) = _Terminal;
}
 159  
lib/models/terminal/terminal.model.freezed.dart
@@ -0,0 +1,159 @@
// coverage:ignore-file
// GENERATED CODE - DO NOT MODIFY BY HAND
// ignore_for_file: type=lint
// ignore_for_file: unused_element, deprecated_member_use, deprecated_member_use_from_same_package, use_function_type_syntax_for_parameters, unnecessary_const, avoid_init_to_null, invalid_override_different_default_values_named, prefer_expression_function_bodies, annotate_overrides, invalid_annotation_target, unnecessary_question_mark

part of 'terminal.model.dart';

// **************************************************************************
// FreezedGenerator
// **************************************************************************

T _$identity<T>(T value) => value;

final _privateConstructorUsedError = UnsupportedError(
    'It seems like you constructed your class using `MyClass._()`. This constructor is only meant to be used by freezed and you are not supposed to need it nor use it.\nPlease check the documentation here for more information: https://github.com/rrousselGit/freezed#custom-getters-and-methods');

/// @nodoc
mixin _$Terminal {
  String? get location => throw _privateConstructorUsedError;
  List<Bus>? get bus => throw _privateConstructorUsedError;

  @JsonKey(ignore: true)
  $TerminalCopyWith<Terminal> get copyWith =>
      throw _privateConstructorUsedError;
}

/// @nodoc
abstract class $TerminalCopyWith<$Res> {
  factory $TerminalCopyWith(Terminal value, $Res Function(Terminal) then) =
      _$TerminalCopyWithImpl<$Res, Terminal>;
  @useResult
  $Res call({String? location, List<Bus>? bus});
}

/// @nodoc
class _$TerminalCopyWithImpl<$Res, $Val extends Terminal>
    implements $TerminalCopyWith<$Res> {
  _$TerminalCopyWithImpl(this._value, this._then);

  // ignore: unused_field
  final $Val _value;
  // ignore: unused_field
  final $Res Function($Val) _then;

  @pragma('vm:prefer-inline')
  @override
  $Res call({
    Object? location = freezed,
    Object? bus = freezed,
  }) {
    return _then(_value.copyWith(
      location: freezed == location
          ? _value.location
          : location // ignore: cast_nullable_to_non_nullable
              as String?,
      bus: freezed == bus
          ? _value.bus
          : bus // ignore: cast_nullable_to_non_nullable
              as List<Bus>?,
    ) as $Val);
  }
}

/// @nodoc
abstract class _$$_TerminalCopyWith<$Res> implements $TerminalCopyWith<$Res> {
  factory _$$_TerminalCopyWith(
          _$_Terminal value, $Res Function(_$_Terminal) then) =
      __$$_TerminalCopyWithImpl<$Res>;
  @override
  @useResult
  $Res call({String? location, List<Bus>? bus});
}

/// @nodoc
class __$$_TerminalCopyWithImpl<$Res>
    extends _$TerminalCopyWithImpl<$Res, _$_Terminal>
    implements _$$_TerminalCopyWith<$Res> {
  __$$_TerminalCopyWithImpl(
      _$_Terminal _value, $Res Function(_$_Terminal) _then)
      : super(_value, _then);

  @pragma('vm:prefer-inline')
  @override
  $Res call({
    Object? location = freezed,
    Object? bus = freezed,
  }) {
    return _then(_$_Terminal(
      location: freezed == location
          ? _value.location
          : location // ignore: cast_nullable_to_non_nullable
              as String?,
      bus: freezed == bus
          ? _value._bus
          : bus // ignore: cast_nullable_to_non_nullable
              as List<Bus>?,
    ));
  }
}

/// @nodoc
class _$_Terminal extends _Terminal {
  const _$_Terminal({this.location, final List<Bus>? bus})
      : _bus = bus,
        super._();

  @override
  final String? location;
  final List<Bus>? _bus;
  @override
  List<Bus>? get bus {
    final value = _bus;
    if (value == null) return null;
    if (_bus is EqualUnmodifiableListView) return _bus;
    // ignore: implicit_dynamic_type
    return EqualUnmodifiableListView(value);
  }

  @override
  String toString() {
    return 'Terminal(location: $location, bus: $bus)';
  }

  @override
  bool operator ==(dynamic other) {
    return identical(this, other) ||
        (other.runtimeType == runtimeType &&
            other is _$_Terminal &&
            (identical(other.location, location) ||
                other.location == location) &&
            const DeepCollectionEquality().equals(other._bus, _bus));
  }

  @override
  int get hashCode => Object.hash(
      runtimeType, location, const DeepCollectionEquality().hash(_bus));

  @JsonKey(ignore: true)
  @override
  @pragma('vm:prefer-inline')
  _$$_TerminalCopyWith<_$_Terminal> get copyWith =>
      __$$_TerminalCopyWithImpl<_$_Terminal>(this, _$identity);
}

abstract class _Terminal extends Terminal {
  const factory _Terminal({final String? location, final List<Bus>? bus}) =
      _$_Terminal;
  const _Terminal._() : super._();

  @override
  String? get location;
  @override
  List<Bus>? get bus;
  @override
  @JsonKey(ignore: true)
  _$$_TerminalCopyWith<_$_Terminal> get copyWith =>
      throw _privateConstructorUsedError;
}
  112  
lib/pages/home.page.dart
@@ -1,14 +1,20 @@
import 'package:flutter/material.dart';
import 'package:hooks_riverpod/hooks_riverpod.dart';
import 'package:lottie/lottie.dart';
import 'package:prototypes/components/appbarsearch.com.dart';
import 'package:prototypes/components/maincard.com.dart';
import 'package:prototypes/components/namecard.com.dart';
import 'package:prototypes/enums/busloading.enum.dart';
import 'package:prototypes/models/terminal/terminal.model.dart';
import 'package:prototypes/repositories/terminal.repository.dart';

class HomePage extends StatelessWidget {
class HomePage extends HookConsumerWidget {
  const HomePage({super.key});

  @override
  Widget build(BuildContext context) {
  Widget build(BuildContext context, WidgetRef ref) {
    final theme = Theme.of(context);
    final data = ref.watch(TerminalRepositoryNotifier.provider);
    return Scaffold(
      appBar: AppBar(
        title: const Text('Bus Terminal'),
@@ -24,46 +30,74 @@ class HomePage extends StatelessWidget {
          child: AppBarSearchCom(),
        ),
      ),
      body: ListView(
        shrinkWrap: true,
        padding: const EdgeInsets.all(20),
        children: const [
          Center(
            child: Padding(
              padding: EdgeInsets.symmetric(vertical: 10),
              child: Text(
                'Location',
                style: TextStyle(
                  fontSize: 28,
                  fontWeight: FontWeight.bold,
                ),
      body: data.location == null
          ? Center(
              child: Column(
                children: [
                  Lottie.asset('assets/lottie/39612-location-animation.json')
                ],
              ),
            ),
          ),
          MainCardCom(
            busName: 'Bus Name Bus Name Bus Name ',
            busLocation: 'Bus Location',
            busCost: '\$2.50',
            busBoarding: 'Boarding in Progress',
            loadingTime: 'Time Past: 10 minute',
          ),
          Center(
            child: Padding(
              padding: EdgeInsets.symmetric(vertical: 10),
              child: Text(
                'Queue Line - 3',
                style: TextStyle(
                  fontSize: 24,
                  fontWeight: FontWeight.bold,
                ),
            )
          : ContentList(data: data),
    );
  }
}

class ContentList extends StatelessWidget {
  const ContentList({
    Key? key,
    required this.data,
  }) : super(key: key);

  final Terminal data;

  @override
  Widget build(BuildContext context) {
    return ListView(
      shrinkWrap: true,
      padding: const EdgeInsets.all(20),
      children: [
        // Center(
        //   child: Padding(
        //     padding: const EdgeInsets.symmetric(vertical: 10),
        //     child: Text(
        //       data.location!,
        //       style: const TextStyle(
        //         fontSize: 28,
        //         fontWeight: FontWeight.bold,
        //       ),
        //     ),
        //   ),
        // ),
        if (data.bus == null)
          ...[]
        else ...[
          for (final item in data.bus!)
            if (item.isLoading) ...[
              MainCardCom(
                busName: item.name!,
                busLocation: item.locations!,
                busCost: item.price!,
                busBoarding: busLoadingToString(item.loading!),
                loadingTime: 'Time Past: ${item.loadtime} minute',
              ),
            ),
          ),
          NameCardCom(title: 'Bus Name'),
          NameCardCom(title: 'Bus Name'),
          NameCardCom(title: 'Bus Name'),
            ] else ...[
              // const Center(
              //   child: Padding(
              //     padding: EdgeInsets.symmetric(vertical: 10),
              //     child: Text(
              //       'Queue Line - 3',
              //       style: TextStyle(
              //         fontSize: 24,
              //         fontWeight: FontWeight.bold,
              //       ),
              //     ),
              //   ),
              // ),
              NameCardCom(title: item.name!, state: item.loading!),
            ],
        ],
      ),
      ],
    );
  }
}
 5  
lib/providers/appdata.provider.dart
@@ -0,0 +1,5 @@
import 'package:hooks_riverpod/hooks_riverpod.dart';
import 'package:prototypes/data/gen.dart';
import 'package:prototypes/models/terminal/terminal.model.dart';

final appDataProvider = Provider.autoDispose<List<Terminal>>((ref) => appData);
 29  
lib/repositories/terminal.repository.dart
@@ -0,0 +1,29 @@
import 'package:hooks_riverpod/hooks_riverpod.dart';
import 'package:prototypes/models/terminal/terminal.model.dart';
import 'package:prototypes/providers/appdata.provider.dart';

final _terminalProvider =
    StateNotifierProvider.autoDispose<TerminalRepositoryNotifier, Terminal>(
        (ref) => TerminalRepositoryNotifier(ref, const Terminal()));

class TerminalRepositoryNotifier extends StateNotifier<Terminal> {
  TerminalRepositoryNotifier(this.ref, super.state);
  final Ref ref;

  static AutoDisposeStateNotifierProvider<TerminalRepositoryNotifier, Terminal>
      get provider => _terminalProvider;

  void search(String location) {
    final data = ref.read(appDataProvider);
    final filter = data
        .where((e) => e.location?.toLowerCase() == location.toLowerCase())
        .toList();

    filter.isEmpty
        ? const Terminal()
        : state = state.copyWith(
            location: filter[0].location,
            bus: filter[0].bus,
          );
  }
}
 BIN -98.8 KB (61%) 
macos/Runner/Assets.xcassets/AppIcon.appiconset/app_icon_1024.png

 BIN -8.22 KB (39%) 
macos/Runner/Assets.xcassets/AppIcon.appiconset/app_icon_128.png

 BIN -55 Bytes (93%) 
macos/Runner/Assets.xcassets/AppIcon.appiconset/app_icon_16.png

 BIN -20.6 KB (42%) 
macos/Runner/Assets.xcassets/AppIcon.appiconset/app_icon_256.png

 BIN -743 Bytes (62%) 
macos/Runner/Assets.xcassets/AppIcon.appiconset/app_icon_32.png

 BIN -45.5 KB (51%) 
macos/Runner/Assets.xcassets/AppIcon.appiconset/app_icon_512.png

 BIN -2.56 KB (48%) 
macos/Runner/Assets.xcassets/AppIcon.appiconset/app_icon_64.png

  35  
pubspec.lock
@@ -216,6 +216,13 @@ packages:
    description: flutter
    source: sdk
    version: "0.0.0"
  flutter_hooks:
    dependency: "direct main"
    description:
      name: flutter_hooks
      url: "https://pub.dartlang.org"
    source: hosted
    version: "0.18.5+1"
  flutter_launcher_icons:
    dependency: "direct dev"
    description:
@@ -230,6 +237,13 @@ packages:
      url: "https://pub.dartlang.org"
    source: hosted
    version: "2.0.1"
  flutter_riverpod:
    dependency: transitive
    description:
      name: flutter_riverpod
      url: "https://pub.dartlang.org"
    source: hosted
    version: "2.1.3"
  flutter_test:
    dependency: "direct dev"
    description: flutter
@@ -270,6 +284,13 @@ packages:
      url: "https://pub.dartlang.org"
    source: hosted
    version: "2.2.0"
  hooks_riverpod:
    dependency: "direct main"
    description:
      name: hooks_riverpod
      url: "https://pub.dartlang.org"
    source: hosted
    version: "2.1.3"
  http_multi_server:
    dependency: transitive
    description:
@@ -410,6 +431,13 @@ packages:
      url: "https://pub.dartlang.org"
    source: hosted
    version: "1.2.1"
  riverpod:
    dependency: transitive
    description:
      name: riverpod
      url: "https://pub.dartlang.org"
    source: hosted
    version: "2.1.3"
  shelf:
    dependency: transitive
    description:
@@ -450,6 +478,13 @@ packages:
      url: "https://pub.dartlang.org"
    source: hosted
    version: "1.10.0"
  state_notifier:
    dependency: transitive
    description:
      name: state_notifier
      url: "https://pub.dartlang.org"
    source: hosted
    version: "0.7.2+1"
  stream_channel:
    dependency: transitive
    description:
  14  
pubspec.yaml
@@ -36,6 +36,8 @@ dependencies:
  flex_color_scheme: ^6.1.2
  fluentui_system_icons: ^1.1.190
  lottie: ^2.2.0
  flutter_hooks: ^0.18.0
  hooks_riverpod: ^2.1.3

dev_dependencies:
  # The "flutter_lints" package below contains a set of recommended lints to
@@ -92,19 +94,19 @@ flutter_icons:
  android: "launcher_icon"
  ios: true
  remove_alpha_ios: true
  image_path: "assets/icon/ic_launcher.png"
  image_path: "assets/icon/image_path.png"
  min_sdk_android: 21 # android min sdk min:16, default 21
  adaptive_icon_background: "assets/icon/ic_launcher_adaptive_back.png"
  adaptive_icon_foreground: "assets/icon/ic_launcher_adaptive_fore.png"
  adaptive_icon_background: "assets/icon/adaptive_icon_background.png"
  adaptive_icon_foreground: "assets/icon/adaptive_icon_foreground.png"
  web:
    generate: true
    image_path: "assets/icon/ic_launcher.png"
    image_path: "assets/icon/image_path.png"
    background_color: "#4496E0"
    theme_color: "#4496E0"
  windows:
    generate: true
    image_path: "assets/icon/ic_launcher.png"
    image_path: "assets/icon/image_path.png"
    icon_size: 48 # min:48, max:256, default: 48
  macos:
    generate: true
    image_path: "assets/icon/ic_launcher.png"
    image_path: "assets/icon/image_path.png"
 BIN -55 Bytes (93%) 
web/favicon.png

 BIN -14.1 KB (40%) 
web/icons/Icon-192.png

 BIN -45.5 KB (51%) 
web/icons/Icon-512.png

 BIN -14.1 KB (40%) 
web/icons/Icon-maskable-192.png

 BIN -45.5 KB (51%) 
web/icons/Icon-maskable-512.png

 BIN -1.6 KB (52%) 
windows/runner/resources/app_icon.ico
Binary file not shown.
0 comments on commit b722d97
@GMark1974
 
Add heading textAdd bold text, <Ctrl+b>Add italic text, <Ctrl+i>
Add a quote, <Ctrl+Shift+.>Add code, <Ctrl+e>Add a link, <Ctrl+k>
Add a bulleted list, <Ctrl+Shift+8>Add a numbered list, <Ctrl+Shift+7>Add a task list, <Ctrl+Shift+l>
Directly mention a user or team
Reference an issue, pull request, or discussion
Add saved reply
Leave a comment
No file chosen
Attach files by dragging & dropping, selecting or pasting them.
Styling with Markdown is supported
 You’re not receiving notifications from this thread.
Footer
© 2023 GitHub, Inc.
Footer navigation
Terms
Privacy
Security
Status
Docs
Contact GitHub
Pricing
API
Training
Blog
About
mvp · kerrongordon/treminal@b722d97
