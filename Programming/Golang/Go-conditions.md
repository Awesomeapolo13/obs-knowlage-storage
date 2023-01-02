
# Условные конструкции

## If/else

Условия в Go представлены привычной конструкцией `if else`. В условии должно быть строго выражение логического типа. Следующий пример вернет ошибку компиляции:

```go
if "hi" {
	// non-bool "hi" (type string) used as if condition
}
```

Корректный пример:

```go
package main

import (
	"fmt"
	"strings"
)

func statusByName(name string) string { 
	// функция проверяет, что строка name начинается с подстроки "Mr."
	if strings.HasPrefix(name, "Mr.") {
		return "married man"
	} else if strings.HasPrefix(name, "Mrs.") {
		return "married woman"
	} else {
		return "single person"
	}
}

func main() {
	n := "Mr. Doe"
	fmt.Println(n + " is a " + statusByName(n)) // Mr. Doe is a married man
	
	n = "Mrs. Berry"
	fmt.Println(n + " is a " + statusByName(n)) // Mrs. Berry is a married woman
	
	n = "Karl"
	fmt.Println(n + " is a " + statusByName(n)) // Karl is a single person
}
```

Логическое выражение пишется после `if` без скобок. `else if` можно написать только раздельно.

## Switch / Case

 Для этой конструкции используется стандартный синтаксис, но логика работы отличается от С-подобных языков. Когда срабатывает условие какого-либо `case`, программа выполняет блок и выходит из конструкции `switch` без необходимости писать `break`:

```go
x := 10

switch x {
	default: // default всегда выполняется последним независимо от расположения в конструкции
		fmt.Println("default case")
	case 10:
		fmt.Println("case 10")
}
```

В примере выше, результатом будет ``10``.

Однако при необходимости можно реализовать логику С-подобных языков и «провалиться» в следующий `case`:

```go
x := 10

switch { // выражение отсутствует. Для компилятора выглядит как: switch true
	default:
		fmt.Println("default case")
	case x == 10:
		fmt.Println("equal 10 case")
		fallthrough
	case x <= 10:
		fmt.Println("less or equal 10 case")
}
```

Результат:

```go
equal 10 case
less or equal 10 case
```

**Ключи:**
- [Сведения о языке Go](GO);

**Хештеги:** #Programming/Go