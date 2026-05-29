# e2e-testing-tool

CLI-инструмент для запуска E2E-тестов веб-приложений через Playwright. Пишешь обычные Python-функции, запускаешь одной командой — получаешь результаты в терминале и HTML-отчёт.

В репозитории два компонента: сам инструмент (`e2e_runner/`) и демо-приложение на Flask (`demo_app/`), на котором можно сразу всё попробовать.

## Требования

- Python 3.10+
- pip

## Установка

```bash
cd e2e_runner
pip install -e .
pip install textual
playwright install chromium
```

## Запуск демо

Сначала поднимаем демо-приложение:

```bash
cd demo_app
python app.py
```

Оно запустится на `http://localhost:8080`. В другом терминале запускаем тесты:

```bash
e2e run demo_app/tests -u http://localhost:8080
```

Три теста из 22 упадут намеренно — так устроена демонстрация retry и fail-fast.

## Как писать свои тесты

Файл называется `test_*.py`, функции — `test_*`. Аргументы `page` и `base_url` подставляются автоматически:

```python
def test_title(page, base_url):
    page.goto(base_url)
    assert "Мой сайт" in page.title()
```

## Команды

```bash
e2e run <папка>            # запустить тесты
e2e run <папка> -k login   # только тесты с "login" в названии
e2e init                   # создать e2e.yaml с настройками
e2e history                # история запусков
e2e export <id>            # экспорт отчёта в HTML/JSON
e2e ui                     # TUI-интерфейс
```

## Конфиг

`e2e init` создаёт `e2e.yaml` — можно прописать путь к тестам, base_url, количество воркеров и прочее, чтобы не передавать флаги каждый раз.
