# TO-DO-LIST

## INDEX.HTML
```C
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>To-Do List</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <h1>My To-Do List</h1>
        <form id="new-task-form">
            <input type="text" id="task-input" placeholder="Add a new task...">
            <button type="submit">Add</button>
        </form>

        <ul id="task-list"></ul>
    </div>

    <script src="script.js"></script>
</body>
</html>
```
## STYLE.CSS
```C
body {
    font-family: sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    background-color: #f0f0f0;
}

.container {
    background-color: #fff;
    padding: 20px;
    border-radius: 5px;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

h1 {
    text-align: center;
    margin-bottom: 20px;
}

#new-task-form {
    display: flex;
}

#task-input {
    flex-grow: 1;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 3px;
    margin-right: 10px;
}

#new-task-form button {
    padding: 10px 15px;
    background-color: #4CAF50;
    color: white;
    border: none;
    border-radius: 3px;
    cursor: pointer;
}

#task-list {
    list-style: none;
    padding: 0;
    margin-top: 20px;
}

#task-list li {
    background-color: #f5f5f5;
    padding: 10px;
    border-radius: 3px;
    margin-bottom: 5px;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

#task-list li input[type="checkbox"] {
    margin-right: 10px;
}

#task-list li button {
    background-color: transparent;
    border: none;
    cursor: pointer;
    font-size: 16px;
}
```
## SCRIPT.JS
```C
const taskInput = document.getElementById('task-input');
const newTaskForm = document.getElementById('new-task-form');
const taskList = document.getElementById('task-list');

// Task Class
class Task {
    constructor(title) {
        this.title = title;
        this.completed = false;
    }
}

// ToDoList Class
class ToDoList {
    constructor() {
        this.tasks = [];
        this.loadTasks(); // Load tasks from local storage on initialization
    }

    addTask(task) {
        this.tasks.push(task);
        this.saveTasks(); // Save the updated tasks to local storage
        this.renderTasks(); // Render the new task on the UI
    }

    deleteTask(index) {
        this.tasks.splice(index, 1);
        this.saveTasks();
        this.renderTasks();
    }

    toggleTaskCompletion(index) {
        this.tasks[index].completed = !this.tasks[index].completed;
        this.saveTasks();
        this.renderTasks();
    }

    renderTasks() {
        taskList.innerHTML = ''; // Clear the task list

        this.tasks.forEach((task, index) => {
            const li = document.createElement('li');
            li.innerHTML = `
                <input type="checkbox" ${task.completed ? 'checked' : ''} data-index="${index}">
                <span>${task.title}</span>
                <button data-index="${index}">X</button>
            `;
            taskList.appendChild(li);

            // Add event listeners to each task item
            li.querySelector('input[type="checkbox"]').addEventListener('change', () => {
                this.toggleTaskCompletion(index);
            });

            li.querySelector('button').addEventListener('click', () => {
                this.deleteTask(index);
            });
        });
    }

    // Load tasks from local storage
    loadTasks() {
        const storedTasks = JSON.parse(localStorage.getItem('tasks'));
        if (storedTasks) {
            this.tasks = storedTasks;
        }
    }

    // Save tasks to local storage
    saveTasks() {
        localStorage.setItem('tasks', JSON.stringify(this.tasks));
    }
}

const toDoList = new ToDoList();

// Handle form submission
newTaskForm.addEventListener('submit', (event) => {
    event.preventDefault();
    const taskTitle = taskInput.value;
    if (taskTitle) {
        toDoList.addTask(new Task(taskTitle));
        taskInput.value = ''; // Clear the input field
    }
});

toDoList.renderTasks(); // Render initial tasks on page load
```
# OUTPUT
![Screenshot (21)](https://github.com/user-attachments/assets/ea77efcb-f5e0-4f75-8c24-dd2e1de81d01)


