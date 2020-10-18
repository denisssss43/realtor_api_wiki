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
    - [OfferImgUrl](https://github.com/denisssss43/realtor_api_wiki/blob/master/model.md#OfferImgUrl-)
1. [Sidekiq](https://github.com/denisssss43/realtor_api_wiki/blob/master/sidekiq.md#sidekiq)


# MODEL
Описание модели данных

## TelephoneNumber [#](https://github.com/denisssss43/realtor_api_wiki/blob/master/model.md#)
Таблица номеров телефона. Записи добавляются при парсе предложения с привязкой (связь многи ко многим). Через количество предложений для одного номера будут определяться предложения от риелторских агенств.
> - `value` <br> 
>   Хранит значение номера телефона 
> - `offers` <br> 
>   Список риелторских предложений 

Генерация модели:
```
rails generate model TelephoneNumber value:string
```
Create миграция:
```
t.string :value, null: false, unique: true
```
Модель:
```
class TelephoneNumber < ApplicationRecord
    has_many :offer_telephone_numbers
    has_many :offers, through: :offer_telephone_numbers
end
```

## Hashtag [#](https://github.com/denisssss43/realtor_api_wiki/blob/master/model.md#)
Таблица хэштегов. Записи добавляются при парсе предложения с привязкой (связь многи ко многим).
> - `value` <br> 
>   Хранит значение хэштега 
> - `offers` <br> 
>   Список риелторских предложений 

Генерация модели:
```
rails generate model Hashtag value:string
```
Create миграция:
```
t.string :value, null: false, unique: true
```
Модель:
```
class Hashtag < ApplicationRecord
    has_many :offer_hashtags
    has_many :offers, through: :offer_hashtags
end
```

## Url [#](https://github.com/denisssss43/realtor_api_wiki/blob/master/model.md#)
Таблица url-адресов.   
> - `value` <br>
>   Хранит значение url-адреса 
> - `offers` <br> 
>   Список риелторских предложений 

Генерация модели:
```
rails generate model Url value:string
```
Create миграция:
```
t.string :value, null: false, unique: true
```
Модель:
```
class Url < ApplicationRecord
    has_many :offer_urls
    has_many :offers, through: :offer_urls
end
```

## ImgUrl [#](https://github.com/denisssss43/realtor_api_wiki/blob/master/model.md#)
Таблица url-адресов изображений.   
> - `value` <br>
>   Хранит значение url-адреса изображения
> - `offers` <br> 
>   Список риелторских предложений 

Генерация модели:
```
rails generate model ImgUrl value:string
```
Create миграция:
```
t.string :value, null: false, unique: true
```
Модель:
```
class ImgUrl < ApplicationRecord
    has_many :offer_img_urls
    has_many :offers, through: :offer_img_urls
end
```

## ResourceType [#](https://github.com/denisssss43/realtor_api_wiki/blob/master/model.md#)
Таблица типов источника [“Группа ВК”, ...]
> - `resources` <br> 
>   Список источников, которые используют тип источника 
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
Модель:
```
class ResourceType < ApplicationRecord
    has_many :resources
end
```

## Country [#](https://github.com/denisssss43/realtor_api_wiki/blob/master/model.md#)
Таблица стран [“Россия”, ...]
> - `cities` <br> 
>   Список городов, входящих в состав страны 
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
Модель:
```
class Country < ApplicationRecord 
    has_many :cities
end
```

## City [#](https://github.com/denisssss43/realtor_api_wiki/blob/master/model.md#)
Таблица городов [“Красноярск”, ...]
> - `country` <br> 
>   Ссылка на страну, в которой находится город
> - `addresses` <br> 
>   Список адресов, которые закреплены за городом
> - `resources` <br> 
>   Список источников, которые закреплены за городом
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
Модель:
```
class City < ApplicationRecord
    belongs_to :country
    has_many :addresses
    has_many :resources 
end
```

## Address [#](https://github.com/denisssss43/realtor_api_wiki/blob/master/model.md#)
Таблица адресов
> - `city` <br> 
>   Ссылка на город, которому принадлежит адрес
> - `offers` <br> 
>   Список предложений, закрепленных за этим адресом
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
Модель:
```
class Address < ApplicationRecord
    belongs_to :city
    has_many :offers
end
```

## Resource [#](https://github.com/denisssss43/realtor_api_wiki/blob/master/model.md#)
Таблица источников
> - `resource_type` <br> 
>   Ссылка на тип ресурса
> - `url` <br> 
>   Ссылка на url-адрес
> - `city` <br> 
>   Ссылка на город, для которого будут определяться найденные в этом источнике предложения
> - `offers` <br> 
>   Список предложений, закрепленных за этим источником

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
Модель:
```
class Resource < ApplicationRecord
    belongs_to :resource_type
    belongs_to :url
    belongs_to :city
    has_many :offers
end
```

## Offer [#](https://github.com/denisssss43/realtor_api_wiki/blob/master/model.md#)
Таблица предложений 
> - `resource` <br> 
>   Ссылка на источник предложения
> - `address` <br> 
>   Ссылка на адрес предложения
> - `telephone_numbers` <br> 
>   Список телефонных номеров
> - `hashtags` <br> 
>   Список хэштегов
> - `urls` <br> 
>   Список url-адресов
> - `img_urls` <br> 
>   Список изображений
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
>   Хранит период аренды `enum`: `:month`, `:day`
> - `description` <br> 
>   Хранит описание (*Полный текст из источника. Нужен только для дальнейшего анализа и парса*). Системное поле
> - `sidekiq_status` <br> 
>   Хранит статус обработки `enum`: `:searcher`, `:parser`, `:completed`, `:error`. Системное поле 

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
    
    has_many :offer_img_urls
    has_many :img_urls, through: :offer_img_urls
end
```

## OfferTelephoneNumber [#](https://github.com/denisssss43/realtor_api_wiki/blob/master/model.md#)
Таблица связей риелторских предложений и телефонных номеров
> - `offer` <br> 
>   Ссылка на риелторское предложение, к которому будет привязан номер
> - `telephone_number` <br> 
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

## OfferHashtag [#](https://github.com/denisssss43/realtor_api_wiki/blob/master/model.md#)
Таблица связей риелторских предложений и хэштегов
> - `offer` <br> 
>   Ссылка на риелторское предложение, к которому будет привязан хэштег
> - `hashtag` <br> 
>   Ссылка на хэштег, который будет привязан к риелторскому предложению

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

## OfferUrl [#](https://github.com/denisssss43/realtor_api_wiki/blob/master/model.md#)
Таблица связей риелторских предложений и url-адресов
> - `offer` <br> 
>   Ссылка на риелторское предложение, к которому будет привязана ссылка
> - `url` <br> 
>   Ссылка, которая будет привязана к риелторскому предложению

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

## OfferImgUrl [#](https://github.com/denisssss43/realtor_api_wiki/blob/master/model.md#)
Таблица связей риелторских предложений и url-адрессов изображений
> - `offer` <br> 
>   Ссылка на риелторское предложение, к которому будет привязано изображение
> - `img_url` <br> 
>   Ссылка на изображение, которое будет привязано к риелторскому предложению

Генерация модели:
```
rails generate model OfferImgUrl \
    offer:references:index \
    img_url:references:index \
```
Create миграция:
```
t.references :offer, null: false, foreign_key: true
t.references :img_url, null: false, foreign_key: true
```
Модель:
```
class OfferImgUrl < ApplicationRecord
    belongs_to :offer
    belongs_to :img_url
end
```