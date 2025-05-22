import json
import os
import sys

TODO_FILE = 'todo.json'

def load_tasks():
    if not os.path.exists(TODO_FILE):
        return []
    with open(TODO_FILE, 'r') as f:
        try:
            return json.load(f)
        except json.JSONDecodeError:
            return []

def save_tasks(tasks):
    with open(TODO_FILE, 'w') as f:
        json.dump(tasks, f, indent=2)

def list_tasks():
    tasks = load_tasks()
    if not tasks:
        print("No tasks found.")
        return
    print("ID  Done  Description")
    print("--  ----  -----------")
    for task in tasks:
        status = "Yes" if task["done"] else "No"
        print(f"{task['id']: <3} {status: <4}  {task['description']}")

def add_task(description):
    tasks = load_tasks()
    new_id = max([t['id'] for t in tasks], default=0) + 1
    tasks.append({
        "id": new_id,
        "description": description,
        "done": False
    })
    save_tasks(tasks)
    print(f"Added task {new_id}: {description}")

def mark_done(task_id):
    tasks = load_tasks()
    for task in tasks:
        if task['id'] == task_id:
            if task["done"]:
                print(f"Task {task_id} is already marked done.")
            else:
                task["done"] = True
                save_tasks(tasks)
                print(f"Marked task {task_id} as done.")
            return
    print(f"Task {task_id} not found.")

def delete_task(task_id):
    tasks = load_tasks()
    new_tasks = [t for t in tasks if t['id'] != task_id]
    if len(new_tasks) == len(tasks):
        print(f"Task {task_id} not found.")
        return
    save_tasks(new_tasks)
    print(f"Deleted task {task_id}.")

def print_help():
    help_text = """
Simple To-Do List CLI Application

Usage:
  python todo.py add "Task description"   - Add a new task
  python todo.py done TASK_ID             - Mark task as done
  python todo.py delete TASK_ID           - Delete task
  python todo.py list                     - List all tasks
  python todo.py help                     - Show this help message
"""
    print(help_text.strip())

def main():
    if len(sys.argv) < 2:
        print_help()
        return
    
    command = sys.argv[1].lower()

    if command == 'add':
        if len(sys.argv) < 3:
            print("Error: Missing task description.")
            return
        description = " ".join(sys.argv[2:]).strip('"\'')
        add_task(description)

    elif command == 'done':
        if len(sys.argv) != 3 or not sys.argv[2].isdigit():
            print("Error: Please provide a valid task ID to mark done.")
            return
        task_id = int(sys.argv[2])
        mark_done(task_id)

    elif command == 'delete':
        if len(sys.argv) != 3 or not sys.argv[2].isdigit():
            print("Error: Please provide a valid task ID to delete.")
            return
        task_id = int(sys.argv[2])
        delete_task(task_id)

    elif command == 'list':
        list_tasks()

    elif command == 'help':
        print_help()

    else:
        print(f"Unknown command: {command}")
        print_help()

if _name_ == '_main_':
    main()

    
    output
   Simple To-Do List CLI Application

Usage:
  python todo.py add "Task description"   - Add a new task        
  python todo.py done TASK_ID             - Mark task as done     
  python todo.py delete TASK_ID           - Delete task
  python todo.py list                     - List all tasks        
  python todo.py help                     - Show this help message 
