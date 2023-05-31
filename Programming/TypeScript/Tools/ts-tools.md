
# Tools

## ESLint and Prettier

**ESLint** - инструмент для проверки соответсвия кодового стиля принятым в команде правилам. Применяется как для TS так и для JS

**Prettier** - форматтер кода. Он автоматически форматирует код в соответствии с указанными правилами. В теории может конфликтовать с ESLint, но существуют версии для работы с ним в связке.

## Установка

```shell
npm i -D eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin prettier eslint-config-prettier eslint-plugin-prettier typescript
```

- eslint - сам ESLint;
- @typescript-eslint/parser - парсер ESLint для TS;
- @typescript-eslint/eslint-plugin - плагин ESLint;
- prettier - сам Prettier;
- eslint-config-prettier - дополнительный конфиг (дефолтный);
- eslint-plugin-prettier - специальный плагин для Prettier;
- typescript - TSв проект как dev зависимость.

Добавляем в корень проекта конфиги `.prettierrc`.

```json
{  
  "singleQuote": true,  // Использование только одинарных кавычек
  "trailingComma": "all",  // Разрещаем висящую ',' в конце элементов массива
  "useTabs": true,  // используем табы а не пробелы
  "semi": true,  // используем ; в конце строк
  "bracketSpacing": true,  // пробелы между скобками
  "printWidth": 100,  // максимальная длинна в строке
  "endOfLine": "auto"  // дополнительный конец линии в конце файла
}
```

Добавляем в корень проекта конфиги `.eslintrc`.

```json
{  
  "root": true,  // Корень проекта
  "parser": "@typescript-eslint/parser",  // парсер для TS
  "plugins": [  
    "@typescript-eslint"  // используемые плагины
  ],  
  "extends": [  // наследуемые пакеты
    "eslint:recommended",  
    "plugin:@typescript-eslint/eslint-recommended",  
    "plugin:@typescript-eslint/recommended",  
    "plugin:prettier/recommended"  
  ],  
  "rules": {  // правила
    "@typescript-eslint/ban-types": "off",  // разрешена запись {}/Object для объектов
    "@typescript-eslint/no-unused-vars": [  // можно использовать не используемые кодом переменные
      "off"  
    ],  
    "@typescript-eslint/no-explicit-any": "off",  // разрешаем тип any
    "@typescript-eslint/explicit-function-return-type": [  
      "warn"  // явно указываем возвращаемые у функций типы
    ],  
    "prettier/prettier": [  // конфиг приттиера, провторяет конфиг .prettierrc
      "error",  
      {  
        "singleQuote": true,  
        "useTabs": true,  
        "semi": true,  
        "trailingComma": "all",  
        "bracketSpacing": true,  
        "printWidth": 100,  
        "endOfLine": "auto"  
      }  
    ]  
  }  
}
```

Настройка в PhpStorm:

- File -> Settings -> Languages & Frameworks ->JavaScript -> Code Quality Tools -> ESLint;
- Выбрать Automatic ESLint configuration, Run eslint --fix on save;
- File -> Settings -> Languages & Frameworks ->JavaScript -> Prettier;
-  Выбрать On 'Reformat Code' action и On save;
- File -> Settings -> Languages & Frameworks -> Node.js;
- Указать путь к Node.js.

**Ключи:**
- [JavaScript](javascript);
- [TypeScript](typescript);
- [Nodemonitor](nodemon);

**Хештеги:** 
- #Programming/TypeScript/Tools
- #Programming/TS/Tools
