# Expense Tracker

A Flutter expense tracking app built to practice intermediate Flutter concepts like state management, form input, validation, modal overlays, responsive UI, list interactions, and app-wide theming.

## Features

- Add expenses with:
	- title
	- amount
	- date picker
	- category selection
- Input validation with feedback dialog
- View expenses in a scrollable list
- Swipe to delete expense
- Undo deletion with `SnackBar`
- Expense chart by category
- Responsive layout:
	- vertical layout on smaller screens
	- side-by-side chart/list layout on wider screens
- Light and dark theme support

## Tech Stack

- Flutter
- Dart
- Material UI widgets

## Project Structure

```text
lib/
	main.dart
	models/
		expense.dart
	widgets/
		expenses.dart
		expenses_list.dart
		expense_item.dart
		new_expense.dart
		chart/
			chart.dart
			chart_bar.dart
```

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/jayasuriya-it21/Expense_Tracker.git
cd Expense_Tracker/expensetracker
```

### 2. Install dependencies

```bash
flutter pub get
```

### 3. Run the app

```bash
flutter run
```

## Run Checks

Static analysis:

```bash
flutter analyze
```

Widget tests:

```bash
flutter test
```

## SDK Compatibility Note

This project currently targets a Dart `2.19+` compatible setup in `pubspec.yaml` to support online compiler environments.

```yaml
environment:
	sdk: ">=2.19.0 <4.0.0"
```

## Learning Goals Covered

- Building reusable widgets
- Managing state in `StatefulWidget`
- Working with lists and `ListView.builder`
- Handling user input with controllers
- Date picking and dropdown input
- Displaying modal bottom sheets and dialogs
- Using dismiss gestures and undo actions
- Applying theme-based app styling
- Creating simple chart visualizations
