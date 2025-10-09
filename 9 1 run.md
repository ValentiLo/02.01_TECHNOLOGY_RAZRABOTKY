## Место метода `run()` в проекте и интеграция с `main()`

### 📍 **Где должен находиться метод `run()`:**

```python
class TaskManagerUI:
    def __init__(self):
        self.manager = TaskManager()
    
    def display_menu(self):
        # ... ваш код меню ...
    
    def display_tasks(self, tasks=None):
        # ... ваш код отображения задач ...
    
    def add_task_ui(self):
        # ... ваш код добавления задачи ...
    
    def edit_task_ui(self):
        # ... ваш код редактирования ...
    
    def delete_task_ui(self):
        # ... ваш код удаления ...
    
    def filter_by_status_ui(self):
        # ... ваш код фильтрации по статусу ...
    
    def filter_by_priority_ui(self):
        # ... ваш код фильтрации по приоритету ...
    
    def run(self):
        """Главный цикл приложения"""
        print("🚀 Запуск системы управления задачами...")
        
        while True:
            self.display_menu()
            
            try:
                choice = input("Выберите действие (1-9): ").strip()
                
                if choice == '1':
                    self.display_tasks()
                elif choice == '2':
                    self.add_task_ui()
                elif choice == '3':
                    self.edit_task_ui()
                elif choice == '4':
                    self.delete_task_ui()
                elif choice == '5':
                    self.filter_by_status_ui()
                elif choice == '6':
                    self.filter_by_priority_ui()
                elif choice == '7':
                    if self.manager.save_tasks():
                        print("✅ Задачи успешно сохранены!")
                    else:
                        print("❌ Ошибка сохранения задач!")
                elif choice == '8':
                    self.show_statistics()
                elif choice == '9':
                    print("\n✅ Данные автоматически сохранены.")
                    print("👋 До свидания!")
                    break
                else:
                    print("❌ Некорректный выбор! Попробуйте снова.")
                    
            except KeyboardInterrupt:
                print("\n\n⚠️  Программа прервана пользователем")
                # Автосохранение при прерывании
                self.manager.save_tasks()
                break
            except Exception as e:
                print(f"❌ Произошла ошибка: {e}")
```

---

## 🔄 **Интеграция с функцией `main()`:**

### **Вариант 1: Простая интеграция**
```python
def main():
    """Точка входа в приложение"""
    try:
        # Создаем UI и запускаем главный цикл
        app = TaskManagerUI()
        app.run()
    except Exception as e:
        print(f"💥 Критическая ошибка при запуске: {e}")
        input("Нажмите Enter для выхода...")

if __name__ == "__main__":
    main()
```

### **Вариант 2: Расширенная версия с настройками**
```python
def main():
    """Точка входа в приложение с дополнительными настройками"""
    print("="*50)
    print("    СИСТЕМА УПРАВЛЕНИЯ ЗАДАЧАМИ")
    print("="*50)
    
    try:
        # Можно добавить выбор файла базы данных
        filename = input("Введите имя файла для данных (или Enter для tasks.json): ").strip()
        if not filename:
            filename = 'tasks.json'
        
        # Создаем приложение
        app = TaskManagerUI(filename)
        
        # Запускаем главный цикл
        app.run()
        
    except KeyboardInterrupt:
        print("\n\n👋 Программа завершена пользователем")
    except Exception as e:
        print(f"\n💥 Критическая ошибка: {e}")
        print("Пожалуйста, проверьте настройки и попробуйте снова.")
    finally:
        print("\n✅ Работа программы завершена.")

if __name__ == "__main__":
    main()
```

### **Вариант 3: С обновленным классом UI**
```python
class TaskManagerUI:
    def __init__(self, filename='tasks.json'):
        self.manager = TaskManager(filename)
    
    # ... все остальные методы UI ...
    
    def show_statistics(self):
        """Показывает статистику по задачам"""
        tasks = self.manager.get_all_tasks()
        if not tasks:
            print("📊 Нет задач для статистики")
            return
        
        total = len(tasks)
        completed = len([t for t in tasks if t.status == Status.COMPLETED])
        in_progress = len([t for t in tasks if t.status == Status.IN_PROGRESS])
        new_tasks = len([t for t in tasks if t.status == Status.NEW])
        
        high_priority = len([t for t in tasks if t.priority == Priority.HIGH])
        
        print("\n" + "="*40)
        print("📊 СТАТИСТИКА ЗАДАЧ")
        print("="*40)
        print(f"📈 Всего задач: {total}")
        print(f"✅ Завершено: {completed} ({completed/total*100:.1f}%)")
        print(f"🔄 В работе: {in_progress} ({in_progress/total*100:.1f}%)")
        print(f"🆕 Новых: {new_tasks} ({new_tasks/total*100:.1f}%)")
        print(f"⚡ Высокий приоритет: {high_priority}")
        print("="*40)
    
    def run(self):
        """Главный цикл приложения"""
        print("🚀 Добро пожаловать в систему управления задачами!")
        
        while True:
            self.display_menu()
            choice = input("\n🎯 Выберите действие (1-9): ").strip()
            
            # Обработка выбора
            if choice == '1':
                self.display_tasks()
            elif choice == '2':
                self.add_task_ui()
            elif choice == '3':
                self.edit_task_ui()
            elif choice == '4':
                self.delete_task_ui()
            elif choice == '5':
                self.filter_by_status_ui()
            elif choice == '6':
                self.filter_by_priority_ui()
            elif choice == '7':
                self.manager.save_tasks()
            elif choice == '8':
                self.show_statistics()
            elif choice == '9':
                # Подтверждение выхода
                confirm = input("❓ Вы уверены, что хотите выйти? (y/n): ").strip().lower()
                if confirm in ['y', 'yes', 'д', 'да']:
                    print("👋 До свидания! Ваши данные сохранены.")
                    break
                else:
                    print("✅ Продолжаем работу...")
            else:
                print("❌ Некорректный выбор! Попробуйте снова.")
            
            # Пауза для удобства чтения
            input("\n⏎ Нажмите Enter для продолжения...")
```

---

## 🏗️ **Полная структура файла task_manager.py:**

```python
# Импорты
from datetime import datetime
from enum import Enum
import json

# Перечисления
class Status(Enum):
    NEW = "новая"
    IN_PROGRESS = "в работе"
    COMPLETED = "завершена"

class Priority(Enum):
    LOW = "низкий"
    MEDIUM = "средний"
    HIGH = "высокий"

# Класс Task
class Task:
    def __init__(self, id, title, description, status=Status.NEW, priority=Priority.MEDIUM):
        self.id = id
        self.title = title
        self.description = description
        self.status = status
        self.priority = priority
        self.created_date = datetime.now()
        self.updated_date = datetime.now()
    
    def to_dict(self):
        return {
            'id': self.id,
            'title': self.title,
            'description': self.description,
            'status': self.status.value,
            'priority': self.priority.value,
            'created_date': self.created_date.isoformat(),
            'updated_date': self.updated_date.isoformat()
        }
    
    @classmethod
    def from_dict(cls, data):
        task = cls(
            id=data['id'],
            title=data['title'],
            description=data['description']
        )
        task.status = Status(data['status'])
        task.priority = Priority(data['priority'])
        task.created_date = datetime.fromisoformat(data['created_date'])
        task.updated_date = datetime.fromisoformat(data['updated_date'])
        return task

# Класс TaskManager
class TaskManager:
    def __init__(self, filename='tasks.json'):
        self.filename = filename
        self.tasks = []
        self.next_id = 1
        self.load_tasks()
    
    def load_tasks(self):
        # ... ваш код загрузки ...
    
    def save_tasks(self):
        # ... ваш код сохранения ...
    
    def add_task(self, title, description, priority=Priority.MEDIUM):
        # ... ваш код добавления ...
    
    # ... другие методы TaskManager ...

# Класс TaskManagerUI
class TaskManagerUI:
    def __init__(self, filename='tasks.json'):
        self.manager = TaskManager(filename)
    
    def display_menu(self):
        # ... ваш код меню ...
    
    def display_tasks(self, tasks=None):
        # ... ваш код отображения задач ...
    
    def add_task_ui(self):
        # ... ваш код UI добавления ...
    
    def edit_task_ui(self):
        # ... ваш код UI редактирования ...
    
    def delete_task_ui(self):
        # ... ваш код UI удаления ...
    
    def filter_by_status_ui(self):
        # ... ваш код UI фильтрации по статусу ...
    
    def filter_by_priority_ui(self):
        # ... ваш код UI фильтрации по приоритету ...
    
    def show_statistics(self):
        # ... ваш код статистики ...
    
    def run(self):
        """ГЛАВНЫЙ ЦИКЛ ПРИЛОЖЕНИЯ"""
        print("🚀 Добро пожаловать в систему управления задачами!")
        
        while True:
            self.display_menu()
            choice = input("\n🎯 Выберите действие (1-9): ").strip()
            
            # Обработка команд
            if choice == '1':
                self.display_tasks()
            elif choice == '2':
                self.add_task_ui()
            elif choice == '3':
                self.edit_task_ui()
            elif choice == '4':
                self.delete_task_ui()
            elif choice == '5':
                self.filter_by_status_ui()
            elif choice == '6':
                self.filter_by_priority_ui()
            elif choice == '7':
                self.manager.save_tasks()
            elif choice == '8':
                self.show_statistics()
            elif choice == '9':
                print("👋 До свидания!")
                break
            else:
                print("❌ Некорректный выбор!")
            
            input("\n⏎ Нажмите Enter для продолжения...")

# Главная функция
def main():
    """Точка входа в программу"""
    try:
        app = TaskManagerUI()
        app.run()
    except Exception as e:
        print(f"💥 Ошибка: {e}")

if __name__ == "__main__":
    main()
```

---

## 💡 **Рекомендация:**

**Используйте простую интеграцию:**

```python
def main():
    """Точка входа в приложение"""
    try:
        app = TaskManagerUI()
        app.run()
    except Exception as e:
        print(f"Критическая ошибка: {e}")

if __name__ == "__main__":
    main()
```

**И добавьте метод `run()` в класс `TaskManagerUI`:**

```python
def run(self):
    """Главный цикл приложения"""
    while True:
        self.display_menu()
        choice = input("Выберите действие (1-9): ").strip()
        
        # Обработка выбора
        if choice == '1':
            self.display_tasks()
        elif choice == '2':
            self.add_task_ui()
        # ... остальные пункты меню ...
        elif choice == '9':
            break
```

Такой подход:
- ✅ Четко разделяет ответственность
- ✅ `main()` только запускает приложение
- ✅ `run()` управляет главным циклом
- ✅ Легко расширять и модифицировать
