
# Go test

**Ключи:**
- Обратные:
	- [Сведения о языке Go](GO);
- Прямые:
	- 

**Хештеги:** #Programming/Go/HTTP

## Общие сведения

Для написания теста какого то пакета, нам необходимо в директории с этим пакетом создать файл с аналогичным именем и суфиксом `_test`. Например `geo.go` - `geo_test.go`. Таким образом Go поймет, что это тесты для пакета `geo`. Тесты всегда храняться рядом с тестируемым функционалом.

Имя пакета так же имеет значение. Если в файле `geo_test.go` указать имя пакета `geo`, то это будет `white box` тестирование, т.к. тогда мы будем иметь доступ ко всем методам и функциям пакета, даже приватным. Иначе, если укажем имя пакета `geo_test`, то получим `black box` тестирование и будем иметь доступ только к публичным методам и функциям.

Всё функции для тестирования начинаются со слова `Test` и название тестируемой функции. Так же можно добавить доп информацию в конец названия метода, например какой это тип теста. Аргументом функции является контекст тестирования `t *testing.T`, по крайней мере этого достаточно для юнит тестирования (есть и другие контексты).

```go
package geo_test  
  
import "testing"  
  
func TestGetMyLocation(t *testing.T) {  
      
}
```

Есть уловный подход для структурирования тестов AAA - Arrange Act Assert
- Arrange - подготовка данных для тестирования, определение ожидаемого результата;
- Act - выполнение функции;
- Assert - проверка результата теста.

```go
func TestGetMyLocation(t *testing.T) {  
    // Arrange  
    city := "London"  
    expectedCity := geo.GeoData{City: city}  
    // Act  
    got, err := geo.GetMyLocation(city)  
    // Assert  
    if err != nil {  
       t.Error("Error when tried to get city: ", err)  
    }  
    if got.City != expectedCity.City {  
       t.Errorf("Expected city %v, got city %v", expectedCity, got)  
    }  
}
```

Для запуска теста используется консольная команда с путем до запускаемого файла. При отсутствии пути до файла запустятся все тесты.

```shell
go test geo/geo_test.go
```

## Группы тестов

Чтобы не писать каждый раз функцию под каждый набор данных, можно объединить их в группу и выполнить все в рамках одного теста. Для этого нужно выделить тесты в отдельный слайс структур с тестовыми данными. И воспользоваться возможностью пакета testing:

```go
var testCases = []struct {  
    name   string  
    format int  
}{  
    {name: "Big format", format: 147},  
    {name: "0 format", format: 0},  
    {name: "Minus format", format: -5},  
}

func TestGetWeatherWrongFormat(t *testing.T) {  
    for _, tc := range testCases {
	   // Перебираем в цикле тест кейсы и для каждого выполняем анонимную функцию
       t.Run(tc.name, func(t *testing.T) {  
          city := "Moscow"  
          geoData := geo.GeoData{  
             City: city,  
          }  
          expected := weather.ErrWrongFormat  
  
          _, err := weather.GetWeather(geoData, tc.format)  
  
          if !errors.Is(err, expected) {  
             t.Errorf("Expected error %v, got city %v", weather.ErrWrongFormat, err)  
          }  
       })  
    }  
}
```
