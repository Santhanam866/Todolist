# Ex03 To-Do List using JavaScript
## Date: 24-08-2026

## AIM
To create a To-do Application with all features using JavaScript.

## ALGORITHM
### STEP 1
Build the HTML structure (index.html).

### STEP 2
Style the App (style.css).

### STEP 3
Plan the features the To-Do App should have.

### STEP 4
Create a To-do application using Javascript.

### STEP 5
Add functionalities.

### STEP 6
Test the App.

### STEP 7
Open the HTML file in a browser to check layout and functionality.

### STEP 8
Fix styling issues and refine content placement.

### STEP 9
Deploy the website.

### STEP 10
Upload to GitHub Pages for free hosting.

## PROGRAM

#### HTML

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Todo Application</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

    <div class="todo-container">

        <h1>📝 My Todo List</h1>

        <div class="input-area">
            <input type="text" id="taskInput" placeholder="Enter your task...">
            <button onclick="addTask()">Add</button>
        </div>

        <div class="controls">
            <input type="text" id="searchInput"
                   placeholder="Search task..."
                   onkeyup="displayTasks()">

            <select id="filter" onchange="displayTasks()">
                <option value="all">All Tasks</option>
                <option value="active">Active</option>
                <option value="completed">Completed</option>
            </select>
        </div>

        <ul id="taskList"></ul>

        <div class="stats">
            <p id="taskCount">0 Active Tasks</p>
        </div>

        <footer>
            Name: <b>Gerius G</b> |
            Register No: <b>21224040090</b>
        </footer>

    </div>

    <script src="script.js"></script>
</body>
</html>
```

#### CSS

```css
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
    font-family: Arial, sans-serif;
}

body {
    min-height: 100vh;
    background: linear-gradient(135deg, #0f172a, #1e3a8a);
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 20px;
}

.todo-container {
    width: 100%;
    max-width: 600px;
    background: white;
    padding: 30px;
    border-radius: 20px;
    box-shadow: 0 15px 40px rgba(0, 0, 0, 0.3);
}

h1 {
    text-align: center;
    margin-bottom: 20px;
    color: #1e293b;
}

.input-area {
    display: flex;
    gap: 10px;
    margin-bottom: 20px;
}

#taskInput {
    flex: 1;
    padding: 13px;
    border: 2px solid #ddd;
    border-radius: 10px;
    outline: none;
}

button {
    border: none;
    padding: 12px 16px;
    border-radius: 10px;
    cursor: pointer;
    color: white;
    background: #2563eb;
}

button:hover {
    opacity: 0.85;
}

.controls {
    display: flex;
    gap: 10px;
    margin-bottom: 20px;
}

.controls input,
.controls select {
    flex: 1;
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 8px;
}

ul {
    list-style: none;
}

li {
    display: flex;
    align-items: center;
    gap: 10px;
    padding: 14px;
    margin-bottom: 10px;
    background: #f1f5f9;
    border-radius: 10px;
}

li span {
    flex: 1;
}

li.completed span {
    text-decoration: line-through;
    color: #888;
}

.actions button {
    padding: 7px 10px;
    margin-left: 4px;
}

.edit {
    background: #f59e0b;
}

.delete {
    background: #ef4444;
}

.stats {
    margin-top: 20px;
    text-align: center;
    color: #475569;
}

footer {
    text-align: center;
    margin-top: 25px;
    padding-top: 15px;
    border-top: 1px solid #ddd;
    color: #64748b;
    font-size: 14px;
}
```

#### Js

```js
let tasks = JSON.parse(localStorage.getItem("tasks")) || [];

function saveTasks() {
    localStorage.setItem("tasks", JSON.stringify(tasks));
}

function addTask() {

    const input = document.getElementById("taskInput");
    const text = input.value.trim();

    if (text === "") {
        alert("Please enter a task!");
        return;
    }

    tasks.push({
        id: Date.now(),
        text: text,
        completed: false
    });

    input.value = "";

    saveTasks();
    displayTasks();
}

function toggleTask(id) {

    tasks = tasks.map(task =>
        task.id === id
            ? { ...task, completed: !task.completed }
            : task
    );

    saveTasks();
    displayTasks();
}

function editTask(id) {

    const task = tasks.find(task => task.id === id);

    const newText = prompt(
        "Edit your task:",
        task.text
    );

    if (newText !== null && newText.trim() !== "") {

        task.text = newText.trim();

        saveTasks();
        displayTasks();
    }
}

function deleteTask(id) {

    if (confirm("Are you sure you want to delete this task?")) {

        tasks = tasks.filter(
            task => task.id !== id
        );

        saveTasks();
        displayTasks();
    }
}

function displayTasks() {

    const list = document.getElementById("taskList");

    const search = document
        .getElementById("searchInput")
        .value
        .toLowerCase();

    const filter = document
        .getElementById("filter")
        .value;

    list.innerHTML = "";

    let filteredTasks = tasks.filter(task => {

        const matchesSearch =
            task.text.toLowerCase().includes(search);

        const matchesFilter =
            filter === "all" ||
            (filter === "active" && !task.completed) ||
            (filter === "completed" && task.completed);

        return matchesSearch && matchesFilter;
    });

    filteredTasks.forEach(task => {

        const li = document.createElement("li");

        if (task.completed) {
            li.classList.add("completed");
        }

        li.innerHTML = `
            <input type="checkbox"
                ${task.completed ? "checked" : ""}
                onchange="toggleTask(${task.id})">

            <span>${task.text}</span>

            <div class="actions">

                <button class="edit"
                    onclick="editTask(${task.id})">
                    Edit
                </button>

                <button class="delete"
                    onclick="deleteTask(${task.id})">
                    Delete
                </button>

            </div>
        `;

        list.appendChild(li);
    });

    const remaining =
        tasks.filter(task => !task.completed).length;

    document.getElementById("taskCount").innerText =
        `${remaining} Active Task(s)`;
}

document.getElementById("taskInput")
    .addEventListener("keypress", function(event) {

        if (event.key === "Enter") {
            addTask();
        }
    });

displayTasks();
```

## OUTPUT

![alt text](image.png)

## RESULT
The program for creating To-do list using JavaScript is executed successfully.
