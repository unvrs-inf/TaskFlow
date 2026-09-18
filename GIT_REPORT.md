# GIT Report — TaskFlow

## 1.

В работе использовались долгоживущие ветки `main` и `develop`, временные ветки `feature/*` и `hotfix/*`, а изменения передавались через Pull Request.

---

## 2. Настройка Git и репозитория

Для работы были настроены пользовательские данные Git:

```bash
git config --global user.name
git config --global user.email
```

Работа выполнялась в собственном GitHub-репозитории:

```text
https://github.com/unvrs-inf/TaskFlow
```

Удалённый репозиторий был подключён под именем `origin`.

Основная ветка проекта — `main`. После создания первого снимка от `main` была создана ветка `develop`.

Структура долгоживущих веток:

```text
main
develop
```

---

## 3. Работа с main и develop

Ветка `main` используется как стабильная и релизная линия проекта.

Ветка `develop` используется для интеграции текущей разработки.

После создания первого снимка репозитория ветка `develop` была создана от `main`:

```bash
git switch main
git switch -c develop
git push -u origin develop
```

После этого основная разработка выполнялась не напрямую в `main` или `develop`, а во временных ветках.

---

## 4. Feature-ветка

Для выполнения основной части задания была создана feature-ветка от актуального состояния `develop`:

```bash
git switch develop
git pull origin develop
git switch -c feature/<login>-intro
```

В feature-ветке были созданы и изменены необходимые файлы:

```text
students/<login>/ABOUT.md
students/<login>/NOTES.md
students/<login>/.env.example
pages/index.html
styles.css
GIT_REPORT.md
```

В feature-ветке было выполнено не менее трёх осмысленных коммитов.

Примеры сообщений коммитов:

```text
docs: add student profile
docs: add git and git flow notes
feat: add homework demo page
```

После завершения работы feature-ветка была отправлена в удалённый репозиторий:

```bash
git push -u origin feature/<login>-intro
```

Затем был создан Pull Request:

```text
feature/<login>-intro → develop
```

Целевая ветка Pull Request — `develop`.

---

## 5. Учебный merge conflict

Для практики разрешения конфликтов были использованы две feature-ветки, которые изменяли одну и ту же часть файла.

При попытке объединить ветки Git обнаружил конфликт.

Конфликт был разрешён вручную:

1. Открыт конфликтующий файл.
2. Удалены маркеры конфликта:
   `<<<<<<<`, `=======`, `>>>>>>>`.
3. Выбран итоговый вариант содержимого.
4. Файл добавлен в staging.
5. Выполнен коммит, завершающий merge.

После разрешения конфликта в файлах не осталось конфликтных маркеров.

Для проверки истории использовалась команда:

```bash
git log --oneline --graph --all
```

---

## 6. Учебный hotfix

Для имитации срочного исправления была создана ветка:

```text
hotfix/<login>-typo
```

Ветка была создана непосредственно от `main`:

```bash
git switch main
git pull origin main
git switch -c hotfix/<login>-typo
```

В ветке была исправлена опечатка в файле:

```text
sandbox/hello.txt
```

После исправления был создан коммит:

```text
fix: correct hello text typo
```

Hotfix был объединён:

```text
hotfix/<login>-typo → main
hotfix/<login>-typo → develop
```

Это необходимо для того, чтобы исправление присутствовало как в стабильной линии `main`, так и в дальнейшей разработке `develop`.

---

## 7. Работа с .gitignore и секретами

В репозитории создан `.gitignore`, исключающий локальные файлы и каталоги, которые не должны попадать в Git.

В частности:

```gitignore
.env
.venv/
__pycache__/
*.pyc
```

Для демонстрации конфигурации без секретов был создан:

```text
students/<login>/.env.example
```

Файл `.env` не добавлялся в репозиторий.

Таким образом, реальные секреты и локальные конфигурационные файлы не должны попадать в remote.

---

## 8. Используемая модель Git Flow

В проекте используются следующие типы веток:

| Ветка       | Откуда создаётся | Куда вливается               | Назначение                                       |
| ----------- | ---------------- | ---------------------------- | ------------------------------------------------ |
| `main`      | —                | —                            | Стабильная и релизная линия                      |
| `develop`   | `main`           | —                            | Интеграция текущей разработки                    |
| `feature/*` | `develop`        | `develop`                    | Разработка отдельной задачи или функциональности |
| `release/*` | `develop`        | `main` и обратно в `develop` | Подготовка конкретного релиза                    |
| `hotfix/*`  | `main`           | `main` и `develop`           | Срочное исправление уже выпущенной версии        |

Главное правило Git Flow в рамках задания:

```text
feature/* → develop
```

Feature-ветки не должны напрямую вливать изменения в `main`.

---

## 9. Проверка истории

Для проверки структуры веток и истории использовалась команда:

```bash
git log --oneline --graph --all
```

Она позволяет увидеть разветвление feature/hotfix-веток и последующие слияния.

Также использовались:

```bash
git status
git branch
git branch -vv
git remote -v
```

---

## 10. Итог

В результате работы:

* создан и настроен Git-репозиторий;
* созданы ветки `main` и `develop`;
* создана feature-ветка от актуального `develop`;
* выполнено более трёх осмысленных коммитов;
* создан и разрешён учебный merge conflict;
* выполнен учебный hotfix от `main`;
* hotfix объединён в `main` и `develop`;
* создан `.gitignore`;
* добавлен `.env.example`;
* реальные секреты в репозиторий не добавлялись;
* подготовлен Pull Request `feature/<login>-intro → develop`.

Основной рабочий поток проекта:

```text
develop
   ↓
feature/<login>-intro
   ↓
Pull Request
   ↓
develop
```

Поток срочного исправления:

```text
main
 ↓
hotfix/<login>-typo
 ├──→ main
 └──→ develop
```
