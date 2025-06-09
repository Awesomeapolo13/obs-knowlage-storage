
# Ошибки в Go

**Ключи:**
- Обатные:
	- [Сведения о языке Go](GO);

**Хештеги:** #Programming/Go

## Общие сведения

В Go нет исключений. Вместо них используется встроенный интерфейс `error`. Ошибки возвращаются явно последним аргументом из функции. Поэтому Go-код выглядит как череда вызовов функций и проверок на ошибки.

Ошибки в Go — это особенность языка, которая позволяет работать с неожиданным поведением кода в явном виде:

```go
import "errors"

func validateName(name string) error {
	if name == "" {
		// errors.New создает новый объект ошибки
		return errors.New("empty name")
	}
	
	if len([]rune(name)) > 50 {
		return errors.New("a name cannot be more than 50 characters")
	}
	
	return nil
}
```

Тип `error` является интерфейсом. Интерфейс — это отдельный тип данных в Go, представляющий набор методов. Любая структура реализует интерфейс неявно через структурную типизацию. Структурная типизация (в динамических языках это называют _утиной типизацией_) — это связывание типа с реализацией во время компиляции без явного указания связи в коде:

```go
package main

import (
	"fmt"
)

// объявление интерфейса
type Printer interface {
	Print()
}

// нигде не указано, что User реализует интерфейс Printer
type User struct {
	email string
} 

// структура User имеет метод Print, как в интерфейсе Printer. Следовательно, во время компиляции запишется связь между User и Printer
func (u *User) Print() {
	fmt.Println("My email is", u.email)
}

// функция принимает как аргумент интерфейс Printer
func TestPrint(p Printer) {
	p.Print()
}

func main() {
	// в функцию TestPrint передается структура User, и так как она реализует интерфейс Printer, все работает без ошибок
	TestPrint(&User{email: "test@test.com"})
}
```

Интерфейс `error` содержит только один метод _Error_, который возвращает строковое представление ошибки:

```go
// The error built-in interface type is the conventional interface for
// representing an error condition, with the nil value representing no error. 
type error interface {
	Error() string
}
```

Следовательно, легко можно создавать свои реализации ошибок:

```go
type TimeoutErr struct {
	msg string
}

// структура TimeoutErr реализует интерфейс error и может быть использована как обычная ошибка
func (e *TimeoutErr) Error() string {
	return e.msg
}
```

Следует запомнить, что если функция возвращает ошибку, то она всегда возвращается последним аргументом:

```go
// функция возвращает несколько аргументов, и ошибка возвращается последней 
func DoHTTPCall(r Request) (Response, error) {
	...
}
```

Нулевое значение для интерфейса — это пустое значение `nil`. Следовательно, когда код работает верно, возвращается `nil` вместо ошибки.

##  Обработка ошибок

Возвращаемые ошибки принято проверять при каждом вызове:

```go
import "log"

response, err := DoHTTPCall()

if err != nil {
	log.Println(err)
}
// только после проверки на ошибку можно делать что-то с объектом response
```

При этом логика обработки отличается от места и типа ошибки. Ошибки можно оборачивать и прокидывать в функцию выше, логировать или делать любые фоллбек действия.

Оборачивание ошибок — важная часть написания кода на Go. Это позволяет явно видеть трейс вызова и место возникновения ошибки. Для оборачивания используется функция `fmt.Errorf`:

```go
package main

import (
	"errors"
	"fmt"
)

// для простоты примера опускаем аргументы запроса и ответа
func DoHTTPCall() error {
	err := SendTCP()
	if err != nil {
		// оборачивается в виде "[название метода]: %w". %w — это плейсхолдер для ошибки
		return fmt.Errorf("send tcp: %w", err)
	}
	
	return nil
}

var errTCPConnectionIssue = errors.New("TCP connect issue")

func SendTCP() error {
	return errTCPConnectionIssue
}

func main() {
	fmt.Println(DoHTTPCall()) // send tcp: TCP connect issue
}
```

В современном Go существуют функции для проверки типов конкретных ошибок. Например, ошибку из примера выше можно проверить с помощью функции `errors.Is`. В данном случае _errTCPConnectionIssue_ обернута другой ошибкой, но функция `errors.Is` найдет ее при проверке:

```go
err := DoHTTPCall()

if err != nil {
	if errors.Is(err, errTCPConnectionIssue) {
		// в случае ошибки соединения ждем 1 секунду и пытаемся сделать запрос снова
		time.Sleep(1 * time.Second)

		return DoHTTPCall()
	}
	
	// обработка неизвестной ошибки
	log.Println("unknown error on HTTP call", err)
}
```

`errors.Is` подходит для проверки статичных ошибок, хранящихся в переменных. Иногда нужно проверить не конкретную ошибку, а целый тип. Для этого используется функция `errors.As`:

```go
package main

import (
	"errors"
	"log"
	"time"
)

// ошибка подключения к базе данных
type ConnectionErr struct{}

func (e ConnectionErr) Error() string {
	return "connection err"
}

func main() {
	// цикл подключения к БД. Пытаемся 3 раза, если не удалось подсоединиться с первого раза.
	tries := 0
	for {
		if tries > 2 {
			log.Println("Can't connect to DB")
			break
		}

		err := connectDB()
		if err != nil {
			// если ошибка подключения, то ждем 1 секунду и пытаемся снова
			if errors.As(err, &ConnectionErr{}) {
				log.Println("Connection error. Trying to reconnect...")
				time.Sleep(1 * time.Second)
				tries++
				continue
			}

			// в противном случае ошибка критичная, логируем и выходим из цикла
			log.Println("connect DB critical error", err)
		}
	
		break
	}
}

// для простоты функция всегда возвращает ошибку подключения
func connectDB() error {
	return ConnectionErr{}
}
```

Вывод программы спустя 3 секунды:

```shell
Connection error. Trying to reconnect...
Connection error. Trying to reconnect...
Connection error. Trying to reconnect...
Can't connect to DB
```