# LIVE CODING:

## 🖨️ Задача: ConsolePrinter и Wrapper класове

Реализирайте клас `ConsolePrinter`, който отпечатва низове в конзолата. Той трябва да поддържа **предикатна функция**, която определя кои символи да бъдат цензурирани (заменяни със `*`).

Създайте два допълнителни класа:
- **`NumberWrapper`** – съдържа цяло число и използва `ConsolePrinter` за отпечатване.
- **`StringWrapper`** – съдържа низ и използва `ConsolePrinter` за отпечатване.

## 🔍 Пример:
### Вход:
```cpp
ConsolePrinter noNumbers([](char ch) { return ch >= '0' && ch <= '9'; });
StringWrapper sr("Test3432wrwe", noNumbers);
sr.print();
```
### Изход:
```
Test****wrwe
```

## 📌 Изисквания
- `ConsolePrinter` използва предикатна функция за цензуриране.
- `NumberWrapper` и `StringWrapper` използват **агрегация** – не създават нов `ConsolePrinter`, а използват подаден като параметър.
- Използвайте **lambda функции** за предаване на предикатите.
