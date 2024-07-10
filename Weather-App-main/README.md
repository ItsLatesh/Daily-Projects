# Flutter Weather App

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Introduction


-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Welcome to our Weather App, a seamless and intuitive solution for all your weather-related needs, built using Flutter. Whether you're planning your daily commute, a weekend getaway, or simply want to stay informed about the weather, our app provides accurate and up-to-date weather information right at your fingertips.


---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Key Features

Real-Time Weather Updates: Get current weather conditions, including temperature, humidity, wind speed, and more, for any location around the globe.

7-Day Forecast: Plan ahead with our detailed 7-day weather forecast, giving you a comprehensive view of the upcoming weather patterns.

Interactive Weather Maps: Visualize weather changes with interactive maps that show precipitation, cloud cover, and temperature variations.

Severe Weather Alerts: Stay safe with timely alerts for severe weather conditions such as storms, heavy rainfall, and extreme temperatures.

User-Friendly Interface: Navigate easily through the app with a clean and modern design, ensuring a smooth user experience.

Location-Based Services: Automatically fetch weather information for your current location or search for weather updates in any city of your choice.

Customizable Widgets: Personalize your home screen with widgets that display the current weather and forecast.

Sunrise and Sunset Times: Keep track of sunrise and sunset times to plan your day more effectively.

Multiple Language Support: Access weather information in multiple languages, making it convenient for users from different regions.

Dark Mode: Reduce eye strain and save battery life with the dark mode feature.

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Technology Stack

Flutter: A powerful UI toolkit by Google that allows us to create natively compiled applications for mobile, web, and desktop from a single codebase.
  : The programming language used with Flutter, known for its performance and ease of use.
RESTful APIs: To fetch real-time weather data from reliable sources.
State Management: Using providers or GetX to manage the app's state efficiently.
Geolocation Services: To provide accurate location-based weather updates.

It includes several components, such as API services, exception handling, data models, UI widgets, utility functions, and a global controller for managing state and fetching data.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


Abstract Class: BaseApiServices


  
abstract class BaseApiServices {
  Future<dynamic> getWeatherApi(var lan, var lon);
}
This defines an abstract class BaseApiServices with a method getWeatherApi, which is intended to fetch weather data based on latitude and longitude.

Exception Handling
  
  
class AppExceptions implements Exception {
  final String? _prefix;
  final String? _message;

  AppExceptions([this._prefix, this._message]);
}

class InternetException extends AppExceptions {
  InternetException([message]) : super('');
}

class RequestTimeOut extends AppExceptions {
  RequestTimeOut([message]) : super('');
}

class ServerError extends AppExceptions {
  ServerError([message]) : super('');
}

class InvalidUrlException extends AppExceptions {
  InvalidUrlException([message]) : super('');
}

class FetchDataException extends AppExceptions {
  FetchDataException([message]) : super('');
}

These classes define custom exceptions for various error scenarios that might occur during API requests, such as internet issues, request timeouts, server errors, invalid URLs, and data fetch errors.


-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


Weather Data Model
  
  
class WeatherModel {
  // Weather data properties
  // Constructor, fromJson and toJson methods
}
This class models the weather data structure. It includes properties such as coordinates, weather conditions, temperature, wind, clouds, etc. It has methods for serializing and deserializing JSON data.

Nested Data Models
The WeatherModel class includes nested models for various components of the weather data:

Coord
Weather
Main
Wind
Clouds
Sys
Each of these classes follows a similar structure with constructors, fromJson, and toJson methods to handle JSON data.


----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


Utility Functions
  
  
import 'package:intl/intl.  ';

class Utils {
  static String date = DateFormat('yMMMMd').format(DateTime.now());

  static String convertVisibility(String visibilityMeters) {
    double visibilityKm = double.parse(visibilityMeters) / 1000.0;
    return '${visibilityKm.toStringAsFixed(0)} Km';
  }

  static String convertSpeed(String speedMetersPerSecond) {
    double speedKilometersPerHour = double.parse(speedMetersPerSecond) * 3.6;
    return '${speedKilometersPerHour.toStringAsFixed(0)} km/h';
  }

  static String getWindDirection(String degrees) {
    int degreesInt = int.tryParse(degrees) ?? 0;
    // Wind direction logic
  }

  static String convertTimestampToTime(String timestamp) {
    int timestampInt = int.tryParse(timestamp) ?? 0;
    // Convert timestamp to time
  }

  static String convertDtToTime(String dt) {
    int dtInt = int.tryParse(dt) ?? 0;
    // Convert datetime to time
  }

  static LinearGradient getBackgroundGradient(String weatherMain) {
    // Determine gradient based on weather
  }
}
The Utils class provides various utility functions for formatting dates, converting visibility and speed units, determining wind direction, and generating gradients based on weather conditions.


--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------



Frosted Glass UI Component
  
  
import 'package:flutter/material.  ';

class FrostedContainer extends StatelessWidget {
  final Widget child;
  final double height;

  const FrostedContainer({super.key, required this.child, required this.height});

  @override
  Widget build(BuildContext context) {
    return Padding(
      // Frosted glass effect implementation
    );
  }
}
The FrostedContainer class is a reusable widget that applies a frosted glass effect to its child widget using BackdropFilter and BoxDecoration.


--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


API URL Construction
  
  
const apiKey = 'your_api_key';

String weatherAppUrl(var lat, var lon) {
  String url;
  url = 'https://api.openweathermap.org/data/2.5/weather?lat=$lat&lon=$lon&appid=$apiKey&units=metric';
  return url;
}
This function constructs the URL for fetching weather data from the OpenWeatherMap API, using the provided latitude, longitude, and an API key.


---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


Global Controller
  
  
import 'package:get/get.  ';

class GlobalController extends GetxController {
  final RxBool _isLoading = true.obs;
  final RxDouble latitude = 0.0.obs;
  final RxDouble longitude = 0.0.obs;
  final Rx<WeatherModel?> _weatherData = Rx<WeatherModel?>(null);
  final RxString _weatherMain = ''.obs;

  // Getters and setters

  @override
  void onInit() {
    super.onInit();
    if (_isLoading.isTrue) {
      getLocationAndFetchWeather();
    }
  }

  Future<void> getLocationAndFetchWeather() async {
    try {
      await getLocation();
      await getWeather();
    } catch (e) {
      print('Error: $e');
    } finally {
      _isLoading.value = false;
    }
  }

  Future<void> getLocation() async {
    try {
      // Get current location
    } catch (e) {
      print('Error getting location: $e');
    }
  }

  Future<void> getWeather() async {
    try {
      final weatherData = await ApiServices().getWeatherApi(
        latitude.value,
        longitude.value,
      );

      final weatherModel = WeatherModel.fromJson(weatherData);
      setWeatherMain(weatherModel.weather![0].main!.toString());

      _weatherData.value = weatherModel;
    } catch (e) {
      print('Error fetching weather data: $e');
    }
  }
}
The GlobalController class uses the GetX package for state management. It handles fetching the user's location and weather data, and stores this data in observable properties.


-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------



Widgets
Footer Widget
  
  
import 'package:flutter/material.  ';
import 'package:get/get.  ';

class FooterWidget extends StatelessWidget {
  const FooterWidget({super.key});

  @override
  Widget build(BuildContext context) {
    final GlobalController globalController = Get.put(GlobalController());

    return Padding(
      padding: const EdgeInsets.symmetric(horizontal: 10),
      child: Row(
        mainAxisAlignment: MainAxisAlignment.spaceBetween,
        children: [
          const Text('OpenWeather', style: lightBodyTextStyle),
          Text('Last updated ${globalController.dtValue()}', style: lightBodyTextStyle),
        ],
      ),
    );
  }
}
The FooterWidget displays the name of the weather service and the last updated time using the data from the GlobalController.

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------



Summary
This code provides a comprehensive structure for a weather application in Flutter. It includes:

Abstract class for API services.
Custom exceptions for error handling.
Data models for weather data.
Utility functions for data formatting.
UI components with a frosted glass effect.
Global controller for managing state and fetching data.
Widgets for displaying weather information.


---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------


#### how to run a app?



1. Open Your Flutter Project
If you've already created a Flutter project or if you have an existing project:

Open Android Studio.
Click on File > Open or Import.
Navigate to your Flutter project directory and select it.
Android Studio will automatically detect it as a Flutter project and set up the necessary configurations.
2. Set Up Emulator or Connect a Device
You have two options to run your Flutter app: using an emulator or a physical Android device.

Using an Emulator:
Open Android Studio.
Click on Tools > AVD Manager.
Create a new Virtual Device if you haven’t already. Choose a device definition and select a system image (preferably one with Google Play for full functionality).
Start the emulator by clicking on the Play button next to your virtual device.
Using a Physical Device:
Enable Developer Mode and USB Debugging on your Android device (Settings > Developer Options).
Connect your device to your computer via USB cable.
3. Run Your Flutter App
Once your device or emulator is ready and connected:

In Android Studio, open the Flutter project.
Click on the green play button (Run) in the toolbar, or use Run > Run 'main.dart' from the top menu.
Android Studio will compile your Flutter app and deploy it to the selected device or emulator.
4. Using VS Code (Optional)
If you prefer using VS Code for Flutter development:

Open VS Code.
Open your Flutter project folder.
Use the Terminal in VS Code to run flutter doctor to ensure everything is set up correctly.
Connect a device or start an emulator as mentioned above.
Open the main.dart file and click on Run > Start Debugging (F5) to run your app.
5. Hot Reload and Debugging
While your app is running, you can make changes to your Dart code.
Save the changes, and Flutter's hot reload feature will automatically update your running app with the new code.
Use breakpoints and debugging tools in Android Studio or VS Code to debug your Flutter app.


-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Why Flutter?

Flutter was chosen for this project due to its numerous advantages:

Fast Development: Flutter’s hot reload feature allows developers to see changes in real time, speeding up the development process.
Expressive and Flexible UI: Flutter provides a rich set of customizable widgets to create beautiful and functional user interfaces.
Cross-Platform Compatibility: With Flutter, we can deploy our app on both Android and iOS platforms with a single codebase, ensuring a consistent user experience across devices.
Strong Community Support: Flutter has a growing community and extensive documentation, making it easier to find solutions and best practices.



