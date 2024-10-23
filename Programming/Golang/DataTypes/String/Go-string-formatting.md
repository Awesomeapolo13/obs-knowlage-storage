
# Форматирование строк

**Ключи:**
- Обратные:
	- [Сведения о языке Go](GO);
	- [Строки](Go-string);

**Хештеги:** #Programming/Go/DataTypes/String

## Общие сведения

Для форматирования строк используется пакет `fmt`. Плейсхолдеры разных типов даннях в основном не отличаются от других ЯП

```go
name := "Andy"
// подставляем строку
fmt.Sprintf("hello %s", name) // "hello Andy"

// число
fmt.Sprintf("there are %d kittens", 10) // "there are 10 kittens"

// логический тип
fmt.Sprintf("your story is %t", true) // "your story is true"
```

Так же есть плейсхолдеры, которые преобразуют сложные данные:

```go
package main

import ( "fmt" )

type Person struct {
	Name string
	Age int
}

func main() {
	p := Person{Name: "Andy", Age: 18}
	
	// вывод значений структуры
	fmt.Println("simple struct:", p)
	
	// вывод названий полей и их значений
	fmt.Printf("detailed struct: %+v\n", p)
	
	// вывод названий полей и их значений в виде инициализации
	fmt.Printf("Golang struct: %#v\n", p)
}
```

Вывод:

```shell
simple struct: {Andy 18}
detailed struct: {Name:Andy Age:18}
Golang struct: main.Person{Name:"Andy", Age:18}
```
