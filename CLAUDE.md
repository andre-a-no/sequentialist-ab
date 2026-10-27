# sequentialist-ab

Движок платформы A/B-тестов (`sequentialist.ab`) и клиент назначения
вариантов для браузеров и Node.js (`ts/ab-client`, npm
`@sequentialist/ab-client`). Версия 0.1.0, не опубликована.

## Семейство

Библиотеки sequentialist — прикладной слой над ядром
[sequential-analysis](https://github.com/andre-a-no/sequential-analysis)
(PyPI `sequential-analysis`, импорт `sequential_analysis`, зависимость
`>=0.4,<0.5`). Каждая библиотека живёт в своём репозитории и ставится
отдельно; общее пространство имён `sequentialist` (PEP 420, без
`__init__.py` на этом уровне).

| Репозиторий | PyPI | Импорт |
|---|---|---|
| sequentialist-core | `sequentialist-core` | `sequentialist.core` — общий слой |
| sequentialist-ab | `sequentialist-ab`, npm `@sequentialist/ab-client` | `sequentialist.ab` |
| sequentialist-quality | `sequentialist-quality` | `sequentialist.quality` |
| sequentialist-monitor | `sequentialist-monitor` | `sequentialist.monitor` |
| sequentialist-markets | `sequentialist-markets` | `sequentialist.markets` |
| sequentialist-clinical | `sequentialist-clinical` | `sequentialist.clinical` |
| — (позже) | `sequentialist-release`, `sequentialist-mlops` | |

sequentialist-core пока не на PyPI: прикладные пакеты ставят его по коммиту
`27f71847ea39cedf0a8e41a5f05042428a533443` (подкаталог `packages/core`),
см. CI. Когда core выйдет на PyPI, эту строку заменить на версию.

## Постоянные правила

- **Статистика — только в ядре.** Ядро и sequentialist-core из этого
  репозитория не править. Чего не хватает — список запросов: что нужно,
  зачем, какой интерфейс. Запрос к core — то, что нужно хотя бы половине
  прикладных пакетов; что уже в core, обратно не выносится.
- **Гарантии.** Каждый вывод называет свою гарантию (`exact`, `asymptotic`,
  `model`, `approximate`, `none`) так, как её объявляет процедура ядра.
  Утверждения честные и сдержанные; ничего не обещать без проверки тестом.
- **Сверка с эталонами.** Каждая процедура и каждый результат сверяются с
  внешней эталонной реализацией или закрытой формулой, где она есть.
- **Python ≥3.8**, обязательных зависимостей, кроме ядра и core, нет.
  Перед пушем: `ruff check .`, `mypy`, тесты, `vermin -t=3.8-`; CI должен
  быть зелёным.
- **Лицензии:** AGPL-3.0-only или коммерческая (COMMERCIAL.md); в каждом
  исходном файле SPDX-заголовок без города; CLA обязателен.
- **Контакты** в публичных файлах — только email: a.hobukob@gmail.com,
  andrei@hobukob.ru. Никаких телефонов, адрес компании не указывать.
- **Никаких идентификаторов моделей** в коммитах, PR и коде.
- **Персональные данные:** 152-ФЗ и GDPR; движок работает с псевдонимами,
  хэшами и агрегатами.
- PR — только по просьбе владельца; ничего не мержить без его явного
  подтверждения.
- Общение с владельцем — по-русски; код, документация и коммиты —
  по-английски.

## Особенности этого репозитория

- `ts/ab-client` и `vectors/` — под Apache-2.0 (окончательно не решено,
  альтернатива — MIT; решает владелец). Назначение в Python и TS совпадает
  бит в бит по `vectors/assignment.json` (SHA-256 от домена, salt и
  единицы, 53 бита; домены traffic и variant).
- Сгенерированные файлы: `tools/make_vectors.py`, `tools/make_schemas.py`
  (с `--check` в CI).
- Эталоны: GrowthBook (gbstats 0.8.0, Python 3.11) и mSPRT, данные
  Hillstrom с проверкой sha256 (`references/`, задача CI references).
- Публикации (TestPyPI rc, npm-организация) — только по команде владельца.

## Запросы к ядру (не выполнены)

1. Адаптивный к дисперсии интервал `BoundedABTest`: сейчас полуширина
   около 0,039 при любой конверсии (36 стандартных ошибок при p = 0,6 %),
   guardrail по редкой конверсии не подтверждается.
2. Доверительная последовательность в `TwoProportionTest`.
3. `e_value_field` у `BoundedABTest`, `RateRatioTest`, `RatioABTest` — для
   e-BH по всем метрикам.
4. Non-inferiority margin и `alternative` для `RatioABTest`.
5. В конфигурации процедуры — запрошенный бэкенд, а не разрешённый.
6. Скорость `BoundedABTest` (0,6 мс на шаг в Python, 0,2 мс с numpy).
7. Публичные `canonical_json` и `loads`.
8. TS-исполнитель ядра в npm и для браузера.
