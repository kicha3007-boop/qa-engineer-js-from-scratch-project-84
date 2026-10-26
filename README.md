# Тестирование интернет-магазина

[![hexlet-check](https://github.com/kicha3007-boop/qa-engineer-js-from-scratch-project-84/actions/workflows/hexlet-check.yml/badge.svg)](https://github.com/kicha3007-boop/qa-engineer-js-from-scratch-project-84/actions)

Ручное тестирование учебного интернет-магазина Хекслета: требования, тест-кейсы, отчёт о прогоне,
баг-репорты со скриншотами и логами DevTools и регрессионное тестирование новой версии.

Учебный проект Хекслета: https://ru.hexlet.io/programs/qa-engineer-js-from-scratch

## Что тестировалось

| Версия | Адрес | Для чего |
|---|---|---|
| 1 | https://products-store-ru.hexlet.app/ | основной прогон всех тест-кейсов |
| 2 | https://products-store-v2-ru.hexlet.app/ | регресс: перепроверка упавших кейсов |

Проверены каталог (карточки, пагинация), поиск, фильтры (категория, производитель, наличие, цена),
страница товара, корзина (добавление, удаление, очистка, сохранение после перезагрузки),
оформление заказа, переключение языка RU/EN, мобильная ширина 375px, загрузка ресурсов и консоль DevTools,
HTTPS, скорость загрузки, работа в Chrome и Edge.

Окружение прогона: Google Chrome 156.0.8078.47 и Microsoft Edge 156.0.4314.42, Windows 11 Pro 10.0.22000,
окно 1366x900 и эмуляция экрана 375x812 в DevTools. Дата прогона — 26.10.2026.

## Итоги

- Требований: 52 (26 функциональных, 8 нефункциональных, 18 неявных).
- Тест-кейсов: 54; на версии 1 — 30 pass, 24 fail; на каждый fail заведён баг-репорт (24).
- Регресс на версии 2: из 24 упавших кейсов исправлено 5, остальные 19 падают по-прежнему;
  найдено 3 новых дефекта (ещё 4 кейса, проходившие на версии 1, на версии 2 падают).

## Файлы

| Файл | Содержимое |
|---|---|
| `about.yml` | сведения об авторе |
| `requirements.yml` | требования: `functional`, `non-functional`, `implicit` |
| `test-cases.yml` | тест-кейсы: summary, requirement, prerequisites, test-data, steps, expected-result |
| `testing-report.yml` | статусы всех тест-кейсов на версии 1 (`pass` / `fail`) |
| `bugreports.yml` | баг-репорты на все упавшие на версии 1 кейсы |
| `regress-report.yml` | регресс на версии 2: статусы перепроверенных кейсов и новые баг-репорты |
| `assets/` | скриншоты (`.png`), журналы DevTools Console/Network и замеры (`.txt`); префикс `v1-`/`v2-` — версия приложения |

## Как повторить проверку самому

Нужен только браузер (Chrome или Edge), Хекслет не нужен.

1. Откройте https://products-store-ru.hexlet.app/ (версия 1, в подвале «version 1»).
2. Откройте DevTools (F12): вкладку **Console** — для ошибок скриптов, **Network** — для кодов ответов
   (включите *Disable cache*), **Application > Session Storage** — там хранится корзина (ключ `cart`).
3. Проходите кейсы из `test-cases.yml` по порядку. Каждый кейс независим: перед ним выполните его
   `prerequisites` (для пустой корзины — «Clear cart» и перезагрузка или новая вкладка, так как корзина живёт
   в сессии вкладки). Сверяйте результат с `expected-result` и сравнивайте со статусом в `testing-report.yml`.
4. Мобильные кейсы — в режиме Device Toolbar (Ctrl+Shift+M) с размером 375x812.
5. Кейс с 50 одновременными запросами выполняется скриптом Node.js 18+, например:

   ```bash
   node -e "Promise.all(Array.from({length:50},(_,i)=>{const s=Date.now();return fetch('https://products-store.hexlet.app/api/products?skip='+(i%7)*30).then(r=>[r.status,Date.now()-s])})).then(r=>console.log(r))"
   ```

6. Для регресса повторите кейсы из `regress-report.yml` на https://products-store-v2-ru.hexlet.app/
   (в адресах из кейсов замените хост на версию 2).
7. Найденный дефект сверяйте с `bugreports.yml` (версия 1) и разделом `bugreports` в `regress-report.yml`
   (новые дефекты версии 2); доказательства — в `assets/`.

Проверить, что yml-файлы читаются:

```bash
npx --yes js-yaml about.yml > /dev/null && echo ok
```

---

<details>
<summary>Автоматические тесты Хекслета</summary>

Тесты запускаются на каждый коммит. За запуск отвечает файл `.github/workflows/hexlet-check.yml` — не удаляйте и не переименовывайте ни его, ни репозиторий.

</details>

## О Хекслете

[Хекслет](https://ru.hexlet.io/) — школа программирования: авторские программы обучения с практикой, поддержкой наставников и реальными проектами, которые остаются в резюме. Этот репозиторий — один из таких проектов.
