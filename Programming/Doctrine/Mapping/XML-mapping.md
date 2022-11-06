
Драйвер сопоставления XML позволяет предоставлять метаданные ORM в форме документов XML.

Для работы с XML маппингом необходимо выполнить следующие условия:
- Каждая сущность должна иметь собственный определенный файл с маппином в формате XML;
- Имя такого документа должно соответствовтаь имени сущности, при этом символы "\" в пространстве имен должны быть замемены на ".";
- Каждый файл маппинга должен иметь расширение "dcm.xml", чтобы быть распознанным его как файл маппинга (это больше условность и сделано для удобства);

Так будет выглядеть маппинг сущности User из модуля Users:

```xml
<doctrine-mapping xmlns="http://doctrine-project.org/schemas/orm/doctrine-mapping"  
                  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
                  xsi:schemaLocation="https://doctrine-project.org/schemas/orm/doctrine-mapping  
                          https://www.doctrine-project.org/schemas/orm/doctrine-mapping.xsd">  
  
    <entity name="App\Users\Domain\Entity\User" table="users_user">  
        <id name="ulid" type="string" length="26">  
            <generator strategy="NONE" />  
        </id>  
        <field name="email" type="string" />  
        <field name="password" type="string" />  
  
    </entity>  
  
</doctrine-mapping>
```

Для валидации схемы можно воспользоваться встроенным в Doctrine механизмом:

```shell
php bin/console doctrine:schema:validate
```


**Хештеги:**
- #Programming/ORM/Doctrine/Mapping