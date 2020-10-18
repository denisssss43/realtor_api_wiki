Оглавление:
1. [About](https://github.com/denisssss43/realtor_api_wiki#about)
1. [Model](https://github.com/denisssss43/realtor_api_wiki/blob/master/model.md#model)
    - [TelephoneNumber](https://github.com/denisssss43/realtor_api_wiki/blob/master/model.md#TelephoneNumber-)
    - [Hashtag](https://github.com/denisssss43/realtor_api_wiki/blob/master/model.md#Hashtag-)
    - [Url](https://github.com/denisssss43/realtor_api_wiki/blob/master/model.md#Url-)
    - [ResourceType](https://github.com/denisssss43/realtor_api_wiki/blob/master/model.md#ResourceType-)
    - [Country](https://github.com/denisssss43/realtor_api_wiki/blob/master/model.md#Country-)
    - [City](https://github.com/denisssss43/realtor_api_wiki/blob/master/model.md#City-)
    - [Addresse](https://github.com/denisssss43/realtor_api_wiki/blob/master/model.md#Addresse-)
    - [Resource](https://github.com/denisssss43/realtor_api_wiki/blob/master/model.md#Resource-)
    - [Offer](https://github.com/denisssss43/realtor_api_wiki/blob/master/model.md#Offer-)
    - [OfferTelephoneNumber](https://github.com/denisssss43/realtor_api_wiki/blob/master/model.md#Offer-)
    - [OfferHashtag](https://github.com/denisssss43/realtor_api_wiki/blob/master/model.md#Offer-)
    - [OfferUrl](https://github.com/denisssss43/realtor_api_wiki/blob/master/model.md#Offer-)
1. [Sidekiq](https://github.com/denisssss43/realtor_api_wiki/blob/master/sidekiq.md#sidekiq)


# MODEL
Описание модели данных

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
t.string :value, null: false, unique: true
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
t.string :value, null: false, unique: true
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
t.string :value, null: false, unique: true
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
t.string :title, null: false, unique: true
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
t.references :country, null: false, foreign_key: true
t.string :title, null: false
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
t.references :country, null: false, foreign_key: true
t.string :title, null: false
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
t.references :city, null: false, foreign_key: true
t.string :title, null: false
t.decimal :latitude, precision: 15, scale: 6, null: false
t.decimal :longitude, precision: 15, scale: 6, null: false
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
t.references :resource_type, null: false, foreign_key: true
t.references :url, null: false, foreign_key: true, unique: true
t.references :city, null: false, foreign_key: true
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
```
Модель:
```
class Offer < ApplicationRecord
    belongs_to :resource
    belongs_to :address

    has_many :offer_telephone_numbers
    has_many :telephone_numbers, through: :offer_telephone_numbers

    has_many :offer_hashtags
    has_many :hashtags, through: :offer_hashtags
    
    has_many :offer_urls
    has_many :urls, through: :offer_urls
end
```

## OfferTelephoneNumber [#]()
Таблица связей риелторских предложений и телефонных номеров
> - `offer_id` <br> 
>   Ссылка на риелторское предложение, к которому будет привязан номер
> - `telephone_number_id` <br> 
>   Ссылка на номер телефона, который будет привязан к риелторскому предложению

Генерация модели:
```
rails generate model OfferTelephoneNumber \
    offer:references:index \
    telephone_number:references:index \
```
Create миграция:
```
t.references :offer, null: false, foreign_key: true
t.references :telephone_number, null: false, foreign_key: true
```
Модель:
```
class OfferTelephoneNumber < ApplicationRecord
    belongs_to :offer
    belongs_to :telephone_number
end
```

## OfferHashtag [#]()
Таблица связей риелторских предложений и телефонных номеров
> - `offer_id` <br> 
>   Ссылка на риелторское предложение, к которому будет привязан номер
> - `hashtag_id` <br> 
>   Ссылка на номер телефона, который будет привязан к риелторскому предложению

Генерация модели:
```
rails generate model OfferHashtag \
    offer:references:index \
    hashtag:references:index \
```
Create миграция:
```
t.references :offer, null: false, foreign_key: true
t.references :hashtag, null: false, foreign_key: true
```
Модель:
```
class OfferHashtag < ApplicationRecord
    belongs_to :offer
    belongs_to :hashtag
end
```

## OfferUrl [#]()
Таблица связей риелторских предложений и телефонных номеров
> - `offer_id` <br> 
>   Ссылка на риелторское предложение, к которому будет привязан номер
> - `url_id` <br> 
>   Ссылка на номер телефона, который будет привязан к риелторскому предложению

Генерация модели:
```
rails generate model OfferUrl \
    offer:references:index \
    url:references:index \
```
Create миграция:
```
t.references :offer, null: false, foreign_key: true
t.references :url, null: false, foreign_key: true
```
Модель:
```
class OfferUrl < ApplicationRecord
    belongs_to :offer
    belongs_to :url
end
```

## OfferImgUrl [#]()
Таблица связей риелторских предложений и url-адрессов изображений
> - `offer_id` <br> 
>   Ссылка на риелторское предложение, к которому будет привязан номер
> - `url_id` <br> 
>   Ссылка на номер телефона, который будет привязан к риелторскому предложению

Генерация модели:
```
rails generate model OfferUrl \
    offer:references:index \
    url:references:index \
```
Create миграция:
```
t.references :offer, null: false, foreign_key: true
t.references :url, null: false, foreign_key: true
```
Модель:
```
class OfferUrl < ApplicationRecord
    belongs_to :offer
    belongs_to :url
end
```