# 02. Потоци и файлове. Текстови файлове.

### Видове потоци

Поток (stream) - последователност от байтове данни влизащи в и излизащи от програмата.

- При операциите за вход, байтовете идват от източник за вход (клавиатура, файл, мрежа или друга програма)
- При операциите за изход, байтовете данни излизат от програмата и се "вливат" във външно "устройство" (конзола, файл, мрежа или друга програма)
- Потоците служат като посредници между програмите и самите IO устройства по начин, който освобождава програмиста от боравене с тях.
- Потокът дефинира интерфейс с операции върху него, които не зависят от избора на IO устройство

  ![enter image description here](https://github.com/GeorgiTerziev02/Object-oriented_programming_FMI/raw/main/Sem.%2002/img/streams.png)  
  За да извърши вход или изход, една C++ програма:

  - Създава поток;
  - Свързва потока с IO устройството (напр. конзола, клавиатура, файл, мрежа или друга програма);
  - Извършва операции за вход/изход върху потока;
  - Прекъсва връзка с потока;
  - Освобождава потока;

Видове потоци:

- Потоци за вход ([istream](https://en.cppreference.com/w/cpp/io/basic_istream)).
- Потоци за изход ([ostream](https://en.cppreference.com/w/cpp/io/basic_ostream)).

![enter image description here](https://github.com/GeorgiTerziev02/Object-oriented_programming_FMI/raw/main/Sem.%2002/img/hierarchy.png)

##### Пример за работа с вход от конзолата. [cin](https://en.cppreference.com/w/cpp/io/cin) (обект от тип istream).

```c++
#include <iostream>

int main() {
	int a, b;
	std::cin >> a >> b; // read two integers from the console input stream
}
```

Четем от стандартния вход.

##### Пример за работа с поток за изход. [cout](https://en.cppreference.com/w/cpp/io/cout) (обект от тип ostream).

```c++
#include <iostream>

int main() {
	int a = 10;
	int b = 12;
	int c = a + b;

	std::cout << a << " + " << b << " = " << c << std::endl; // write two integers in the console output stream
}
```

### Работа с поток за вход от файл ([ifstream](https://en.cppreference.com/w/cpp/io/basic_ifstream))

Пример за четене от файл!

```c++
#include <iostream>
#include <fstream>

constexpr char FILE_NAME[] = "myFile.txt";

int main() {
	std::ifstream file(FILE_NAME); // create input file stream associated with myFile.txt

	if (!file.is_open()) {
		std::cout << "Error!" << std::endl;
		return -1;
	}

	int a, b;
	file >> a >> b;

	file.close();
}
```

Пример за прочитане на цялото съдържание файл:

```c++
#include <iostream>
#include <fstream>

constexpr int BUFF_SIZE = 1024;
constexpr char FILE_NAME[] = "myFile.txt";

int main() {
	std::ifstream file(FILE_NAME);

	if (!file.is_open()) {
		std::cout << "Error!" << std::endl;
		return -1;
	}

	while (!file.eof()) {
		char buff[BUFF_SIZE];
		file.getline(buff, BUFF_SIZE);

		//do something with the line
		std::cout << buff << std::endl;
	}

	file.close();
}
```

### Работа с поток за изход към файл ([ofstream](https://en.cppreference.com/w/cpp/io/basic_ofstream))

```c++
#include <iostream>
#include <fstream>

constexpr char FILE_NAME[] = "myFile.txt";

int main() {
	std::ofstream file(FILE_NAME);  // create output file stream associated with myFile.txt

	if (!file.is_open()) {
		std::cout << "Error!" << std::endl;
		return -1;
	}

	int a = 3;
	int b = 10;

	file << a << " " << b << " " << a + b << std::endl; // write into the output file stream

	if(!file.eof()) { //check if the file has ended
		std::cout << "The file contains more data after the two integers!" << std::endl;
	}

	file.close();
}
```

- (istream) [get](https://en.cppreference.com/w/cpp/io/basic_istream/get) - функция, която чете следващия character в потока.
- (ostream) [put](https://en.cppreference.com/w/cpp/io/basic_ostream/put) - функция, която поставя на следваща позиция character в потока.
- ifstream или istream - съдържа get указател, който реферира елемента, който ще се прочете при следващата входна операция.
- ofstream или ostream - съдържа put указател, който реферира мястото, където ще се запише следващият елемент.
- put и get не са [форматирани](https://www.geeksforgeeks.org/unformatted-input-output-operations-in-cpp/) за разлика от operator<< и operator>>, тоест не пропускат whitespaces и др.

### Позициониране във файл

- tellg() - Връща позицития на текущия символ в **потока за четене**
- tellp() - Връща позицития на текущия символ в **потока за писане**
- seekg(offset, direction) - Премества get-указателят на позцития на **потока за четене**.
- seekg(streampos idx) - Премества get-указателят на позция idx на **потока за четене**.
- seekp(offset, direction) - Премества put-указателят на позцития на **потока за писане**.
- seekp(streampos idx) - Премества put-указателят на позция idx на **потока за писане**.

-**offset** : целочислена стойност. Отместването от direction.

-**direction** : Може да заема следите стойностти:

1. ios::beg - началото на файла.
2. ios::cur - текущата позиция във файла.
3. ios::end - края на файла.

### [Режими на работа](https://en.cppreference.com/w/cpp/io/ios_base/openmode)

```c++
	ifstream str("file.txt", <режим на работа>).
```

Режимът на работа е цяло число. Ако искаме да зададем повече от един ги изреждаме с двоично или('|').

| ios              | Ефект:                                                                                                                           |     |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------------- | --- |
| ios::in          | Отваря файл за извличане.                                                                                                        | 1   |
| ios::out         | Отваряне на файл за вмъкване. Допуска се вмъкване на произволни места във файла. Ако файлът съществува, съдържанието се изтрива. | 2   |
| ios::ate         | Отваря за вмъкване и установява указателя put в края на файла. Допуска вмъкване на произволни места.                             | 4   |
| ios::app         | Отваря за вмъкване и установява указателя put в края на файла                                                                    | 8   |
| ios::trunc       | Ако файлът съществува, съдържанието се изтрива.                                                                                  | 16  |
| ios::binary      | Превключва режима от текстов в двоичен                                                                                           | 32  |
| ios::\_Nocreate  | Отваря за вмъкване, само ако файлът с указаното име съществува.                                                                  | 64  |
| ios::\_Noreplace | Отваря за вмъкване само ако файлът с указаното име не съществува.                                                                | 128 |

```c++
	ofstream file("file.txt", ios::out | ios::app).
```

### [Флагове на състоянията на потока](https://en.cppreference.com/w/cpp/io/ios_base/iostate)

| Флаг:                                                       | Значение:                                                                   |
| ----------------------------------------------------------- | --------------------------------------------------------------------------- |
| bad()                                                       | Има загуба на информация. Някоя операция за четене и писане не е изпълнена. |
| fail()                                                      | Последната входно/изходна операция е невалидна.                             |
| good()                                                      | Всички операции са изпълнени успешно.                                       |
| clear()                                                     | Изчиства състоянието на потока (Вече good() ще върне истина).               |
| [eof()](https://en.cppreference.com/w/cpp/io/basic_ios/eof) | Достигнат е края на файла.                                                  |

# Управление на грешки в файловите потоци в C++ (`fstream`)

Когато работим с файлови потоци в C++ (`std::fstream`, `std::ifstream`, `std::ofstream`), могат да възникнат грешки, които засягат функционалността на различни операции.

---

## **Флагове за грешки в `std::fstream`**

Файловите потоци в C++ (`std::fstream`) използват вътрешни флагове за посочване на различни видове грешки:

| Флаг      | Условие на задействане                                                    | Ефект върху потока                                                 |
| --------- | ------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| `eofbit`  | Достигнат е краят на файла (EOF)                                          | Спира четенето, но не означава сериозна грешка                     |
| `failbit` | Грешен формат на входа (например опит за четене на число от текст)        | Блокира входните операции като `>>`, `get()`, `seekg()`, `seekp()` |
| `badbit`  | Сериозна повреда на потока (например хардуерен проблем или повреден файл) | Напълно блокира потока, трудно за възстановяване                   |

---

## **Операции и тяхната работа при различни флагове**

| Операция     | Работи при `eofbit` | Работи при `failbit` | Работи при `badbit` |
| ------------ | ------------------- | -------------------- | ------------------- |
| `read()`     | ❌                  | ❌                   | ❌                  |
| `write()`    | ✅                  | ✅                   | ❌                  |
| `seekg()`    | ❌                  | ❌                   | ❌                  |
| `seekp()`    | ✅                  | ❌                   | ❌                  |
| `clear()`    | ✅                  | ✅                   | ✅                  |
| `eof()`      | ✅                  | ✅                   | ❌                  |
| `fail()`     | ✅                  | ✅                   | ❌                  |
| `bad()`      | ✅                  | ✅                   | ✅                  |
| `get()`      | ❌                  | ❌                   | ❌                  |
| `getline()`  | ❌                  | ❌                   | ❌                  |
| `operator>>` | ❌                  | ❌                   | ❌                  |
| `operator<<` | ✅                  | ✅                   | ❌                  |

**Забележка:**

- `clear()` винаги работи и може да бъде използван за възстановяване на потока.
- `seekg()` и `seekp()` не работят, докато има активен `failbit` или `badbit`.
- `operator<<` работи дори при `eofbit` и `failbit`, защото записът може да продължи въпреки EOF или грешен вход.

---

## **Кога се активират `failbit` и `badbit`?**

### **`failbit` – Грешен входен формат**

✅ **Пример: Опит за четене на число, но срещаме текст**

```cpp
std::ifstream ifs("file.txt");
int num;
ifs >> num; // Ако във файла има "hello", `failbit` ще се активира

if (ifs.fail()) {
    std::cout << "failbit е активен!" << std::endl;
}
```

❌ **Резултат:** `failbit` се активира, защото входът не е число.

---

### **`badbit` – Критична грешка в потока**

✅ **Пример: Ръчно задаване на `badbit` за симулиране на грешка**

```cpp
std::ifstream ifs("file.txt");
ifs.setstate(std::ios::badbit); // Симулиране на `badbit` грешка

if (ifs.bad()) {
    std::cout << "badbit е активен!" << std::endl;
}
```

🔴 **Ако `badbit` е активен, потокът става напълно неизползваем.**

---

### **`eofbit` – Достигнат е краят на файла**

✅ **Пример: Четене до края на файла**

```cpp
std::ifstream ifs("file.txt");
char ch;
while (ifs.get(ch)) {
    std::cout << ch;
}

if (ifs.eof()) {
    std::cout << "Край на файла!" << std::endl;
}
```

📌 **Забележка:** `eofbit` **не означава грешка**, но предотвратява по-нататъшно четене.

---

## **Как да изчистим грешките и да продължим работа?**

Ако някой от тези флагове се активира, **трябва да извикаме `clear()`**, за да възстановим нормалното състояние на потока.

✅ **Поправка с `clear()`**

```cpp
std::ifstream ifs("file.txt");

int num;
ifs >> num; // Ако четенето е неуспешно, `failbit` се активира

if (ifs.fail()) {
    std::cout << "failbit открит! Изчистване..." << std::endl;
    ifs.clear();  // Изчистване на всички грешки
    ifs.seekg(0); // Връщане на позицията на курсора в началото на файла
}
```

---

## **Обобщение**

| Флаг      | Причина                    | Ефект                              | Решение                |
| --------- | -------------------------- | ---------------------------------- | ---------------------- |
| `eofbit`  | Достигнат е краят на файла | Спира четенето                     | `clear(); seekg(0);`   |
| `failbit` | Грешен входен формат       | Блокира входа, `seekg()` не работи | `clear(); seekg();`    |
| `badbit`  | Критична повреда на потока | Потокът напълно спира да работи    | Обикновено непоправимо |
