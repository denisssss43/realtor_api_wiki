> Оглавление:
> 1. [TelephoneNumbers (Номера телефонов)]()
> 1. [Hashtags (Хэштеги)]()
> 1. [Urls (URL адреса)]()
> 1. [ResourceTypes (Типы источника)]()
> 1. [Countries (Страны)]()
> 1. [Cities (Города)]()
> 1. [Addresses (Адреса)]()
> 1. [Resources (Источники)]()
> 1. [Offers (Предложения)]()

## TelephoneNumber [#]()
Таблица номеров телефона. Записи добавляются при парсе предложения с привязкой (связь многи ко многим). Через количество предложений для одного номера будут определяться предложения от риелторских агенств.
> - `value` <br> 
>   Хранит значение номера телефона 

Генерация модели:
```
rails generate model TelephoneNumber value:string
```
Create миграция:
```
create_table :telephone_numbers do |t|
    t.string :value, null: false, unique: true

    t.timestamps
end
```

## Hashtag [#]()
Таблица хэштегов. Записи добавляются при парсе предложения с привязкой (связь многи ко многим).
> - `value` <br> 
>   Хранит значение хэштега 

Генерация модели:
```
rails generate model Hashtag value:string
```
Create миграция:
```
create_table :hashtags do |t|
    t.string :value, null: false, unique: true

    t.timestamps
end
```

## Url [#]()
Таблица url-адресов.   
> - `value` <br>
>   Хранит значение url-адреса 

Генерация модели:
```
rails generate model Url value:string
```
Create миграция:
```
create_table :urls do |t|
    t.string :value, null: false, unique: true

    t.timestamps
end
```

## ResourceType [#]()
Таблица типов источника [“Группа ВК”, ...]
> - `title` <br> 
>   Хранит наименование типа источника 

Генерация модели:
```
rails generate model ResourceType title:string
```
Create миграция:
```
create_table :resource_types do |t|
    t.string :title, null: false, unique: true

    t.timestamps
end
```

## Country [#]()
Таблица стран [“Россия”, ...]
> - `title` <br> 
>   Хранит наименование наименование страны 

Генерация модели:
```
rails generate model Country title:string
```
Create миграция:
```
create_table :cities do |t|
    t.references :country, null: false, foreign_key: true
    t.string :title, null: false

    t.timestamps
end
```

## City [#]()
Таблица городов [“Красноярск”, ...]
> - `country_id` <br> 
>   Ссылка на страну, в которой находится город
> - `title` <br> 
>   Хранит наименование города 

Генерация модели:
```
rails generate model City \
    country:references:index \
    title:string
```
Create миграция:
```
create_table :cities do |t|
    t.references :country, null: false, foreign_key: true
    t.string :title, null: false

    t.timestamps
end
```

## Address [#]()
Таблица адресов
> - `city_id` <br> 
>   Ссылка на город, которому принадлежит адрес
> - `title` <br> 
>   Хранит полную строку адреса
> - `latitude` <br> 
>   Хранит широту адреса
> - `longitude` <br> 
>   Хранит долготу адреса 

Генерация модели:
```
rails generate model Address \
    city:references:index \
    title:string \
    'latitude:decimal{15,6}' \
    'longitude:decimal{15,6}'
```
Create миграция:
```
create_table :addresses do |t|
    t.references :city, null: false, foreign_key: true
    
    t.string :title, null: false

    t.decimal :latitude, precision: 15, scale: 6, null: false
    t.decimal :longitude, precision: 15, scale: 6, null: false

    t.timestamps
end
```

## Resource [#]()
Таблица источников
> - `resource_type_id` <br> 
>   Ссылка на тип ресурса
> - `url_id` <br> 
>   Ссылка на url-адрес
> - `city_id` <br> 
>   Ссылка на город, для которого будут определяться найденные в этом источнике предложения

Генерация модели:
```
rails generate model Resource \
    resource_type:references:index \
    url:references:index \
    city:references:index \
```
Create миграция:
```
create_table :resources do |t|
    t.references :resource_type, null: false, foreign_key: true
    t.references :url, null: false, foreign_key: true, unique: true
    t.references :city, null: false, foreign_key: true

    t.timestamps
end
```

## Offer [#]()
Таблица предложений 
> - `resource_id` <br> 
>   Ссылка на источник предложения
> - `address_id` <br> 
>   Ссылка на адрес предложения
> - `price` <br> 
>   Хранит цену предложения
> - `square_meters` <br> 
>   Хранит кол-во квадратных метров
> - `room_count` <br> 
>   Хранит кол-во комнат
> - `floor_number` <br> 
>   Хранит этаж
> - `apartment_number` <br> 
>   Хранит номер квартиры
> - `is_furniture` <br> 
>   Указывает на то есть ли мебель в данном предложении
> - `is_realtor` <br> 
>   Указывает на то является ли данное предложение предложением агенства 
> - `rental_period` <br> 
>   Хранит период аренды `enum`: `:month`,`:day`
> - `description` <br> 
>   Хранит описание (*Полный текст из источника. Нужен только для дальнейшего анализа и парса*). Системное поле
> - `sidekiq_status` <br> 
>   Хранит статус обработки `enum`: `:searcher`,`:parser`,`:completed`,`:error`. Системное поле 

Генерация модели:
```
rails generate model Offer \
    resource:references:index \
    address:references:index \
    'price:decimal{15,2}' \
    square_meters:int \
    room_count:int \
    floor_number:int \
    apartment_number:int \
    is_furniture:boolean \
    is_realtor:boolean \
    rental_period:int \
    description:text \
    sidekiq_status:int  
```
Create миграция:
```
create_table :offers do |t|
    t.references :resource, null: false, foreign_key: true
    t.references :address, null: false, foreign_key: true
    
    t.int :square_meters
    t.int :room_count
    t.int :floor_number
    t.int :apartment_number
    t.boolean :is_furniture
    t.boolean :is_realtor, default: false
    t.int :rental_period, default: 1
    
    t.decimal :price, precision: 15, scale: 2, null: false
    
    t.text :description, unique: true, null: false
    t.int :sidekiq_status, null: false, default: 1

    t.timestamps
end
```
