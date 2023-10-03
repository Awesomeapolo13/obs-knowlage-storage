
# File template

В IDE PhpStrom есть возможность переопределить шаблоны создаваемых файлов для классов, скриптов, геттеров и т.д.

1) Шаблоны новых файлов - File -> Settings -> Editor -> File and Code Templates, вкладка File.
2) Геттеры и сеттеры в классах (File -> Settings -> Editor -> File and Code Templates, вкладка Code):
	- есть возмоность выбрать fluent setter (сеттер, возвращающий `$this`) при генерации через `Alt` + `Ins`;
	- во вкладке Code можно настроить формат геттеров и сеттеров 