
# Контекст

Context - строеный пакет стандартной библиотеки Go, который предоставляет набор инструментов для управления параллельными операциями и передачей данных.

Возможности:
- обогощать его key / value и читать их из другого места через контекст;
- сигналы отмены - отменять какие то операции;
- Deadlines - ограничивать операции по времени.

Контекст встроен в http библиотеку, а именно в http.Request.


## Создание контекста

Создать пустой контекст без всяких данных и функций можно через:

```go
ctx := context.Background()
```

Он может быть преобразован в контекст с таймаутом или любой другой, а так же передаваться между функциями.

**С таймаутом**, мы передаем в базовый контекст текоторый таймаут для операции. Теперь, если функция завершится до истечения таймаута - мы обработаем её результаты. Но если таймаут выйдет, то можно, например, создать ошибку. Это удобно, когда у нас есть ограничения по времени ожидания результатов, например, когда отправляем запрос в стороннее API или в БД.

```go
func main() {  
    ctx := context.Background()  
    ctxWithTimeout, cancel := context.WithTimeout(ctx, 4*time.Second)  
    defer cancel()  
  
    done := make(chan struct{})  
  
    go func() {  
       time.Sleep(3 * time.Second)  
       close(done)  
    }()  
  
    select {  
    case <-done:  
       fmt.Println("done")  
    case <-ctxWithTimeout.Done():  
       fmt.Println("timeout")  
    }  
}
```

**Со значением**, мы можем передавать  значения и обрабатывать их далее в коде приложения.

```go
func main() {  
    type key int  
    const EmailKey key = 0  
    ctx := context.Background()  
    ctxWithValue := context.WithValue(ctx, EmailKey, "a@a.ru")  
  
    if userEmail, ok := ctxWithValue.Value(EmailKey).(string); ok {  
       fmt.Println(userEmail)  
    } else {  
       fmt.Println("No value for email")  
    }  
}
```

**С отменой**. Допустим у нас есть функция, которая бесконечно что-то обрабатывает, но мы хотим завершить её по какому то условию. Для этого можно передать в неё контекст и прочитать его и отменить операцию, которая происходит в какой нибудь горутине.

```go
func tickOperation(ctx context.Context) {  
    ticker := time.NewTicker(200 * time.Millisecond)  
    for {  
       select {  
       case <-ticker.C:  
          fmt.Println("Tick")  
       case <-ctx.Done():  
          fmt.Println("Cancel")  
          return  
       }  
    }  
}  
  
func main() {  
    ctx, cancel := context.WithCancel(context.Background())  
    go tickOperation(ctx)  
  
    time.Sleep(2 * time.Second)  
    cancel()  
    time.Sleep(2 * time.Second)  
}
```

