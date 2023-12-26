
# Структура и работа индексов

Индексы использую бинарную древовидную структуру. Такая структура называется B-Tree или B+Tree, в зависимости от того, хранит ли индекс указатели только на конечные таблицы или нет. Значения столбца или столбцов, используемых в качестве ключа индекса, хранятся, сортируются (упорядочеваются), и организуются в корневом блоке, блоках ветвей (если требуется) и в конечных блоках.

![[Pasted image 20230721093014.png]]

В приведенном выше примере, в котором используется структура B+Tree, у нас есть индекс, созданный для столбца «Бренд» (значения выделены синим цветом), с корневым блоком (блок № 70) с диапазонами значений и указателем (красным) на каждый конечный блок (№ 71, № 72 и № 73), где расположены значения для этого диапазона. Конечные блоки включают в себя каждое индексированное значение (снова синее) и указатель (также красный) на блок или блоки таблицы, в которых данные присутствуют в таблице.

Если нам нужно найти пользователя с определенной фамилией и мы не имеем индекса по соответствующей колонке, то придется считать все таблицу (это 14 блоков). Но после создания индекса это количество упадет до 3-х:

1) Сначала считывается корневой блок, чтобы найти конечный, показывающий где в индексе находится нужное значение.
2) Далее считывается блок конечного индекса, чтобы найти конкретную фамилию и блок (блоки), где находятся данные.
3) Читает конкретный блок с данными.

Чем больше таблица, тем больше может потребоваться промежуточных блоков ветвей для хранения большего количества значений.

**Ключи:**
- [Базы данных](databases);
- [Индексы](db-index);

**Хештеги:** #Programming/Databases/Index