# Lab_Rigorous

Рабочий репозиторий для курса **Rigorous Methods for Software Engineering**.

Окружение используется через **GitHub Codespaces**.

## Структура репозитория

```text
Lab_Rigorous/
├── README.md
├── projects/
│   ├── rigorous_lab/
│   │   ├── alire.toml
│   │   ├── rigorous_lab.gpr
│   │   ├── alire/
│   │   └── src/
│   │       └── rigorous_lab.adb
│   │
│   ├── lab02/
│   │   ├── alire.toml
│   │   ├── lab02.gpr
│   │   └── src/
│   │
│   └── ...
│
└── ...
```

`Lab_Rigorous` является общей папкой курса.

Каждая папка внутри `projects/` является отдельным Alire-проектом.

---

# 1. Открытие Codespace

На странице репозитория GitHub:

```text
Code → Codespaces → Create codespace on main
```

После запуска Codespace рабочий каталог репозитория:

```bash
/workspaces/Lab_Rigorous
```

---

# 2. Проверка Alire

```bash
alr --version
```

Ожидаемый результат:

```text
alr 2.1.1
```

---

# 3. Создание нового проекта

Перейти в каталог проектов:

```bash
cd /workspaces/Lab_Rigorous/projects
```

Создать папку нового проекта:

```bash
mkdir lab01
cd lab01
```

Создать бинарный Ada-проект:

```bash
alr init --in-place --bin lab01
```

На вопросы Alire можно нажимать `Enter`, если специальные значения не требуются.

После создания структура будет примерно такой:

```text
lab01/
├── alire.toml
├── lab01.gpr
└── src/
    └── lab01.adb
```

---

# 4. Добавление SPARK / GNATprove

В папке проекта выполнить:

```bash
alr with gnatprove
```

При первом запуске Alire также может автоматически предложить установить:

```text
GNAT
GPRbuild
```

Подтвердить установку нажатием `Enter`.

---

# 5. Сборка проекта

Находясь в папке проекта:

```bash
alr build
```

Например:

```bash
cd /workspaces/Lab_Rigorous/projects/lab01
alr build
```

При успешной сборке:

```text
Build finished successfully
```

Исполняемый файл обычно создаётся в:

```text
bin/
```

Запуск:

```bash
./bin/lab01
```

---

# 6. Работа со SPARK

Минимальный SPARK-файл:

```ada
procedure Lab01
  with SPARK_Mode
is
   X : Integer := 1;
begin
   pragma Assert (X = 1);
end Lab01;
```

Проверка GNATprove:

```bash
alr gnatprove -P lab01.gpr
```

Более высокий уровень доказательства:

```bash
alr gnatprove -P lab01.gpr --level=2
```

---

# 7. Проверка GNATprove

```bash
alr gnatprove --version
```

GNATprove устанавливается как зависимость Alire-проекта, поэтому обычная команда:

```bash
gnatprove --version
```

может не работать.

Использовать:

```bash
alr gnatprove --version
```

или:

```bash
alr exec -- gnatprove --version
```

---

# 8. Важное правило имён

Для простоты рекомендуется использовать одинаковое имя проекта во всех основных файлах.

Например проект:

```text
lab01
```

должен иметь:

```text
lab01/
├── alire.toml
├── lab01.gpr
└── src/
    └── lab01.adb
```

В `alire.toml`:

```toml
name = "lab01"
```

В `lab01.gpr`:

```gpr
project Lab01 is

   for Source_Dirs use ("src");
   for Object_Dir use "obj";
   for Exec_Dir use "bin";

   for Main use ("lab01.adb");

end Lab01;
```

В `src/lab01.adb`:

```ada
procedure Lab01
  with SPARK_Mode
is
begin
   null;
end Lab01;
```

---

# 9. Работа с существующим проектом

Перейти в папку проекта:

```bash
cd /workspaces/Lab_Rigorous/projects/rigorous_lab
```

Собрать:

```bash
alr build
```

Запустить GNATprove:

```bash
alr gnatprove -P rigorous_lab.gpr
```

---

# 10. Несколько исходных файлов

Дополнительные `.ads` и `.adb` файлы помещаются в `src/`.

Например:

```text
src/
├── lab01.adb
├── maths.ads
└── maths.adb
```

Alire/GPRbuild автоматически найдут исходники через:

```gpr
for Source_Dirs use ("src");
```

---

# 11. Очистка результатов сборки

При необходимости удалить результаты предыдущей сборки:

```bash
rm -rf bin obj
```

Затем:

```bash
alr build
```

Каталоги будут созданы заново.

---

# 12. Типичные команды

Перейти к проекту:

```bash
cd /workspaces/Lab_Rigorous/projects/lab01
```

Собрать:

```bash
alr build
```

Проверить SPARK:

```bash
alr gnatprove -P lab01.gpr
```

Проверить версию GNATprove:

```bash
alr gnatprove --version
```

Запустить программу:

```bash
./bin/lab01
```

---

# 13. Git

После создания или изменения проекта:

```bash
cd /workspaces/Lab_Rigorous

git status
git add .
git commit -m "Add lab01"
git push
```

Перед commit рекомендуется проверить проект:

```bash
cd projects/lab01

alr build
alr gnatprove -P lab01.gpr
```

---

# Краткая схема

Создание нового проекта:

```bash
cd /workspaces/Lab_Rigorous/projects

mkdir lab01
cd lab01

alr init --in-place --bin lab01
alr with gnatprove
```

Работа:

```bash
alr build
alr gnatprove -P lab01.gpr
```

Основное правило:

```text
Lab_Rigorous
    ↓
projects
    ↓
отдельная папка для каждого Alire/SPARK проекта
```

## Примечание

Установка самого Alire относится к настройке Codespace, а не к созданию каждого нового проекта.

Если `alr` уже установлен в Codespace, для новых лабораторных достаточно:

```bash
mkdir ...
alr init ...
alr with gnatprove
```
