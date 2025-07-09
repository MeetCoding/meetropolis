Flutter is a Dart programming language based UI framework developed by Google for cross-platform UI consistency and a single code base system.
It works on a widget-based architecture with hot reload and native performance. It contains a library of pre-built UI components for Material Design (Android) and Cupertino (iOS).
# Introduction to Flutter
## Architecture of Flutter
Flutter uses a layered architecture where each layer only depends on the layer right beneath it.
- **Embedder (Platform Specific):** This is the bottom most layer acting as a bridge between flutter and the operating system.
	- Responsible for rendering surface (SurfaceView for Android and Metal for iOS).
	- Manages threads and OS event loop.
	- Handles native plugins for features like camera and bluetooth.
- **Engine (Core Layer):** Written in C++, it handles rendering via Skia or Impeller graphics engine. 
	- Manages compilation (Just-in-Time for developement and Ahead-of-Time for production).
	- Schedules frames and animations.
	- Provides low-level services like asset loading, file i/o and plugin architecture.
- **Framework (Dart Layer):** Layer with which developers interact and write code to develop mobile applications.
	- Rich set of libraries for ui, animation and gesture handling.
	- Organizes app as a widget tree.
	- Handles build phase where widget tree is converted into element tree and render object tree.
	- Handles state management.
The framework layer contains three trees:
1. **Widget Tree:** Represents the UI structure, every visual element is a widget.
2. **Element Tree:** Maps widget instances to their location in the UI heirarchy.
3. **Render Object Tree:** Handles layout, painting and hit-testing. This is the tree that is actually drawn on the UI.
## Flutter Project Architecture
When initiating a flutter project using `flutter create <app_name>`, we are concerned with four elements:
- `lib/main.dart`: This is the main entry point for our flutter project, where the code starts
- `pubspec.yaml`: Manages dependencies
- `android/ and ios/`: Platform-specific code
- `test/`: For writing tests

Everything in Flutter is a widget, each button, text and even the entire app itself. Widget is the most basic building block of a flutter application. Widgets are of two types:
- **Stateless Widget:** A widget whose properties do not change with state.
- **Stateful Widget:** A widget whose properties do change with the state, re-rendering as changes are made to the state.
**BuildContext** is a crucial element for a widget in flutter. Each widget has its own BuildContext which stores the reference of the position of that widget in the element tree and helps us traverse parent widgets.
In code, a context object is passed to the build method of the widget.

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('Flutter BuildContext'),
        ),
        body: Center(
          child: MyWidget(),
        ),
      ),
    );
  }
}

class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Text('Hello, BuildContext!');
  }
}

```
# Common Widgets in Flutter
- **Material Components:** Use these widgets for Android/Google-style apps.
- **Cupertino Widgets:** Use Cupertino equivalents (`CupertinoApp`, `CupertinoNavigationBar`, etc.) for iOS-style apps.
Styling is handled using slots like activeColor, hoverColor, etc.

**How to Use Slots**
A “slot” is a property where you insert another widget or value. For example, in `Scaffold`, the `appBar` slot expects an `AppBar` widget, and the `body` slot expects your main content widget.
**Example: Scaffold with slots**

- **MaterialApp** and **Scaffold** form the backbone of most apps.
- **AppBar**, **Drawer**, **BottomNavigationBar**, and **FloatingActionButton** are key slots for navigation and actions.
- **Column**, **Row**, **Stack**, and **Container** are essential for layout.
- **Text**, **Image**, **Icon**, and **Card** are core for content.
- **ElevatedButton**, **TextField**, **Checkbox**, and other input widgets capture user interaction.
- **ListView** and **GridView** handle scrollable content.
- **FutureBuilder** and **StreamBuilder** are vital for async UI.
- **AlertDialog**, **SnackBar**, and **BottomSheet** handle overlays and messages.
- **SafeArea**, **MediaQuery**, and **GestureDetector** make your app robust and responsive.
## **Core Structural Widgets**

|Widget|Purpose/Usage|Key Slots/Properties|
|---|---|---|
|**MaterialApp**|Entry point for Material Design apps|`home`, `routes`, `theme`, `title`, `navigatorKey`|
|**Scaffold**|Basic visual layout structure for screens|`appBar`, `body`, `drawer`, `bottomNavigationBar`, `floatingActionButton`, `bottomSheet`, `backgroundColor`|
|**AppBar**|Top app bar with title and actions|`title`, `actions`, `leading`, `bottom`, `backgroundColor`|
|**Drawer**|Side navigation panel|`child`, usually a `ListView` or `Column`|
|**BottomNavigationBar**|Bottom navigation tabs|`items`, `currentIndex`, `onTap`, `type`|
|**FloatingActionButton**|Circular button for primary actions|`child`, `onPressed`, `tooltip`, `backgroundColor`|
|**TabBar/TabBarView**|Tab navigation and content display|`tabs`, `controller`, `children` (for TabBarView)|
## **Layout Widgets**

|Widget|Purpose/Usage|Key Slots/Properties|
|---|---|---|
|**Column**|Vertical arrangement of children|`children`, `mainAxisAlignment`, `crossAxisAlignment`|
|**Row**|Horizontal arrangement of children|`children`, `mainAxisAlignment`, `crossAxisAlignment`|
|**Stack**|Overlapping widgets|`children`, `alignment`, `fit`|
|**Container**|Box model for styling, padding, and alignment|`child`, `padding`, `margin`, `decoration`, `alignment`, `width`, `height`|
|**Expanded**|Expands a child of Row, Column, or Flex|`child`, `flex`|
|**Padding**|Adds padding around a child|`child`, `padding`|
|**SizedBox**|Adds fixed space or size|`child`, `width`, `height`|
|**Wrap**|Wraps children onto multiple lines|`children`, `direction`, `spacing`, `runSpacing`|
|**Align**|Aligns a child within itself|`child`, `alignment`|
|**ListView**|Scrollable list of widgets|`children` or `itemBuilder`, `scrollDirection`|
|**GridView**|Scrollable grid of widgets|`children` or `itemBuilder`, `gridDelegate`|
|**SingleChildScrollView**|Makes a single child scrollable|`child`, `scrollDirection`|
## **Content and Display Widgets**

|Widget|Purpose/Usage|Key Slots/Properties|
|---|---|---|
|**Text**|Displays styled text|`data`, `style`, `textAlign`, `overflow`|
|**Image**|Displays images from assets, network, or files|`image`, `fit`, `width`, `height`|
|**Icon**|Displays a Material icon|`icon`, `size`, `color`|
|**Card**|Material-style card container|`child`, `elevation`, `margin`, `color`|
|**CircleAvatar**|Circular image or icon, often for user profiles|`child`, `backgroundImage`, `radius`|
|**FlutterLogo**|Displays the Flutter logo|`size`, `colors`, `style`|
## **Input and Interaction Widgets**

|Widget|Purpose/Usage|Key Slots/Properties|
|---|---|---|
|**ElevatedButton**|Raised button for actions|`child`, `onPressed`, `style`|
|**TextButton**|Flat button for less prominent actions|`child`, `onPressed`, `style`|
|**IconButton**|Button with an icon|`icon`, `onPressed`, `tooltip`|
|**TextField**|User text input|`controller`, `decoration`, `onChanged`, `keyboardType`|
|**Checkbox**|Checkbox input|`value`, `onChanged`, `activeColor`|
|**Switch**|On/off switch input|`value`, `onChanged`, `activeColor`|
|**Radio**|Radio button input|`value`, `groupValue`, `onChanged`|
|**Slider**|Slider for selecting a value from a range|`value`, `onChanged`, `min`, `max`|
|**DropdownButton**|Dropdown menu for selection|`items`, `value`, `onChanged`|
## **Navigation and Routing Widgets**

|Widget|Purpose/Usage|Key Slots/Properties|
|---|---|---|
|**Navigator**|Manages a stack of routes/screens|`pages`, `onPopPage`, `key`|
|**PageRouteBuilder**|Custom route transitions|`pageBuilder`, `transitionsBuilder`|
|**BottomSheet**|Modal or persistent bottom sheets|`builder`, `backgroundColor`, `shape`|
|**AlertDialog**|Modal dialog for alerts or confirmations|`title`, `content`, `actions`|
|**SnackBar**|Temporary message overlay|`content`, `action`, `duration`|
## **Async and State Management Widgets**

|Widget|Purpose/Usage|Key Slots/Properties|
|---|---|---|
|**FutureBuilder**|Builds UI based on a Future’s state|`future`, `builder`|
|**StreamBuilder**|Builds UI based on a Stream’s state|`stream`, `builder`|
|**InheritedWidget**|Passes data down the widget tree efficiently|`child`, custom data fields|
|**Provider**|(from package) State management and dependency injection|`create`, `child`, `builder`|
## **Animation and Effects Widgets**

|Widget|Purpose/Usage|Key Slots/Properties|
|---|---|---|
|**AnimatedContainer**|Animates changes to its properties|`duration`, `child`, `decoration`, `width`, `height`|
|**Hero**|Hero animations between routes|`tag`, `child`|
|**FadeTransition**|Fades a widget in/out|`opacity`, `child`|
|**AnimatedBuilder**|Custom animations|`animation`, `builder`, `child`|
## **Specialty and Utility Widgets**

| Widget              | Purpose/Usage                                     | Key Slots/Properties                                      |
| ------------------- | ------------------------------------------------- | --------------------------------------------------------- |
| **SafeArea**        | Avoids system UI intrusions (notches, status bar) | `child`, `minimum`, `maintainBottomViewPadding`           |
| **MediaQuery**      | Accesses screen size, orientation, etc.           | `data`, `child`                                           |
| **GestureDetector** | Detects gestures (tap, drag, etc.)                | `child`, gesture callbacks (`onTap`, `onPanUpdate`, etc.) |
| **Placeholder**     | Shows a placeholder box (for layout debugging)    | `color`, `strokeWidth`                                    |
# Widget Types and Customization
## Stateless and Stateful Widgets
Stateless widgets are also called static widgets as they are only used for display and interaction of non-changing data:

```dart
import 'package:flutter/material.dart';

class MyStatelessWidget extends StatelessWidget {
  final String title;

  MyStatelessWidget({required this.title});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Stateless Widget Example"),
      ),
      body: Center(
        child: Text(
          title,
          style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
        ),
      ),
    );
  }
}

void main() {
  runApp(MaterialApp(
    home: MyStatelessWidget(title: "Hello, Stateless Widget!"),
  ));
}
```

Stateful widgets are used to create elements that change with state:

```dart
import 'package:flutter/material.dart';

class ToggleTextWidget extends StatefulWidget {
  @override
  _ToggleTextWidgetState createState() => _ToggleTextWidgetState();
}

class _ToggleTextWidgetState extends State<ToggleTextWidget> {
  bool _isVisible = true;

  void _toggleVisibility() {
    setState(() {
      _isVisible = !_isVisible;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("Stateful Widget Example")),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            if (_isVisible)
              Text(
                'Now you see me!',
                style: TextStyle(fontSize: 24),
              ),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: _toggleVisibility,
              child: Text(_isVisible ? 'Hide Text' : 'Show Text'),
            ),
          ],
        ),
      ),
    );
  }
}

void main() {
  runApp(MaterialApp(
    home: ToggleTextWidget(),
  ));
}
```

The **setState()** method is responsible for prompting flutter to re-render the widget with updated state.
## Theme and Styling
Flutter uses the **ThemeData** class for consistent color and typography across the app. The ThemeData object is applied to the MaterialApp widget of our flutter app.

```dart
MaterialApp(
  title: 'My App',
  theme: ThemeData(
    colorScheme: ColorScheme.fromSeed(seedColor: Colors.purple, brightness: Brightness.dark),
    textTheme: TextTheme(
      displayLarge: TextStyle(fontSize: 72, fontWeight: FontWeight.bold),
      titleLarge: TextStyle(fontSize: 30, fontStyle: FontStyle.italic),
      bodyMedium: TextStyle(fontSize: 16),
    ),
  ),
  home: MyHomePage(),
);
```

To use this theme in the children widgets, we use the Theme object:

```dart
Container(
  color: Theme.of(context).colorScheme.primary,
  child: Text(
    'Styled by Theme',
    style: Theme.of(context).textTheme.bodyMedium!.copyWith(
      color: Theme.of(context).colorScheme.onPrimary,
    ),
  ),
);
```

To define separate theme for dark mode, we use the darkMode slot in the MaterialApp widget.
## Custom Widgets and Drawing
To make a custom widget in flutter, we can define variables and a constructor as so:

```dart
class CustomButton extends StatelessWidget {
  final String text;
  final VoidCallback onPressed;

  CustomButton({required this.text, required this.onPressed});

  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: onPressed,
      child: Text(text),
    );
  }
}
```

**CustomPainter** is a powerful tool in flutter that will let you draw any shape, line, pattern or even complex graphics onto a blank canvas in your flutter app.

```dart
class MyCirclePainter extends CustomPainter {
  @override
  void paint(Canvas canvas, Size size) {
    Paint paint = Paint()
      ..color = Colors.blue
      ..style = PaintingStyle.stroke
      ..strokeWidth = 5;
    canvas.drawCircle(Offset(size.width/2, size.height/2), 50, paint);
  }

  @override
  bool shouldRepaint(CustomPainter oldDelegate) => false;
}

// Use it in your widget tree:
CustomPaint(
  size: Size(200, 200),
  painter: MyCirclePainter(),
)
```

`shouldRepaint` should usually compare the current and old delegate if the painting depends on external data.
## Animations
We can easily add animations to flutter using the following AnimationContainer object:

```dart
AnimatedContainer(
  duration: Duration(seconds: 1),
  width: _selected ? 200 : 100,
  height: _selected ? 200 : 100,
  color: _selected ? Colors.blue : Colors.red,
  child: FlutterLogo(),
)
```
# App Structure 
## Navigation and Routing
Flutter uses a stack (LIFO) based navigation system in which each new screen is pushed on to the stack and popping removes the top screen revealing the one beneath. 
You can choose one of four routing techniques in Flutter: Navigator 1.0, Navigator 2.0 (with go_router) and Named Routes.
We can use Navigator 1.0 for easy navigation in flutter:

```dart
// Navigate to SecondScreen
Navigator.push(
  context,
  MaterialPageRoute(builder: (context) => SecondScreen(
	  // ...slots
  )),
);

// Return to previous screen
Navigator.pop(context);

```
## Async UI and Builders
When we want to make API calls from our app, we use Builders which fetch data asynchronously and help us render the loading screen. We mainly do this by using two builders:
- **FutureBuilder:** Used to show things that have to be fetched once and shown
- **StreamBuilder:** Used to show things that have to be periodically fetched and displayed continuously such as sockets.

```dart
FutureBuilder<String>(
  future: fetchData(), // async function that returns a String
  builder: (context, snapshot) {
    if (snapshot.connectionState == ConnectionState.waiting) {
      return CircularProgressIndicator(); // loading spinner
    } else if (snapshot.hasError) {
      return Text('Error: ${snapshot.error}');
    } else {
      return Text('Result: ${snapshot.data}');
    }
  },
)

StreamBuilder<int>(
  stream: counterStream(), // async function that yields numbers
  builder: (context, snapshot) {
    if (snapshot.connectionState == ConnectionState.active) {
      return Text('Count: ${snapshot.data}');
    } else {
      return CircularProgressIndicator();
    }
  },
)
```