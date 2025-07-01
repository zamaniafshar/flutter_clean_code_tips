
# 🧼 Clean Functions – Simplified for Flutter & Dart Developers

> Based on Chapter 3 of *Clean Code* by Robert C. Martin  
> Adapted for Dart/Flutter developers to write clean, maintainable, testable functions

---

## 📌 Why Clean Functions Matter

In Flutter and Dart, your functions build UIs, fetch data, handle user actions, and manage state. Clean functions are the foundation of readable, maintainable, and bug-free codebases.

Bad functions lead to confusion, tightly-coupled code, and untestable widgets or logic.



---

## ✳️ Principles of Clean Functions

### 1. **Do One Thing**
> FUNCTIONS SHOULD DO ONE THING. THEY SHOULD DO IT WELL.
THEY SHOULD DO IT ONLY.

✅ Good:
```dart
void saveUser(User user) {
  userRepository.save(user);
}
```

❌ Bad:
```dart
void saveUser(User user) {
  userRepository.save(user);
  Navigator.of(context).pushReplacementNamed('/home');
}
```

---

### 2. **Short Functions**
> The shorter, the better. Aim for 5–15 lines max.

Split long functions into smaller, named chunks that describe what they do.

---

### 3. **Descriptive Names**
> Function names should clearly state *what* they do.

✅ Good:
```dart
Future<void> fetchAndDisplayPosts() async { ... }
```

❌ Bad:
```dart
Future<void> handleStuff() async { ... }
```

---

### 4. **Function Arguments**
> Ideal: 0–2 arguments. 3 is a code smell. Avoid flags (`bool` parameters).

✅ Good:
```dart
void showUserProfile(User user) { ... }
```

❌ Bad:
```dart
void showUserProfile(User user, bool isAdmin, int mode) { ... }
```

Prefer wrapping multiple arguments in a class:
```dart
class UserContext {
  final User user;
  final bool isAdmin;
  final int mode;
}

void showUserProfile(UserContext context) { ... }
```

---

### 5. **No Side Effects**
> Functions should do what their name says and nothing else.

Avoid unrelated operations like navigation or logging inside logic functions.

---

### 6. **Use Exceptions Instead of Returning Error Codes**
> In Dart, use `try-catch`, `Result`, or `Either` types.

✅ Good:
```dart
Future<Result<User>> loadUser(String id) async {
  try {
    final user = await api.getUser(id);
    return Result.success(user);
  } catch (e) {
    return Result.failure(e);
  }
}
```

---

### 7. **Don’t Repeat Yourself (DRY)**

If you repeat a code block 2–3 times, move it into a function with a meaningful name.

✅ Good:
```dart
void showError(BuildContext context, String message) {
  ScaffoldMessenger.of(context).showSnackBar(SnackBar(content: Text(message)));
}
```

---

### 8. **Prefer Command-Query Separation**

A function should either:
- **Do something (command)** → e.g., `saveUser()`
- **Return something (query)** → e.g., `isLoggedIn()`
But not both.

---

### 9. **Avoid Output Arguments**

Return data instead of modifying input parameters.

❌ Bad:
```dart
void fillList(List<int> list) {
  list.addAll([1, 2, 3]);
}
```

✅ Good:
```dart
List<int> createNumberList() {
  return [1, 2, 3];
}
```

---

### 10. **Keep Indentation Low**

Deep nesting makes code hard to read. Extract logic into smaller functions.

---

## 💡 Flutter-Specific Examples

Avoid mixing UI with logic. Extract decisions into smaller helpers.

❌ Bad:
```dart
Widget build(BuildContext context) {
  if (user.isLoggedIn && user.isVerified) {
    return HomeScreen();
  } else {
    return LoginScreen();
  }
}
```

✅ Good:
```dart
Widget build(BuildContext context) {
  return _buildScreenForUser(user);
}

Widget _buildScreenForUser(User user) {
  if (_canAccessHome(user)) return HomeScreen();
  return LoginScreen();
}

bool _canAccessHome(User user) => user.isLoggedIn && user.isVerified;
```

---

## 🧪 Clean Functions Are Testable

Functions that:
- Do one thing
- Have no side effects
- Return values (instead of mutating)
→ Are easy to test in isolation.

---

## ✅ Checklist for Clean Dart Functions

- [ ] Does it do only one thing?
- [ ] Is the name descriptive?
- [ ] Is it short (preferably under 15 lines)?
- [ ] Does it avoid flags and more than 2 arguments?
- [ ] Does it have no side effects?
- [ ] Can it be tested in isolation?
- [ ] Is logic separate from UI?

---

## 📚 Recommended Next Step

Refactor one of your large `build()` methods or controller functions using these principles. Make it:
- Easier to read
- Easier to change
- Easier to test

---

> ✨ “Functions are the first line of organization in any system.”  
> – Robert C. Martin

---

## 🛠️ Author’s Note

This document adapts the core concepts of *Clean Code* – Chapter 3 for Dart and Flutter. The goal is not strict rule-following, but **thoughtful design and communication through code**.
