
# Методы структур

**Ключи:**
- Обратные:
	- [Сведения о языке Go](GO);
	- [Структуры Go](Go-structure-methods);

**Хештеги:** #Programming/Go/OOP

Метод — это функция с дополнительным аргументом, который указывается в скобках между `func` и названием функции:

```go
package main

import (
	"fmt"
)

type Dog struct{}

// сначала объявляется дополнительный аргумент "(d Dog)", а следом идет обычное описание функции
func (d Dog) Bark() {
	fmt.Println("woof!")
}

func main() {
	d := Dog{}
	d.Bark() // woof!
}
```

В примере выше структура _Dog_ передается по значению, то есть копируется. Если изменятся любые свойства внутри метода _Bark_, они останутся неизменными в исходной структуре:

```go
package main

import (
	"fmt"
)

type Dog struct {
	IsBarked bool
}

func (d Dog) Bark() {
	fmt.Println("woof!")
	d.IsBarked = true
}

func main() {
	d := Dog{}
	d.Bark() // woof!
	
	fmt.Println(d.IsBarked) // false
}
```

Если есть необходимость в изменении состояния, структура должна передаваться указателем:

```go
package main

import (
	"fmt"
)

type Dog struct {
	IsBarked bool
}

func (d *Dog) Bark() {
	fmt.Println("woof!")
	d.IsBarked = true
}

func main() {
	d := &Dog{}
	d.Bark() // woof!
	fmt.Println(d.IsBarked) // true 
}
```
