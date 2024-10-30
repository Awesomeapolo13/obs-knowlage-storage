
# Указатели в GO

**Ключи:**
- Обратные:
	- [Сведения о языке Go](GO);
	- [Функции Go](Go-functions);

**Хештеги:** #Programming/Go/Functions 

## Общие сведения

Go поддерживает *указатели*, позволяя передавать ссылки на значения и записи внутри вашей программы.

Покажем, как работают указатели в отличие от значений с помощью двух функций: `Zeroval` и `Zeroptr`. `Zeroval` имеет параметр `int`, поэтому аргументы будут передаваться ему по значению. `Zeroval` получит копию `ival`, отличную от копии в вызывающей функции.

```go
package main

import "fmt"

func zeroval(ival int) {
    ival = 0
}

func zeroptr(iptr *int) {
    *iptr = 0
}

func main() {
    i := 1
    fmt.Println("initial:", i)

    zeroval(i)
    fmt.Println("zeroval:", i)

    zeroptr(&i) // Синтаксис `&i` дает адрес памяти `i`, то есть указатель на `i`.
    fmt.Println("zeroptr:", i)

    fmt.Println("pointer:", &i) // Указатели также можно распечатать.
}
```

Zeroptr, напротив, имеет параметр `*int`, что означает, что он принимает указатель int. Код `*iptr` в теле функции затем разыменовывает указатель с адреса памяти на текущее значение по этому адресу. Присвоение значения разыменованному указателю изменяет значение по указанному адресу.

```shell
$ go run pointers.go
initial: 1
zeroval: 1
zeroptr: 0
pointer: 0x42131100
```

`Zeroval` не меняет `i` в main, а `Zeroptr` меняет, потому что у него есть ссылка на адрес памяти для этой переменной.

## Аргументы с указателями

Здесь рассмотрены только основы передачи указателей на аргументы в функции:

```go
package main

import (
	"fmt"
)

type User struct {
	email string
	password string
}

// при объявлении указываем,
// что переменная должна быть указателем.
// Для этого ставим звездочку * перед типом данных
func fillUserData(u *User, email string, pass string) {
	u.email = email
	u.password = pass
}

func main() {
	u := User{}
	
	// передаем указатель с помощью амперсанда
	// & перед переменной
	fillUserData(&u, "test@test.com", "qwerty")
	
	fmt.Printf("points on func call %+v\n", u)
	// points on func call {email:test@test.com password:qwerty}
	
	// сразу инициализируем переменную с указателем
	up := &User{}
	
	fillUserData(up, "test@test.com", "qwerty")
	
	fmt.Printf("points on init %+v\n", up)
	// points on init {email:test@test.com password:qwerty}
}
```

Маппы по умолчанию передаются с указателем:

```go
package main

import (
	"fmt"
)

func main() {
	m := map[string]int{}
	fillMap(m)
	fmt.Println(m) // map[random:1]
}

func fillMap(m map[string]int) {
	m["random"] = 1
}
```

Разработчики, пришедшие из других языков, часто используют фразы "передача по ссылке" или "ссылка на переменную". Строго говоря, в Go нет ссылок, только указатели:

```go
package main

import "fmt"

func main() {
	a := 1
	b := &a
	c := &b
	
	fmt.Printf("%p %p %p\n", &a, &b, &c)
	// 0xc000018030 0xc00000e028 0xc00000e030
}
```

В этом примере _b_ и _c_ содержат одинаковые значения — адрес переменной _a_, однако _b_ и _c_ хранятся в разных адресах. Из-за этого обновление переменной _b_ не изменит _c_. Поэтому если кто-то говорит про ссылки в Go, он имеет в виду указатели.
