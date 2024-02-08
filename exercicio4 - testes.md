- Escreva testes unitários para uma função de validação de email.

Primeiramente defino validator em um local separado.

```dart
class Validator {
  static RegExp emailRegExp = RegExp(r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$');
  static isEmailValid(String? v){
    if(v == null) return false;
    return emailRegExp.hasMatch(v);
  }
}
```

Depois crio a estrutura de testes unitarios

```dart
void main() {
  group('Email Test', () {
    test('returns false if email field provides a null type', () {
      expect(Validator.isEmailValid(null), false);
    });
    test('returns true for valid email', () {
      expect(Validator.isEmailValid('test@example.com'), true);
    });

    test('returns false for invalid email without @', () {
      expect(Validator.isEmailValid('testexample.com'), false);
    });

    test('returns false for invalid email without domain', () {
      expect(Validator.isEmailValid('test@'), false);
    });

    test('returns false for invalid email with missing . in domain', () {
      expect(Validator.isEmailValid('test@examplecom'), false);
    });
  });
}
```

  - Escreva testes de widgets para um botão de login que se habilita apenas quando o formulário é válido.

```dart
import 'package:flutter/material.dart';
import 'package:flutter_test/flutter_test.dart';

import 'login_button.dart';

void main() {
  group('Test Login Button Widget', () {
    testWidgets('Button is enabled when form is valid', (widgetTester) async {
      final formkey = GlobalKey<FormState>();
      bool isValid = false;
      await widgetTester.pumpWidget(MaterialApp(
        home: Scaffold(
          body: StatefulBuilder(builder: (context, setState) {
            return Form(
              key: formkey,
              child: Column(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  TextFormField(
                    key: const ValueKey('emailField'),
                    controller: TextEditingController(),
                    onChanged: (value) {
                      isValid = formkey.currentState?.validate() ?? false;
                      setState(() {});
                    },
                    validator: (v) {
                      return null;
                    },
                  ),
                  Visibility(
                    visible: isValid,
                    child: ElevatedButton(
                      onPressed: () {},
                      child: Text(
                        'Login',
                        style: TextStyle(color: isValid ? null : Colors.grey),
                      ),
                    ),
                  ),
                ],
              ),
            );
          }),
        ),
      ));

      await widgetTester.pumpAndSettle();
      await widgetTester.enterText(
          find.byKey(const ValueKey('emailField')), 'this is a test');
      await widgetTester.pumpAndSettle();
      final button = find.byType(ElevatedButton);
      expect(button, findsOneWidget);
      expect(widgetTester.widget<ElevatedButton>(button).onPressed, isNotNull);
    });
  });
  testWidgets('Button is disabled when form is invalid', (widgetTester) async {
    final formkey = GlobalKey<FormState>();
    bool isValid = false;
    await widgetTester.pumpWidget(MaterialApp(
      home: Scaffold(
        body: StatefulBuilder(builder: (context, setState) {
          return Form(
            key: formkey,
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                TextFormField(
                  key: const ValueKey('emailField'),
                  controller: TextEditingController(),
                  onChanged: (value) {
                    isValid = formkey.currentState?.validate() ?? false;
                    setState(() {});
                  },
                  validator: (v) {
                    if (v?.isEmpty ?? true) return 'Invalid email';
                    return null;
                  },
                ),
                Visibility(
                    visible: isValid,
                    child: ElevatedButton(
                      onPressed: () {},
                      child: Text(
                        'Login',
                        style: TextStyle(color: isValid ? null : Colors.grey),
                      ),
                    ),
                  ),
              ],
            ),
          );
        }),
      ),
    ));

    await widgetTester.pumpAndSettle();

    final button = find.byType(ElevatedButton);
    expect(button, findsNothing);
  });
}
```
```



