
# Строки и байты

**Ключи:**
- Обратные:
	- [Сведения о языке Go](GO);
	- [Строки](Go-string);
- Прямые:
	- [Руны](Go-rune);

**Хештеги:** #Programming/Go/DataTypes/String

## Общие сведения

Строки в Go — это иммутабельные массивы байт. Для стандартного компилятора Go внутренняя структура строки описана следующим образом:

```go
type _string struct {
	elements *byte // байты
	len int // кол-во байт
}
```

После инициализации строку нельзя изменить и такая иммутабельность позволяет избежать побочных эффектов в коде:

```go
s := "hello"
s[4] = "" // ошибка компиляции: cannot assign to s[4] (strings are immutable)
```

Стоит отметить, что тип данных `byte` — это **алиас** к типу `uint8` (0-255). Во-первых, потому что нужно абстрактно отличать типы в коде. Во-вторых, байты представляют ASCII символы, а в кодовой таблице ASCII символов 256 кодов:

```go
package main

import "fmt"

func main() {
	s := "hey"
	
	fmt.Println(s[0], s[1], s[2]) // 104 101 121
	
	fmt.Println(string(s[0]), string(s[1]), string(s[2])) // h e y
}
```

Большинство библиотечных функций работают со слайсами байт `[]byte` для производительности. Конвертация строки в слайс байт описывается в коде явно:

```go
package main

import "fmt"

func main() {
	s := "hey"
	bs := []byte(s)
	
	fmt.Println([]byte(s)) // [104 101 121]
	
	fmt.Println(string(bs)) // hey
}
```

Отдельные ASCII символы можно объявлять сразу с типом `byte`. Для этого нужно обернуть символ в одинарные кавычки и указать тип `byte`:

```go
package main

import (
	"fmt"
	"reflect"
)

func main() {
	asciiCh := byte('Z')
	asciiChStr := string(asciiCh)
	
	fmt.Println(reflect.TypeOf(asciiCh), asciiCh) // uint8 90
	fmt.Println(reflect.TypeOf(asciiChStr), asciiChStr) // string Z
}
```

## Обход строки

Поскольку строка - массив байт, её можно обойти с помощью цикла `for`. Код ниже побуквенно выведет строку в терминале.

```go
package main

import (
	"fmt"
)

func main() {
	s := "hello"
	for i := 0; i < len(s); i++ {
		fmt.Println(string(s[i]))
	}
}
```

Но таким способом можно обойти лишь строки, состоящие из ASCII символов. Если строка содержит мультибайтовые символы, вывод будет некорректен. Для работы с другими кодировки используются руны.

