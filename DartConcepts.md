Here is a complete, structured breakdown of every concept used in this project — ordered from most fundamental to most advanced, mapped to the actual files where you can see each concept in action.

---

## DART LANGUAGE CONCEPTS

### 1. Variables & Data Types
Used in every file.
```dart
final String id;       // String
final double amount;   // Double (decimal number)
final DateTime date;   // Built-in date type
double sum = 0;        // Mutable variable
```
- `final` = value assigned once, can't be reassigned
- `var` = Dart infers the type automatically
- `const` = compile-time constant (value known before the app runs)
- Core types: `String`, `int`, `double`, `bool`, `DateTime`

---

### 2. Classes & Objects
lib/models/expense.dart
```dart
class Expense {
  final String id;
  final String title;
  ...
}
```
- A **class** is a blueprint for creating objects
- An **object** is one real instance of that class
- `Expense(title: 'Food', ...)` creates one expense object

---

### 3. Constructors
lib/models/expense.dart
```dart
Expense({
  required this.title,
  required this.amount,
  ...
}) : id = DateTime.now().microsecondsSinceEpoch.toString();
```
- **Named parameters** with `{}` — called by name: `Expense(title: 'x', ...)`
- **`required`** — makes a named parameter mandatory
- **Positional parameters** without `{}` — order matters: `ExpenseItem(expense)`
- **Initializer list** (`: id = ...`) — sets a property before the constructor body runs
- **Named constructors** (`ExpenseBucket.forCategory(...)`) — alternate ways to construct an object

---

### 4. `enum` — Enumeration
lib/models/expense.dart
```dart
enum Category { food, travel, leisure, work }
```
- Defines a fixed set of allowed values
- Prevents typos — Dart gives you an error if you use an unknown value
- Access values like: `Category.food`, `Category.work`
- `.name` gives you the string form: `Category.food.name` → `"food"`

---

### 5. Getters
lib/models/expense.dart
```dart
String get formattedDate {
  return '${date.month}/${date.day}/${date.year}';
}
```
- A **getter** looks like a property but computes a value on the fly
- Called like a property (no `()`): `expense.formattedDate`

---

### 6. Collections — `List` and `Map`
Used throughout.
```dart
List<Expense> _registeredExpenses = [];   // List (ordered)
const categoryIcons = {                    // Map (key → value)
  Category.food: Icons.lunch_dining,
  ...
};
```
- **List** — ordered collection, access by index `list[0]`
- **Map** — key-value pairs, access by key `categoryIcons[Category.food]`
- `.add()`, `.remove()`, `.insert()`, `.indexOf()`, `.length`, `.isEmpty`, `.isNotEmpty`, `.where()`, `.map()`, `.toList()`

---

### 7. String Interpolation
lib/widgets/expense_item.dart
```dart
'${expense.amount.toStringAsFixed(2)}'
'${date.month}/${date.day}/${date.year}'
```
- `$variableName` — inject a simple variable into a string
- `${expression}` — inject any expression into a string
- `\$` — escaped dollar sign, outputs literal `$`

---

### 8. Null Safety & Nullable Types
lib/widgets/new_expense.dart
```dart
DateTime? _selectedDate;      // nullable — can be null
_selectedDate!                // null assertion — "I know it's not null"
double.tryParse(...)          // returns double? (null if parse fails)
```
- `?` after a type means it can hold `null`
- `!` asserts the value is not null (crashes if it is)
- `??` — null fallback: `value ?? defaultValue`

---

### 9. Functions as Values (Callbacks)
lib/widgets/expenses_list.dart
```dart
final void Function(Expense expense) onRemoveExpense;
```
- Functions in Dart are first-class — you can store them in variables and pass them as arguments
- This is how child widgets communicate back to parent widgets

---

### 10. Arrow Functions & Anonymous Functions
lib/widgets/expenses_list.dart
```dart
itemBuilder: (ctx, index) => Dismissible(...)   // arrow (single expression)

onChanged: (value) {                             // anonymous function (block body)
  setState(() { _selectedCategory = value; });
}
```

---

### 11. `async` / `await` — Asynchronous Code
lib/widgets/new_expense.dart
```dart
void _presentDatePicker() async {
  final pickedDate = await showDatePicker(...);
  setState(() { _selectedDate = pickedDate; });
}
```
- `async` = this function may do time-consuming work
- `await` = pause here until the result is ready, then continue
- Used for things that take time: date picker dialog, network calls, file I/O

---

### 12. `for` loops and collection iteration
lib/widgets/chart/chart.dart
```dart
for (final bucket in buckets)      // for-in loop
  ChartBar(fill: ...)

for (final expense in expenses) {  // standard for-in
  sum += expense.amount;
}
```

---

### 13. `if` inside Widget trees (Collection `if`)
lib/widgets/new_expense.dart
```dart
children: [
  if (width >= 600)
    Row(...)     // only added when condition is true
  else
    TextField(...),
]
```
- Dart lets you use `if/else` directly inside a list literal

---

### 14. `for` inside Widget trees (Collection `for`)
lib/widgets/chart/chart.dart
```dart
children: [
  for (final bucket in buckets)
    ChartBar(fill: ...)
]
```

---

## FLUTTER FRAMEWORK CONCEPTS

### 15. `StatelessWidget`
Used in: expense_item.dart, expenses_list.dart, chart.dart, chart_bar.dart
- A widget with **no changing data**
- Rendered once; rebuilt only if its parent passes new data
- Must override `build(BuildContext context)`

---

### 16. `StatefulWidget` + `setState`
Used in: expenses.dart, new_expense.dart
```dart
class Expenses extends StatefulWidget { ... }
class _ExpensesState extends State<Expenses> { ... }

setState(() {
  _registeredExpenses.add(expense);
});
```
- Two classes: the **widget** (immutable) + the **state** (mutable)
- `setState()` — tells Flutter "data changed, please rebuild the UI"
- State class name starts with `_` (private by convention)

---

### 17. Widget Tree & `BuildContext`
Every `build()` method receives a `BuildContext`.
- The **widget tree** is the nested structure of all your widgets
- `context` gives you access to the theme, screen size, navigator, etc.
- `Theme.of(context)`, `MediaQuery.of(context)`, `Navigator.of(context)` all use it

---

### 18. Layout Widgets
| Widget | Purpose | Where used |
|---|---|---|
| `Column` | Stack children vertically | expense_item.dart, expenses.dart |
| `Row` | Place children horizontally | expense_item.dart, new_expense.dart |
| `Expanded` | Fill remaining space in Row/Column | new_expense.dart, chart_bar.dart |
| `Spacer` | Push widgets apart in Row/Column | expense_item.dart |
| `Padding` | Add internal spacing | expense_item.dart |
| `SizedBox` | Fixed-size gap | expense_item.dart, new_expense.dart |
| `Center` | Center a child | expenses.dart |
| `SingleChildScrollView` | Make content scrollable | new_expense.dart |

---

### 19. `Scaffold` & `AppBar`
lib/widgets/expenses.dart
```dart
Scaffold(
  appBar: AppBar(
    title: const Text('Flutter ExpenseTracker'),
    actions: [ IconButton(...) ],
  ),
  body: Column(...),
)
```
- `Scaffold` — standard page structure (background, appbar, body, snackbar host)
- `AppBar` — top navigation bar; `actions` adds buttons on the right

---

### 20. Common UI Widgets
| Widget | Purpose |
|---|---|
| `Text` | Display text |
| `Icon` | Display a Material icon |
| `Card` | Elevated container with shadow |
| `Container` | Box with decoration, padding, margin |
| `IconButton` | Tappable icon |
| `TextButton` | Text-only button |
| `ElevatedButton` | Filled button |
| `TextField` | Text input field |
| `DropdownButton` | Dropdown menu selector |

---

### 21. `ListView.builder`
lib/widgets/expenses_list.dart
```dart
ListView.builder(
  itemCount: expenses.length,
  itemBuilder: (ctx, index) => ExpenseItem(expenses[index]),
)
```
- Builds list items **lazily** — only creates items currently visible on screen
- Much more performant than `Column` for long or unknown-length lists
- `itemCount` tells Flutter how many items exist
- `itemBuilder` is called once per visible item with the current `index`

---

### 22. `Dismissible` — Swipe to Delete
lib/widgets/expenses_list.dart
```dart
Dismissible(
  key: ValueKey(expenses[index]),
  background: Container(color: Colors.red),
  onDismissed: (direction) { onRemoveExpense(expenses[index]); },
  child: ExpenseItem(...),
)
```
- Wraps a widget to make it swipeable to dismiss
- `key` is required — uniquely identifies each item
- `background` — what shows behind the item as it slides

---

### 23. `Key` & `ValueKey`
```dart
key: ValueKey(expenses[index])
```
- Flutter uses **keys** to track widget identity when lists change
- `ValueKey` creates a key from any value (here, the expense object)

---

### 24. Modal Bottom Sheet
lib/widgets/expenses.dart
```dart
showModalBottomSheet(
  context: context,
  isScrollControlled: true,
  builder: (ctx) => NewExpense(onAddExpense: _addExpense),
);
```
- A panel that slides up from the bottom of the screen
- `isScrollControlled: true` — lets the sheet grow to full height if needed
- `useSafeArea: true` — avoids overlapping with system UI

---

### 25. `AlertDialog` — Validation Feedback
lib/widgets/new_expense.dart
```dart
showDialog(
  context: context,
  builder: (ctx) => AlertDialog(
    title: const Text('Invalid input'),
    content: const Text('Please fill in all fields.'),
    actions: [ TextButton(onPressed: () => Navigator.pop(ctx), ...) ],
  ),
);
```
- A pop-up dialog for user feedback
- `actions` — buttons at the bottom of the dialog

---

### 26. `SnackBar` + Undo Action
lib/widgets/expenses.dart
```dart
ScaffoldMessenger.of(context).showSnackBar(
  SnackBar(
    content: Text('Expense deleted.'),
    action: SnackBarAction(label: 'Undo', onPressed: () { ... }),
  ),
);
```
- Brief notification at the bottom of the screen
- `SnackBarAction` — adds an actionable button (here, Undo)
- `ScaffoldMessenger.of(context)` — finds the nearest Scaffold to show the bar on

---

### 27. `Navigator` — Closing Screens
lib/widgets/new_expense.dart
```dart
Navigator.pop(context);
```
- Closes the current screen/modal and returns to the previous one

---

### 28. `TextEditingController`
lib/widgets/new_expense.dart
```dart
final _titleController = TextEditingController();
TextField(controller: _titleController)
_titleController.text   // read input value
```
- Links a `TextField` to a controller so you can read and clear its value
- Must be **disposed** when the widget is removed to free memory

---

### 29. `dispose()` — Resource Cleanup
lib/widgets/new_expense.dart
```dart
@override
void dispose() {
  _titleController.dispose();
  _amountController.dispose();
  super.dispose();
}
```
- Called when the widget is removed from the tree
- Always dispose controllers, animations, streams to prevent memory leaks

---

### 30. `showDatePicker`
lib/widgets/new_expense.dart
```dart
final pickedDate = await showDatePicker(
  context: context,
  initialDate: now,
  firstDate: firstDate,
  lastDate: now,
);
```
- Built-in Material date picker dialog
- Returns `DateTime?` (null if user cancels)

---

### 31. `MediaQuery` — Screen Info
lib/widgets/expenses.dart and new_expense.dart
```dart
final width = MediaQuery.of(context).size.width;
final keyboardSpace = MediaQuery.of(context).viewInsets.bottom;
MediaQuery.of(context).platformBrightness  // light or dark mode
```
- Gives you screen dimensions, keyboard height, orientation, brightness
- Used here for **responsive layout** — different layouts for wide vs narrow screens

---

### 32. `LayoutBuilder` — Constraint-based Layout
lib/widgets/new_expense.dart
```dart
LayoutBuilder(builder: (ctx, constraints) {
  final width = constraints.maxWidth;
  if (width >= 600) { ... } else { ... }
})
```
- Provides the **available space** for the current widget (not the full screen)
- More reliable than `MediaQuery` for sub-widget responsive behavior

---

### 33. App-wide Theming
lib/main.dart
```dart
MaterialApp(
  theme: ThemeData().copyWith(
    colorScheme: kColorScheme,
    appBarTheme: ...,
    elevatedButtonTheme: ...,
    textTheme: ...,
  ),
  darkTheme: ThemeData.dark().copyWith(...),
  themeMode: ThemeMode.system,
)
```
- `ThemeData` — defines colors, fonts, button styles app-wide
- `ColorScheme.fromSeed()` — generates a harmonious color palette from one seed color
- `ThemeMode.system` — automatically follows device light/dark setting
- `Theme.of(context).colorScheme` — access theme colors anywhere in the tree

---

### 34. `BoxDecoration` — Custom Styling
lib/widgets/chart/chart.dart
```dart
decoration: BoxDecoration(
  borderRadius: BorderRadius.circular(8),
  gradient: LinearGradient(...),
),
```
- Used on `Container` or `DecoratedBox` for rounded corners, gradients, borders, shadows

---

### 35. `FractionallySizedBox`
lib/widgets/chart/chart_bar.dart
```dart
FractionallySizedBox(
  heightFactor: fill,   // 0.0 to 1.0 — proportion of parent height
  child: DecoratedBox(...),
)
```
- Sizes its child as a **fraction** of the parent's size
- Used here to draw bars proportional to the max expense amount

---

### 36. `@override`
Every `build()` method.
```dart
@override
Widget build(BuildContext context) { ... }
```
- Tells Dart you're intentionally overriding a method from the parent class
- Good practice — the compiler will warn you if the method doesn't exist in the parent

---

### 37. `const` Constructor & Widgets
```dart
const Text('Hello')
const SizedBox(height: 8)
const EdgeInsets.symmetric(horizontal: 20)
```
- Adding `const` means the widget object is created once and reused
- Improves performance — Flutter skips rebuilding `const` widgets

---

## SUMMARY MAP

```
Expense Tracker
│
├── Dart Fundamentals
│   ├── Variables, final, const, var
│   ├── Types: String, double, int, bool, DateTime
│   ├── Classes, constructors, named parameters
│   ├── enum
│   ├── Getters
│   ├── List, Map
│   ├── String interpolation
│   ├── Null safety (?, !, ??)
│   ├── Functions as values / callbacks
│   ├── Arrow & anonymous functions
│   ├── async / await
│   ├── for loops, collection if/for
│   └── @override
│
└── Flutter Framework
    ├── StatelessWidget
    ├── StatefulWidget + setState
    ├── Widget tree + BuildContext
    ├── Layout: Column, Row, Expanded, Spacer, Padding, SizedBox
    ├── Scaffold + AppBar
    ├── Text, Icon, Card, Container, Buttons
    ├── ListView.builder (lazy loading)
    ├── Dismissible (swipe to delete)
    ├── Keys & ValueKey
    ├── Modal Bottom Sheet
    ├── AlertDialog
    ├── SnackBar + Undo
    ├── Navigator (pop)
    ├── TextEditingController + dispose()
    ├── showDatePicker
    ├── MediaQuery (screen size, keyboard, brightness)
    ├── LayoutBuilder (responsive layout)
    ├── App-wide ThemeData + ColorScheme + dark theme
    ├── BoxDecoration (gradients, rounded corners)
    ├── FractionallySizedBox
    └── const widgets (performance optimization)
```

This is everything in your current codebase. Each concept builds on the previous, so starting from **variables → classes → StatelessWidget → StatefulWidget → Layout → State management → Forms → Theming** is the natural learning order.