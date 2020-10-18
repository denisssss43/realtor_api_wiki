Оглавление:
1. [About](https://github.com/denisssss43/realtor_api_wiki#about)
1. [Model](https://github.com/denisssss43/realtor_api_wiki/blob/master/model.md#model)
1. [Sidekiq](https://github.com/denisssss43/realtor_api_wiki/blob/master/sidekiq.md#sidekiq)
    - [OffersSearchWorker](https://github.com/denisssss43/realtor_api_wiki/blob/master/sidekiq.md#OffersSearchWorker-)
    - [OffersParsWorker](https://github.com/denisssss43/realtor_api_wiki/blob/master/sidekiq.md#OffersParsWorker-)
    - [OffersParsAddressWorker](https://github.com/denisssss43/realtor_api_wiki/blob/master/sidekiq.md#OffersParsAddressWorker-)

# SIDEKIQ
Описание работы воркеров

## OffersSearchWorker [#](https://github.com/denisssss43/realtor_api_wiki/blob/master/sidekiq.md#sidekiq)
Поисковик риэлторских предложений.

Обходит все источники и собирает описания предложений. Создает пустую запись в таблице `offers` и заполняет в ней поля:
> - `resource`, 
> - `description` (*значение `description` в таблице `offers` уникально (если запись с текущим значением `description` ранее создана, то новая запись не добавляется)*), 
> - `sidekiq_status` заполняется значением `:searcher`.

Код:
```
```

## OffersParsWorker [#](https://github.com/denisssss43/realtor_api_wiki/blob/master/sidekiq.md#sidekiq)
Парсер риэлторских предложений.

Обходит все записи в таблице `offers` со статусом `:searcher` и парсит значение поля `description`. Заполняет результатом параса поля: 
> - `price`, 
> - `room_count`, 
> - `floor_number`, 
> - `apartment_number`, 
> - `is_furniture`, 
> - `is_realtor`, 
> - `rental_period`, 
> - `sidekiq_status` заполняется значением `:parser`.

Так же создает записи в таблицах: 
> - `telephone_number`, 
> - `urls`, 
> - `hashtags`.

Код:
```
```

## OffersParsAddressWorker [#](https://github.com/denisssss43/realtor_api_wiki/blob/master/sidekiq.md#sidekiq)
Парсер риэлторских предложений для поиска точного адреса.
Обходит все записи в таблице со статусом `:parser` и ищет в поле `description` подстроку с похожим на адрес значением. Определение и нормализация адреса осуществляется с помощью `Яндекс.Карт`.

Создает новую запись (*в случае если запись не была добавлена ранее*) в таблице `addresses` и привязывает ее к текущей записи в таблице `offers`.

В случае успешного определения адреса поле `sidekiq_status` заполняется значением `:completed`, в случае если адрес не был определен поле `sidekiq_status` заполняется значением `:error`.

Код:
```
```
