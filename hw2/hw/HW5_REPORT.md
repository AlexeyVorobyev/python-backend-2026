# Отчёт по ДЗ 5: покрытие тестами HW2 и CI

## Что сделано

- Добавлены тесты для обработки отсутствующих корзин и товаров, проверки входных данных, фильтрации списка корзин, мягкого удаления товаров и расчёта стоимости корзины.
- Проверены WebSocket-рассылки между участниками одной комнаты, изоляция разных комнат, отключение отправителя и удаление недоступного клиента.
- Добавлен `requirements-hw5.txt`, который включает `requirements.txt` и добавляет `pytest-cov`; зависимости существующего HW2 pipeline не меняются.
- Добавлен отдельный workflow `.github/workflows/hw5-tests.yml`: он запускает тесты на Python 3.13 и 3.14, выводит пропущенные строки и завершает job с ошибкой, если покрытие пакета `shop_api` ниже 95%.
- Новый workflow запускается при изменении файлов HW2 или самого workflow: при push в любую ветку и при pull request в `main`. Существующий workflow `.github/workflows/hw2-tests.yml` оставлен без изменений.

## Покрытие

| Python | Тесты | `shop_api` | `main.py` | `database.py` |
| --- | ---: | ---: | ---: | ---: |
| 3.13 | 47 passed | 99.47% | 100% | 97% |
| 3.14 | 47 passed | 99.46% | 100% | 97% |

Одна строка в `database.py` не покрыта: настройка PostgreSQL engine при установленной переменной `DATABASE_URL`. CI использует встроенную SQLite базу; общее покрытие пакета при этом выше требуемых 95%.

## Запуск тестов

Из корня репозитория:

```bash
cd hw2/hw
pip install -r requirements-hw5.txt
PYTHONPATH="$PWD" pytest -v --cov=shop_api --cov-report=term-missing --cov-fail-under=95
```

## CI

После push GitHub Actions запускает отдельную проверку покрытия. Результаты доступны в [workflow HW5 Coverage](https://github.com/AlexeyVorobyev/python-backend-2026/actions/workflows/hw5-tests.yml).
