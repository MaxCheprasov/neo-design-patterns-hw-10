# Домашнє завдання — Тема 10. Поведінковий патерн Команда (Command)

Консольний TODO-застосунок із підтримкою `undo/redo` на основі патерну Command.

## Патерн Command

Кожна дія над задачами — окремий об'єкт-команда:

| Команда | Дія | Скасування (`undo`) |
|---|---|---|
| `AddTaskCommand` | Додає задачу до списку | Видаляє задачу |
| `RemoveTaskCommand` | Видаляє задачу | Повертає задачу |
| `UpdateTaskCommand` | Оновлює поля задачі | Відновлює попередні значення |
| `CompleteTaskCommand` | Змінює статус `completed` | Відновлює попередній статус |

**`CommandHistory`** виконує команди та управляє стеком `undo/redo`. **`TaskManager`** — фасад, що обгортає `TaskList` та `CommandHistory`.

## Структура проєкту

```
src/
├── commands/
│   ├── Command.ts              # Інтерфейс команди (execute, undo, redo)
│   ├── AbstractCommand.ts      # Базова реалізація (redo делегує execute)
│   ├── AddTaskCommand.ts
│   ├── RemoveTaskCommand.ts
│   ├── UpdateTaskCommand.ts
│   ├── CompleteTaskCommand.ts
│   └── CommandHistory.ts       # Стек undo/redo
├── models/
│   ├── Task.ts
│   └── TaskList.ts
├── services/
│   └── TaskManager.ts
└── main.ts
```

## Запуск

```bash
npm install
npx ts-node src/main.ts
```

## Приклад виводу

```
--- Після додавання задачі ---
[ { id: '...', completed: false, title: 'Завершити домашнє завдання', priority: 'high' } ]
--- Після оновлення задачі ---
[ { id: '...', completed: false, title: 'Завершити складне домашнє завдання', priority: 'medium' } ]
--- Після позначення як виконаної ---
[ { id: '...', completed: true, title: 'Завершити складне домашнє завдання', priority: 'medium' } ]
--- Після видалення задачі ---
[]
--- Після undo видалення ---
[ { id: '...', completed: true, ... } ]
--- Після undo додавання задачі ---
[]
--- Після redo додавання задачі ---
[ { id: '...', completed: false, title: 'Завершити домашнє завдання', priority: 'high' } ]
```
