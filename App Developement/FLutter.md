# Dart Language 
Variable declaration can either be implicit `var str = "Hello World"` or explicit `String str = "Hello World"`. Dart offers data types like `String, int, bool, double, etc...`. Dart offers two keywords to store constant values:
- **final:** initialized at run-time
- **const**: initialized at compile-time
`final String str = "Hello World"`

## Function
```dart
int addNumbers(int a, int b) {
	return a+b;
}
```

## Classes
```dart
class Dog {
	String? name;
	int? age;
	bool? isGoodBoy;
	Dog(this.name);
	void bark() {
		print("Woof");
	}
}
class Husky extends Dog {
	String? eyeColour;
	Husky(String name, this.eyeColour) : super(name);
	@override
	void bark() {
		print("Howwwwwww");
	}
}

void main() {
	Dog oreo = Dog("Oreo");
	oreo.age = 2;
	oreo.isGoodBoy = true;
	oreo.bark();
}
```

## Conditionals
```dart
void bark() {
	if(age >= 2) {
		print("Wooooof");
	} else if (age < 2 && age >= 1) {
		print("Woooof");
	}
	else {
		print("Woof");
	}
}
```

## Loops
```dart
// For Loop
for(var i=0; i<10; i++) {
	print('iteration: $i');
}
// For in Loop
var people = ["Yash", "Adit", "Bharat"];
for (var person in people) {
	print(person);
}
// While Loop
int i=0;
while(i<=5) {
	print('Hello: $i');
	i++;
}
```

## Asynchronous Programming in Dart
A Future in Dart is like a JS Promise which will complete at some point in future. We pair it with asynchronous functions to make API calls:
```dart
Future<String> getMessage() async {
	try {
		var message = await fetchMessage();
		return 'Your message is: $message';
	} catch (err) {
		print("Error occured!");
		return '';
	}
}
```

Streams are used to handle a series of futures:
```dart
Stream<String> getMessages() async* {
	var message1 = await fetchMessage1();
	yield message1;
	var message2 = await fetchMessage2();
	yield message3;
	var message3 = await fetchMessage3();
	yield message3;
}
```

Futures and Streams can be consumed as follows:
```dart
void main() {
	printMessages(); // Asynchornous function
}
void printMessages() async {
	// Consuming Future
	var message = await getMessage();
	print(message);
	// Consuming Stream
	await for (var msg in getMessages()) {
		print(msg);
	}
}
```

## Collections
Collections help us to dynamically add, remove and manage data.
```dart
List<String> names = ['Alice', 'Bob'];
Set<int> numbers = {1, 2, 3};
Map<String, int> ages = {'Alice': 25, 'Bob': 30};
```

| **Operation Type**    | **List**                  | **Set**      | **Map**                             |
| --------------------- | ------------------------- | ------------ | ----------------------------------- |
| **Add single item**   | `add()` / `insert()`      | `add()`      | `map[key] = value`                  |
| **Remove item**       | `remove()` / `removeAt()` | `remove()`   | `remove(key)`                       |
| **Check if contains** | `contains()`              | `contains()` | `containsKey()` / `containsValue()` |
| **Get size**          | `length`                  | `length`     | `length`                            |
| **Clear all**         | `clear()`                 | `clear()`    | `clear()`                           |
| **Convert to other**  | `toSet()`                 | `toList()`   | `keys` / `values`                   |

# Flutter Funcdamentals
Each widget in flutter consists of a `build()` function which builds the widget tree by recurringly calling build function of its children. We make the widget tree by calling the `runApp()` method.
Elements in widget tree can either be Stateful or Stateless.
## Widget Hierarchy and Composition

Widgets form a **hierarchical structure based on composition**[3](https://docs.flutter.dev/get-started/fundamentals/widgets). Each widget can nest inside its parent and receive context from that parent, creating a tree structure that extends all the way to the root widget. This composition-based approach allows for flexible and reusable UI components.

```dart
MaterialApp( // Root widget   
	home: Scaffold(    
		appBar: AppBar(      
			title: const Text('My Home Page'),    
		),    
	body: 
		Center(      
			child: Column(        
				children: [
					const Text('Hello, World!'),    
					ElevatedButton(            
						onPressed: () => print('Click!'), 
						child: const Text('A button'),          
					),        
				],      
			),    
		),  
	), 
)
```

## Widget Categories

**Design Systems**
- **Material Components**: Implementing Material 3 design specification for Android-style interfaces
- **Cupertino**: High-fidelity widgets following Apple's Human Interface Guidelines for iOS

**Base Widget Categories**
- **Layout widgets**: Arrange other widgets in columns, rows, grids, and various layouts
- **Basic widgets**: Essential components like Text, Container, and buttons
- **Input widgets**: Handle user input beyond Material and Cupertino components
- **Styling widgets**: Manage themes, responsiveness, and visual appearance
- **Animation and motion widgets**: Bring dynamic effects to applications
- **Scrolling widgets**: Enable scrollable content with multiple children
## Core Widget Types

**Stateless Widgets**  
Stateless widgets are immutable and don't maintain any internal state. They're perfect for static content that doesn't change based on user interaction[4](https://www.dhiwise.com/post/flutter-widget-cheatsheet-basic-widgets-types)[7](https://www.bacancytechnology.com/blog/flutter-widgets):

```dart
class MyStatelessWidget extends StatelessWidget {   
	@override  
	Widget build(BuildContext context) {    
		return Text('This content never changes');  
	} 
}
```

**Stateful Widgets**  
Stateful widgets can change their appearance in response to events triggered by user interactions or when they receive data. Examples include checkboxes, radio buttons, forms, and text fields:

```dart
class MyStatefulWidget extends StatefulWidget {   
	@override  
	_MyStatefulWidgetState createState() => _MyStatefulWidgetState(); 
} 
class _MyStatefulWidgetState extends State<MyStatefulWidget> {   
	int counter = 0;     
	@override  
	Widget build(BuildContext context) {    
		return Text('Counter: $counter');  
	} 
}
```

## Essential Basic Widgets

**Layout Widgets**
- **Row**: Arranges children horizontally using flexbox layout model
- **Column**: Arranges children vertically
- **Stack**: Places widgets on top of each other in paint order
- **Container**: Combines painting, positioning, and sizing capabilities
**Content Widgets**
- **Text**: Displays styled text content
- **Image**: Shows images from various sources
- **Icon**: Displays Material Design icons
- **ElevatedButton**: Material Design elevated button that responds to user taps
**Specialized Widget Types**
- **RenderObjectWidget**: Provides configuration for RenderObjectElements that handle actual rendering
- **InheritedWidget**: Efficiently propagates information down the widget tree, useful for sharing data across multiple widgets
**ProxyWidget**: Contains a child widget instead of building new widgets, serving as a base for other specialized widgets
## Widget Interaction and Callbacks

Many widgets support user interaction through callback functions. For example, buttons like `IconButton`, `ElevatedButton`, and `FloatingActionButton` have `onPressed()` callbacks that trigger when users tap the widget
## Advanced Layout Capabilities
Flutter widgets support sophisticated layout patterns:
- **Flexible layouts**: Using `Expanded` and `Flexible` widgets within `Row` and `Column`
- **Responsive design**: Widgets that adapt to different screen sizes
- **Custom layouts**: Creating specialized layout behaviors through custom widgets

