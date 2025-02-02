# Синтез речи по шаблонам

С помощью синтеза по шаблонам вы можете синтезировать речь по заранее заготовленным фразам (_шаблонам_), в которых изменяются отдельные ключевые части — _переменные_. Новая фраза будет синтезирована целиком, а не склеена из заранее записанного и синтезированного блоков. При этом интонации копируются из шаблона, а речь звучит естественно и неотличима от речи живого человека.

Синтез по шаблонам доступен только для голосов [{{ brand-voice-name }}](brand-voice/index.md).


{% note warning %}

Сервис {{ brand-voice-name }} предоставляется по запросу. Для доступа к технологии [заполните форму](#contact-form).

{% endnote %}


## Исходные данные для синтеза по шаблонам {#income-data}

Каждый запрос для синтеза по шаблонам должен содержать:

* аудиозапись шаблонной фразы;
* текстовый шаблон с разметкой переменных частей;
* значения переменных для синтеза новой фразы;
* временную метку начала и длительность каждой переменной части.

> Например, желаемая фраза для синтеза `Давайте проверим бронирование. Ваш рейс восьмого сентября в одиннадцать двадцать по маршруту Москва Санкт-Петербург. Аэропорт вылета Домодедово, аэропорт прилета Пулково. Все верно?`
> Аудиозапись шаблона содержит фразу `Давайте проверим бронирование. Ваш рейс двадцать третьего марта в двадцать один ноль ноль по маршруту Екатеринбург Владивосток. Аэропорт вылета Кольцово, аэропорт прилета Кневичи. Все верно?`.
> Текстовый шаблон должен выглядеть так: `Давайте проверим бронирование. Ваш рейс {date} по маршруту {route}. Аэропорт вылета {source}, аэропорт прилета {destination}. Все верно?`
> Список переменных: `variable_name = '{date}', variable_value = 'восьмого сентября в одиннадцать двадцать'`, `variable_name = '{route}', variable_value = 'Москва Санкт-Петербург'`, `variable_name = '{source}', variable_value = 'Домодедово'`, `variable_name = '{destination}', variable_value = 'Пулково'`.

Пример реализации синтеза по шаблонам см. на странице [{#T}](api/tts-templates.md).

### Требования к аудиозаписям шаблонов {#requirements-audio}

| Требование | Значение |
| --- | --- |
| Частота дискретизации | 48 кГц для {{ brand-voice-core-name }} и {{ brand-voice-premium-name }}</br>{{ tts-cc-quality }} или выше для {{ brand-voice-cc-name }} |
| Глубина аудио (audio bit depth) |  16 бит PCM |
| Количество каналов |  1 (моно) |
| Формат | [WAV](https://ru.wikipedia.org/wiki/WAV) |

Аудиозаписи шаблонов не должны содержать посторонние шумы и эхо. Допускается минимальная обработка аудиозаписи. Длительность тишины в начале и в конце записи — не более 1 секунды.

### Рекомендации для записи аудио {#tips}

{% include notitle [tips](../../_includes/speechkit/audio-tips.md) %}

### Требования к синтезируемым текстам {#requirements-text}

Шаблон должен соответствовать желаемому скрипту.

В шаблоне должна быть одна или несколько переменных для замены. Длительность звучания переменной части должна соответствовать образцу в шаблоне. Если длительность звучания переменной части может сильно отличаться, рекомендуем записать несколько шаблонов с примерами разной длины.

> Например, если в шаблоне `Здравствуйте, меня зовут Ирина, я работаю в {company}.` длина переменной `company` может быть очень разной (`компании Облака`, `банке Надежность`, `региональном представительстве известной зарубежной компании`), запишите два шаблона — для коротких и длинных значений. В зависимости от подставляемого значения передавайте при синтезе один из них. Так синтезированная речь останется естественной.

Длинные реплики диктора необходимо разбить на самостоятельные предложения и разные шаблоны. Синтезируемая фраза не может превышать {{ tts-v3-time }} и {{ tts-v3-count }}.

При записи речи на русском языке обязательно указывайте букву <q>ё</q> в расшифровке. Использование <q>е</q> вместо <q>ё</q> в любых текстах для синтеза недопустимо. В словах-омографах, где ударение может быть поставлено неоднозначно, необходимо явно указать ударную гласную знаком `+`.
   > _<q>Он вставил ключ в зам+ок.</q>_, ударение падает на второй слог.
   > _<q>Михайловский з+амок — главный корпус Пушкинского музея.</q>_, ударение падает на первый слог.

Все данные необходимо денормализовать, чтобы строки текста для синтеза не содержали цифр и сокращений вида <q>ул. Строителей, кв.15 к.3</q> или <q>13 руб 10 коп</q>. Цифры и числа необходимо записать прописью, сокращения — полностью раскрыть, аббревиатуры можно оставить как есть или развернуть.

В вопросительных предложениях необходимо указать, на какое слово приходится \*\*логическое ударение\*\* — вопросительная интонация предложения. 

{% note warning %}

Логическое ударение не должно падать на переменную часть фразы.

{% endnote %}

> Предложение <q>Кот пошел в лес?</q> можно прочитать тремя разными способами:
> * \*\*Кот\*\* пошел в лес? — со смыслом <q>Кто пошел в лес? Это был действительно кот?</q>
> * Кот \*\*пошел\*\* в лес? — со смыслом <q>Кот пошел или побежал?</q> или <q>Было ли совершено само действие? Кот ушел или нет?</q>
> * Кот пошел \*\*в лес\*\*? — со смыслом <q>Куда или за чем пошел кот? В лес, в школу, за колбасой?</q>
>
> Во всех предложениях логическое ударение выделяет основной смысл предложения.

## Использование синтеза по шаблонам {#templates-implementation}

Синтез по шаблону работает только для [API v3](../tts-v3/api-ref/grpc/). Примеры реализации:

* [{{ brand-voice-premium }} и {{ brand-voice-core }}](api/tts-templates.md).
* [{{ brand-voice-cc-name }}](api/tts-templates-bvcc.md).
