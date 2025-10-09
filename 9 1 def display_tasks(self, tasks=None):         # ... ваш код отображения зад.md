## Реализация метода `display_tasks()`

```python
def display_tasks(self, tasks=None):
    """Отображает список задач в удобном формате"""
    if tasks is None:
        tasks = self.manager.get_all_tasks()
    
    if not tasks:
        print("📭 Задачи не найдены")
        return
    
    print(f"\n📋 НАЙДЕНО ЗАДАЧ: {len(tasks)}")
    print("="*80)
    
    for i, task in enumerate(tasks, 1):
        # Определяем emoji для статуса
        status_emoji = {
            Status.NEW: "🆕",
            Status.IN_PROGRESS: "🔄", 
            Status.COMPLETED: "✅"
        }
        
        # Определяем emoji для приоритета
        priority_emoji = {
            Priority.LOW: "⚪",
            Priority.MEDIUM: "🟡",
            Priority.HIGH: "🔴"
        }
        
        print(f"#{i}")
        print(f"  ID: {task.id}")
        print(f"  📝 Заголовок: {task.title}")
        print(f"  📄 Описание: {task.description}")
        print(f"  {status_emoji[task.status]} Статус: {task.status.value}")
        print(f"  {priority_emoji[task.priority]} Приоритет: {task.priority.value}")
        print(f"  📅 Создана: {task.created_date.strftime('%d.%m.%Y %H:%M')}")
        print(f"  🔄 Обновлена: {task.updated_date.strftime('%d.%m.%Y %H:%M')}")
        print("-" * 80)
```

## 📝 Подробное объяснение:

### **Что делает метод:**
1. **Принимает список задач** (если None - берет все задачи)
2. **Проверяет наличие задач** и выводит сообщение если пусто
3. **Форматирует вывод** каждой задачи в читаемом виде
4. **Использует emoji** для визуального улучшения
5. **Форматирует даты** в удобный формат

### **Логика отображения:**

| Элемент | Форматирование | Пример |
|---------|----------------|---------|
| Заголовок | Прямой вывод | "Изучить Python" |
| Описание | Прямой вывод | "Освоить основы" |
| Статус | Emoji + значение | "🔄 в работе" |
| Приоритет | Emoji + значение | "🔴 высокий" |
| Даты | Формат ДД.ММ.ГГГГ ЧЧ:ММ | "15.01.2024 14:30" |

---

## 🔧 Альтернативные варианты реализации:

### **Вариант 1: Компактный вывод**
```python
def display_tasks(self, tasks=None):
    """Компактное отображение задач в виде таблицы"""
    if tasks is None:
        tasks = self.manager.get_all_tasks()
    
    if not tasks:
        print("📭 Задачи не найдены")
        return
    
    print(f"\n📋 ЗАДАЧИ ({len(tasks)})")
    print("─" * 100)
    print(f"{'ID':<4} {'Статус':<12} {'Приоритет':<10} {'Заголовок':<30} {'Создана':<16}")
    print("─" * 100)
    
    for task in tasks:
        status_icon = "🆕" if task.status == Status.NEW else "🔄" if task.status == Status.IN_PROGRESS else "✅"
        priority_icon = "🔴" if task.priority == Priority.HIGH else "🟡" if task.priority == Priority.MEDIUM else "⚪"
        
        # Обрезаем длинный заголовок
        title = task.title[:27] + "..." if len(task.title) > 30 else task.title
        
        print(f"{task.id:<4} {status_icon} {task.status.value:<8} {priority_icon} {task.priority.value:<8} {title:<30} {task.created_date.strftime('%d.%m.%Y'):<16}")
    
    print("─" * 100)
```

### **Вариант 2: Группировка по статусу**
```python
def display_tasks(self, tasks=None):
    """Отображение задач с группировкой по статусу"""
    if tasks is None:
        tasks = self.manager.get_all_tasks()
    
    if not tasks:
        print("📭 Задачи не найдены")
        return
    
    # Группируем задачи по статусу
    tasks_by_status = {
        Status.NEW: [],
        Status.IN_PROGRESS: [],
        Status.COMPLETED: []
    }
    
    for task in tasks:
        tasks_by_status[task.status].append(task)
    
    # Выводим по группам
    for status, status_tasks in tasks_by_status.items():
        if status_tasks:
            status_emoji = "🆕" if status == Status.NEW else "🔄" if status == Status.IN_PROGRESS else "✅"
            print(f"\n{status_emoji} {status.value.upper()} ({len(status_tasks)})")
            print("─" * 60)
            
            for task in status_tasks:
                priority_icon = "🔴" if task.priority == Priority.HIGH else "🟡" if task.priority == Priority.MEDIUM else "⚪"
                print(f"  {priority_icon} [{task.id}] {task.title}")
```

### **Вариант 3: Подробный вывод с цветами**
```python
def display_tasks(self, tasks=None):
    """Подробное отображение с цветовым оформлением"""
    # Коды цветов (для поддерживаемых терминалов)
    COLORS = {
        'red': '\033[91m',
        'green': '\033[92m',
        'yellow': '\033[93m',
        'blue': '\033[94m',
        'magenta': '\033[95m',
        'cyan': '\033[96m',
        'reset': '\033[0m',
        'bold': '\033[1m'
    }
    
    if tasks is None:
        tasks = self.manager.get_all_tasks()
    
    if not tasks:
        print(f"{COLORS['yellow']}📭 Задачи не найдены{COLORS['reset']}")
        return
    
    print(f"\n{COLORS['bold']}{COLORS['blue']}📋 НАЙДЕНО ЗАДАЧ: {len(tasks)}{COLORS['reset']}")
    print("=" * 80)
    
    for i, task in enumerate(tasks, 1):
        # Цвета в зависимости от статуса
        status_color = {
            Status.NEW: COLORS['blue'],
            Status.IN_PROGRESS: COLORS['yellow'],
            Status.COMPLETED: COLORS['green']
        }
        
        # Цвета в зависимости от приоритета
        priority_color = {
            Priority.LOW: COLORS['cyan'],
            Priority.MEDIUM: COLORS['yellow'],
            Priority.HIGH: COLORS['red']
        }
        
        print(f"{COLORS['bold']}#{i}{COLORS['reset']}")
        print(f"  {COLORS['cyan']}ID:{COLORS['reset']} {task.id}")
        print(f"  {COLORS['cyan']}📝 Заголовок:{COLORS['reset']} {task.title}")
        print(f"  {COLORS['cyan']}📄 Описание:{COLORS['reset']} {task.description}")
        print(f"  {status_color[task.status]}📊 Статус:{COLORS['reset']} {task.status.value}")
        print(f"  {priority_color[task.priority]}⚡ Приоритет:{COLORS['reset']} {task.priority.value}")
        print(f"  {COLORS['magenta']}📅 Создана:{COLORS['reset']} {task.created_date.strftime('%d.%m.%Y %H:%M')}")
        print(f"  {COLORS['magenta']}🔄 Обновлена:{COLORS['reset']} {task.updated_date.strftime('%d.%m.%Y %H:%M')}")
        print("-" * 80)
```

### **Вариант 4: Минималистичная версия**
```python
def display_tasks(self, tasks=None):
    """Простое отображение задач"""
    if tasks is None:
        tasks = self.manager.get_all_tasks()
    
    if not tasks:
        print("Задачи не найдены")
        return
    
    print(f"\nНайдено задач: {len(tasks)}")
    for task in tasks:
        print(f"\nID: {task.id}")
        print(f"Заголовок: {task.title}")
        print(f"Описание: {task.description}")
        print(f"Статус: {task.status.value}")
        print(f"Приоритет: {task.priority.value}")
        print(f"Создана: {task.created_date.strftime('%d.%m.%Y %H:%M')}")
        print("-" * 40)
```

---

## 🎯 Интеграция с другими методами:

```python
def filter_by_status_ui(self):
    """UI для фильтрации по статусу с отображением результата"""
    print("\n🔍 ФИЛЬТРАЦИЯ ПО СТАТУСУ")
    print("1. 🆕 Новые")
    print("2. 🔄 В работе") 
    print("3. ✅ Завершенные")
    print("4. ↩️  Назад")
    
    choice = input("Выберите статус: ").strip()
    
    if choice == '1':
        filtered_tasks = self.manager.get_tasks_by_status(Status.NEW)
        self.display_tasks(filtered_tasks)
    elif choice == '2':
        filtered_tasks = self.manager.get_tasks_by_status(Status.IN_PROGRESS)
        self.display_tasks(filtered_tasks)
    elif choice == '3':
        filtered_tasks = self.manager.get_tasks_by_status(Status.COMPLETED)
        self.display_tasks(filtered_tasks)
    elif choice == '4':
        return
    else:
        print("❌ Некорректный выбор")

def filter_by_priority_ui(self):
    """UI для фильтрации по приоритету"""
    print("\n⚡ ФИЛЬТРАЦИЯ ПО ПРИОРИТЕТУ")
    print("1. 🔴 Высокий")
    print("2. 🟡 Средний")
    print("3. ⚪ Низкий")
    print("4. ↩️  Назад")
    
    choice = input("Выберите приоритет: ").strip()
    
    if choice == '1':
        filtered_tasks = self.manager.get_tasks_by_priority(Priority.HIGH)
        self.display_tasks(filtered_tasks)
    elif choice == '2':
        filtered_tasks = self.manager.get_tasks_by_priority(Priority.MEDIUM)
        self.display_tasks(filtered_tasks)
    elif choice == '3':
        filtered_tasks = self.manager.get_tasks_by_priority(Priority.LOW)
        self.display_tasks(filtered_tasks)
    elif choice == '4':
        return
    else:
        print("❌ Некорректный выбор")
```

---

## 💡 Рекомендация:

**Используйте улучшенную базовую версию:**

```python
def display_tasks(self, tasks=None):
    """Отображает список задач в удобном формате"""
    if tasks is None:
        tasks = self.manager.get_all_tasks()
    
    if not tasks:
        print("📭 Задачи не найдены")
        return
    
    print(f"\n📋 НАЙДЕНО ЗАДАЧ: {len(tasks)}")
    print("="*80)
    
    for i, task in enumerate(tasks, 1):
        # Emoji для статуса
        status_emoji = "🆕" if task.status == Status.NEW else "🔄" if task.status == Status.IN_PROGRESS else "✅"
        
        # Emoji для приоритета  
        priority_emoji = "🔴" if task.priority == Priority.HIGH else "🟡" if task.priority == Priority.MEDIUM else "⚪"
        
        print(f"#{i}")
        print(f"  ID: {task.id}")
        print(f"  📝 {task.title}")
        print(f"  📄 {task.description}")
        print(f"  {status_emoji} {task.status.value}")
        print(f"  {priority_emoji} {task.priority.value}")
        print(f"  📅 Создана: {task.created_date.strftime('%d.%m.%Y %H:%M')}")
        print(f"  🔄 Обновлена: {task.updated_date.strftime('%d.%m.%Y %H:%M')}")
        print("-" * 80)
```

Эта версия:
- ✅ Хорошо читается
- ✅ Использует интуитивные emoji
- ✅ Показывает всю необходимую информацию
- ✅ Форматирует даты удобно для пользователя
- ✅ Подсчитывает и показывает количество задач
