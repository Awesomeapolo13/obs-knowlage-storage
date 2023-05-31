
# Частые проблемы из-за типов и их решения

1) Element implicitly has an 'any' type because expression of type - возникает если обращаться не к конкретному ключу объекта, а расчитывать его в выражении.

Решение:

Обозначить тип ключа, например:

```typescript
readonly ICONS: {  
    [key: string]: string,  
} = {  
    '01': '☀️',  
    '02': '🌤️',  
    '03': '☁️',  
    '04': '☁️',  
    '09': '🌧️',  
    '10': '🌦️',  
    '11': '🌩️',  
    '13': '❄️',  
    '50': '🌫️',  
};

this.ICONS[icon.slice(0, -1)]
```

**Ключи:**
- [TypeScript](typescript).

**Хештеги:** 
- #Programming/TypeScript/Types
- #Programming/TS/Types