# Random Password Generator — пошаговая инструкция по созданию приложения

## 1. Структура проекта

Создайте папку `password_generator` и добавьте в неё:
- файл `main.py` — основной код приложения;
- файл `history.json` — для хранения истории паролей;
- файл `.gitignore` — чтобы не отслеживать временные файлы;
- файл `README.md` — описание проекта.

## 2. Основной код (`main.py`)

```python
import tkinter as tk
from tkinter import ttk, messagebox
import json
import random
import string

HISTORY_FILE = 'history.json'
MIN_LENGTH = 4
MAX_LENGTH = 32

def load_history():
    try:
        with open(HISTORY_FILE, 'r', encoding='utf-8') as f:
            return json.load(f)
    except FileNotFoundError:
        return []

def save_history(data):
    with open(HISTORY_FILE, 'w', encoding='utf-8') as f:
        json.dump(data, f, ensure_ascii=False, indent=2)

def generate_password():
    length = scale_length.get()
    use_digits = var_digits.get()
    use_letters = var_letters.get()
    use_special = var_special.get()

    if not (use_digits or use_letters or use_special):
        messagebox.showerror("Ошибка", "Выберите хотя бы один тип символов.")
        return

    chars = ''
    if use_digits: chars += string.digits
    if use_letters: chars += string.ascii_letters
    if use_special: chars += string.punctuation

    password = ''.join(random.choices(chars, k=length))
    entry_password.delete(0, tk.END)
    entry_password.insert(0, password)

    history.append(password)
    save_history(history)
    refresh_history()

def refresh_history():
    for item in tree.get_children():
        tree.delete(item)
    for pwd in history[::-1]:
        tree.insert("", "end", values=(pwd,))

history = load_history()

root = tk.Tk()
root.title("Random Password Generator")
root.geometry("500x400")

frame_options = ttk.LabelFrame(root, text="Параметры пароля")
frame_options.pack(padx=10, pady=10, fill="x")

ttk.Label(frame_options, text="Длина:").grid(row=0, column=0, padx=5, pady=5, sticky="w")
scale_length = tk.Scale(frame_options, from_=MIN_LENGTH, to=MAX_LENGTH, orient="horizontal")
scale_length.set(12)
scale_length.grid(row=0, column=1, padx=5, pady=5)

var_digits = tk.BooleanVar(value=True)
ttk.Checkbutton(frame_options, text="Цифры", variable=var_digits).grid(row=1, column=0, padx=5, pady=5, sticky="w")
var_letters = tk.BooleanVar(value=True)
ttk.Checkbutton(frame_options, text="Буквы", variable=var_letters).grid(row=1, column=1, padx=5, pady=5, sticky="w")
var_special = tk.BooleanVar(value=True)
ttk.Checkbutton(frame_options, text="Спецсимволы", variable=var_special).grid(row=2, column=0, padx=5, pady=5, sticky="w")

btn_generate = ttk.Button(frame_options, text="Сгенерировать", command=generate_password)
btn_generate.grid(row=2, column=1, padx=5, pady=5)

ttk.Label(root, text="Сгенерированный пароль:").pack(padx=10, pady=5, anchor="w")
entry_password = ttk.Entry(root, width=40)
entry_password.pack(padx=10, pady=5, fill="x")

frame_history = ttk.LabelFrame(root, text="История паролей")
frame_history.pack(padx=10, pady=10, fill="both", expand=True)

tree = ttk.Treeview(frame_history, columns=("Пароль",), show="headings")
tree.heading("Пароль", text="Пароль")
tree.pack(side="left", fill="both", expand=True)
scrollbar = ttk.Scrollbar(frame_history, orient="vertical", command=tree.yview)
scrollbar.pack(side="right", fill="y")
tree.configure(yscrollcommand=scrollbar.set)

refresh_history()
root.mainloop()
```

## 3. Файл `.gitignore`

```
__pycache__/
*.pyc
*.log
*.swp
*.bak
```
*(Если хотите хранить историю в Git — уберите `history.json` из .gitignore)*

## 4. README.md (пример оформления)

```
# Random Password Generator

**Автор:** Иван Иванов

## Описание программы

Random Password Generator — приложение для генерации случайных паролей с графическим интерфейсом. Позволяет настраивать длину пароля и состав символов (цифры, буквы, спецсимволы), а также сохранять историю сгенерированных паролей в файл JSON.

## Как использовать

1. Установите Python 3.x.
2. Скопируйте файлы проекта в одну папку.
3. Запустите main.py.
4. Настройте параметры пароля (длина и типы символов).
5. Нажмите «Сгенерировать».
6. История паролей отображается в таблице.
7. Данные сохраняются автоматически.

## Примеры использования

- Сгенерировать пароль длиной 16 символов с цифрами и буквами.
- Сгенерировать пароль только из спецсимволов.
- Проверить историю последних 10 паролей.
```

## 5. Использование Git

1. Откройте терминал в папке проекта.
2. Инициализируйте репозиторий:
   ```
   git init
   ```
3. Добавьте файлы:
   ```
   git add .
   ```
4. Сделайте первый коммит:
   ```
   git commit -m "Initial commit"
   ```
5. (Опционально) Создайте репозиторий на GitHub/GitLab и залейте проект.
```
git remote add origin <ваш_репозиторий>
git push -u origin master
```
```
