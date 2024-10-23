
# Руны

**Ключи:**
- Обратные:
	- [Сведения о языке Go](GO);
	- [Строки](Go-string);

**Хештеги:** #Programming/Go/DataTypes/String

## Общие сведения


Для работы с Юникод символами в Go представлен тип `rune`:

```go
package main

import (
	"fmt"
)

func main() {
	emoji := []rune("привет😀")
	
	for i := 0; i < len(emoji); i++ {
		fmt.Println(emoji[i], string(emoji[i])) // выводим код символа и его строковое представление
	}
}
```

`rune` — это алиас к `int32`. Как и байты, руны были созданы для отличия от встроенного типа данных. Каждая руна представляет собой код символа стандарта Юникод. Строка свободно преобразуется в `[]byte` и `[]rune`, но эти 2 типа данных не конвертируются между собой напрямую:

```go
s := "hey😉"
rs := []rune([]byte(s)) // cannot convert ([]byte)(s) (type []byte) to type []rune
bs := []byte([]rune(s)]) // cannot convert ([]rune)(s) (type []rune) to type []byte
```

## Обход строки

В Go присутствует синтаксический сахар при обходе строки. Если использовать конструкцию `for range`, строка автоматически будет преобразована в `[]rune`, то есть обход будет по Юникод символам:

```go
package main
import (
	"fmt"
)

func main() {
	emoji := []rune("cool😀")
	
	for _, ch := range emoji {
		fmt.Println(ch, string(ch)) // выводим код символа и его строковое представление
	} 
}
```
