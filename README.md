<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TO-DO App</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }

       .task-list {
            margin-bottom: 20px;
        }

       .task-list ul {
            list-style: none;
            padding: 0;
            margin: 0;
        }

       .task-list li {
            padding: 10px;
            border-bottom: 1px solid #ccc;
        }

       .task-list li:last-child {
            border-bottom: none;
        }

       .task-list li.completed {
            text-decoration: line-through;
            color: #666;
        }

       .add-task {
            margin-top: 20px;
        }

       .add-task form {
            display: flex;
            flex-direction: column;
            align-items: center;
        }

       .add-task input[type="text"] {
            width: 100%;
            padding: 10px;
            margin-bottom: 10px;
        }

       .add-task input[type="date"],.add-task input[type="time"] {
            width: 100%;
            padding: 10px;
            margin-bottom: 10px;
        }

       .add-task button[type="button"] {
            width: 100%;
            padding: 10px;
            background-color: #4CAF50;
            color: #fff;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

       .add-task button[type="button"]:hover {
            background-color: #3e8e41;
        }
    </style>
</head>
<body>
    <header>
        <h1>TO-DO App</h1>
    </header>
    <main>
        <section class="task-list">
            <h2>Tasks</h2>
            <ul id="task-list">
                <!-- tasks will be rendered here -->
            </ul>
        </section>
        <section class="add-task">
            <h2>Add Task</h2>
            <form id="add-task-form">
                <input type="text" id="task-input" placeholder="Enter task">
                <input type="date" id="task-date" placeholder="Select date">
                <input type="time" id="task-time" placeholder="Select time">
                <button id="add-task-btn">Add Task</button>
            </form>
        </section>
    </main>
    <script>
        // Task list array
        let tasks = [];

        // Render task list
        function renderTaskList() {
            const taskListElement = document.getElementById('task-list');
            taskListElement.innerHTML = '';
            tasks.forEach((task) => {
                const taskElement = document.createElement('li');
                taskElement.textContent = task.name;
                taskElement.dataset.taskId = task.id;
                if (task.completed) {
                    taskElement.classList.add('completed');
                }
                taskListElement.appendChild(taskElement);
            });
        }

        // Add task event listener
        document.getElementById('add-task-btn').addEventListener('click', (e) => {
            e.preventDefault();
            const taskInput = document.getElementById('task-input');
            const taskDate = document.getElementById('task-date');
            const taskTime = document.getElementById('task-time');
            const taskName = taskInput.value.trim();
            const taskDateValue = taskDate.value;
            const taskTimeValue = taskTime.value;
            if (taskName && taskDateValue && taskTimeValue) {
                const newTask = {
                    id: Date.now(),
                    name: taskName,
                    date: taskDateValue,
                    time: taskTimeValue,
                    completed: false,
                };
                tasks.push(newTask);
                taskInput.value = '';
                taskDate.value = '';
                taskTime.value = '';
                renderTaskList();
            }
        });

        // Edit task event listener
        document.getElementById('task-list').addEventListener('click', (e) => {
            if (e.target.tagName === 'LI') {
                const taskId = e.target.dataset.taskId;
                const task = tasks.find((task) => task.id === parseInt(taskId));
                if (task) {
                    const taskName = prompt('Edit task:', task.name);
                    if (taskName) {
                        task.name = taskName;
                        renderTaskList();
                    }
                }
            }
        });

        // Mark task as completed event listener
        document.getElementById('task-list').addEventListener('click', (e) => {
            if (e.target.tagName === 'LI') {
                const taskId = e.target.dataset.taskId;
                const task = tasks.find((task) => task
