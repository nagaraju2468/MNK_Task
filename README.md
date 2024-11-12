import json
import os

class TaskManager:
    def __init__(self, filename='tasks.json'):
        self.filename = filename
        self.tasks = self.load_tasks()

    def load_tasks(self):
        if os.path.exists(self.filename):
            with open(self.filename, 'r') as f:
                return json.load(f)
        return []

    def save_tasks(self):
        with open(self.filename, 'w') as f:
            json.dump(self.tasks, f)

    def add_task(self, task):
        self.tasks.append({'task': task, 'completed': False})
        self.save_tasks()

    def view_tasks(self):
        print("Tasks:")
        for i, task in enumerate(self.tasks):
            status = "Completed" if task['completed'] else "Pending"
            print(f"{i+1}. {task['task']} - {status}")
    def delete_task(self, index):
        try:
            del self.tasks[index-1]
            self.save_tasks()
        except IndexError:
            print("Invalid task index.")

    def complete_task(self, index):
        try:
            self.tasks[index-1]['completed'] = True
            self.save_tasks()
        except IndexError:
            print("Invalid task index.")

def main():
    task_manager = TaskManager()

    while True:
        print("\nTask Manager")
        print("1. Add Task")
        print("2. View Tasks")
        print("3. Delete Task")
        print("4. Complete Task")
        print("5. Quit")

        choice = input("Choose an option: ")

        if choice == "1":
            task = input("Enter task: ")
            task_manager.add_task(task)
        elif choice == "2":
            task_manager.view_tasks()
        elif choice == "3":
            index = int(input("Enter task index to delete: "))
            task_manager.delete_task(index)
        elif choice == "4":
            index = int(input("Enter task index to complete: "))
            task_manager.complete_task(index)
        elif choice == "5":
            break
        else:
            print("Invalid option. Please choose again.")

if __name__ == "__main__":
    main()
