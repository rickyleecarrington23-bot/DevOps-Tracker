# DevOps-Tracker
study-tracker/
  index.html
  style.css
  app.js

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Study Tracker</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div class="app">
    <header class="app-header">
      <h1>Study Tracker</h1>
      <p>Track your learning tasks, stay organised, and show this off in your portfolio.</p>
    </header>

    <main class="app-main">
      <!-- Create / Edit Task -->
      <section class="card card-create">
        <h2 id="formTitle">Add new task</h2>
        <form id="taskForm">
          <div class="form-row">
            <label for="title">Title<span class="required">*</span></label>
            <input type="text" id="title" name="title" required placeholder="e.g. Azure DevOps module 4" />
          </div>

          <div class="form-row">
            <label for="description">Description</label>
            <textarea id="description" name="description" rows="3" placeholder="Short notes about this task..."></textarea>
          </div>

          <div class="form-row form-row-inline">
            <div class="form-group">
              <label for="category">Category</label>
              <select id="category" name="category">
                <option value="General">General</option>
                <option value="DevOps">DevOps</option>
                <option value="Cloud">Cloud</option>
                <option value="Security">Security</option>
                <option value="Frontend">Frontend</option>
                <option value="Backend">Backend</option>
              </select>
            </div>

            <div class="form-group">
              <label for="priority">Priority</label>
              <select id="priority" name="priority">
                <option value="low">Low</option>
                <option value="medium" selected>Medium</option>
                <option value="high">High</option>
              </select>
            </div>

            <div class="form-group">
              <label for="dueDate">Due date</label>
              <input type="date" id="dueDate" name="dueDate" />
            </div>
          </div>

          <div class="form-actions">
            <button type="submit" id="submitBtn">Save task</button>
            <button type="button" id="cancelEditBtn" class="btn-secondary hidden">Cancel edit</button>
          </div>
        </form>
      </section>

      <!-- Filters & Sorting -->
      <section class="card card-controls">
        <h2>Filters & sorting</h2>
        <div class="controls-grid">
          <div class="control-group">
            <label for="filterCategory">Category</label>
            <select id="filterCategory">
              <option value="all">All</option>
              <option value="DevOps">DevOps</option>
              <option value="Cloud">Cloud</option>
              <option value="Security">Security</option>
              <option value="Frontend">Frontend</option>
              <option value="Backend">Backend</option>
              <option value="General">General</option>
            </select>
          </div>

          <div class="control-group">
            <label for="filterStatus">Status</label>
            <select id="filterStatus">
              <option value="all">All</option>
              <option value="pending">Pending</option>
              <option value="done">Done</option>
            </select>
          </div>

          <div class="control-group">
            <label for="sortBy">Sort by</label>
            <select id="sortBy">
              <option value="createdDesc">Newest first</option>
              <option value="createdAsc">Oldest first</option>
              <option value="dueAsc">Closest due date</option>
              <option value="priorityDesc">Highest priority</option>
            </select>
          </div>
        </div>
      </section>

      <!-- Task list -->
      <section class="card card-list">
        <div class="list-header">
          <h2>Tasks</h2>
          <span id="taskCount" class="task-count">0 tasks</span>
        </div>
        <div id="taskList" class="task-list">
          <!-- Tasks will be rendered here by JS -->
        </div>
      </section>
    </main>

    <footer class="app-footer">
      <small>
        Built with HTML, CSS &amp; JavaScript. Customize this for your portfolio.
      </small>
    </footer>
  </div>

  <!-- Template for task items -->
  <template id="taskTemplate">
    <article class="task-item">
      <div class="task-main">
        <button class="status-toggle" title="Toggle done"></button>
        <div class="task-info">
          <div class="task-title-row">
            <h3 class="task-title"></h3>
            <span class="task-category"></span>
          </div>
          <p class="task-description"></p>
          <div class="task-meta">
            <span class="task-priority"></span>
            <span class="task-due"></span>
          </div>
        </div>
      </div>
      <div class="task-actions">
        <button class="btn-small btn-edit">Edit</button>
        <button class="btn-small btn-danger btn-delete">Delete</button>
      </div>
    </article>
  </template>

  <script src="app.js"></script>
</body>
</html>
:root {
  --bg: #0f172a;
  --bg-alt: #111827;
  --card-bg: #1f2937;
  --accent: #38bdf8;
  --accent-soft: rgba(56, 189, 248, 0.15);
  --accent-strong: #0ea5e9;
  --text: #e5e7eb;
  --text-muted: #9ca3af;
  --danger: #f97373;
  --border-soft: rgba(148, 163, 184, 0.3);
  --radius-lg: 16px;
  --radius-sm: 6px;
  --shadow-soft: 0 18px 45px rgba(15, 23, 42, 0.5);
  --transition-fast: 0.15s ease;
  --font-main: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
}

*,
*::before,
*::after {
  box-sizing: border-box;
}

body {
  margin: 0;
  min-height: 100vh;
  font-family: var(--font-main);
  background: radial-gradient(circle at top, #1e293b, #020617);
  color: var(--text);
  display: flex;
  justify-content: center;
  padding: 24px;
}

.app {
  width: 100%;
  max-width: 1100px;
  background: linear-gradient(145deg, rgba(15, 23, 42, 0.95), rgba(15, 23, 42, 0.9));
  border-radius: 24px;
  box-shadow: var(--shadow-soft);
  padding: 24px 20px 20px;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

/* Header */
.app-header {
  padding: 4px 8px 12px;
  border-bottom: 1px solid var(--border-soft);
}

.app-header h1 {
  margin: 0 0 6px;
  font-size: 1.8rem;
  letter-spacing: 0.04em;
}

.app-header p {
  margin: 0;
  color: var(--text-muted);
  font-size: 0.9rem;
}

/* Layout */
.app-main {
  display: grid;
  grid-template-columns: 1.1fr 0.9fr;
  grid-template-rows: auto auto;
  gap: 16px;
}

.card {
  background: radial-gradient(circle at top left, rgba(15, 118, 255, 0.12), transparent 55%),
    radial-gradient(circle at bottom right, rgba(52, 211, 153, 0.04), transparent 55%),
    var(--card-bg);
  border-radius: var(--radius-lg);
  padding: 16px 18px 18px;
  border: 1px solid var(--border-soft);
}

/* Make layout responsive */
@media (max-width: 840px) {
  .app {
    padding: 16px;
  }

  .app-main {
    grid-template-columns: 1fr;
  }
}

/* Create card spans two columns on desktop */
.card-create {
  grid-column: 1 / -1;
}

/* Form styles */
.card-create h2,
.card-controls h2,
.card-list h2 {
  margin: 0 0 10px;
  font-size: 1.1rem;
}

.form-row {
  margin-bottom: 10px;
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.form-row-inline {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 10px;
}

@media (max-width: 720px) {
  .form-row-inline {
    grid-template-columns: 1fr;
  }
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

label {
  font-size: 0.85rem;
  color: var(--text-muted);
}

.required {
  color: var(--accent);
  margin-left: 4px;
}

input[type="text"],
input[type="date"],
textarea,
select {
  background: var(--bg);
  border-radius: var(--radius-sm);
  border: 1px solid var(--border-soft);
  padding: 8px 9px;
  color: var(--text);
  font: inherit;
  outline: none;
  transition: border-color var(--transition-fast), box-shadow var(--transition-fast),
    background-color var(--transition-fast), transform var(--transition-fast);
}

input::placeholder,
textarea::placeholder {
  color: #6b7280;
}

textarea {
  resize: vertical;
  min-height: 70px;
}

input:focus,
textarea:focus,
select:focus {
  border-color: var(--accent-strong);
  box-shadow: 0 0 0 1px rgba(56, 189, 248, 0.3);
  background: #020617;
  transform: translateY(-0.5px);
}

/* Buttons */
button {
  font: inherit;
  cursor: pointer;
  border: none;
}

.form-actions {
  margin-top: 4px;
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
}

#submitBtn {
  padding: 8px 16px;
  border-radius: 999px;
  background: linear-gradient(135deg, var(--accent), var(--accent-strong));
  color: #0b1120;
  font-weight: 600;
  letter-spacing: 0.02em;
  border: 1px solid rgba(56, 189, 248, 0.4);
  box-shadow: 0 10px 30px rgba(56, 189, 248, 0.35);
  transition: transform var(--transition-fast), box-shadow var(--transition-fast),
    filter var(--transition-fast);
}

#submitBtn:hover {
  transform: translateY(-1px);
  filter: brightness(1.05);
  box-shadow: 0 16px 40px rgba(56, 189, 248, 0.45);
}

#submitBtn:active {
  transform: translateY(0);
  box-shadow: 0 8px 22px rgba(56, 189, 248, 0.35);
}

.btn-secondary {
  padding: 8px 14px;
  border-radius: 999px;
  background: rgba(15, 23, 42, 0.8);
  border: 1px solid var(--border-soft);
  color: var(--text-muted);
  font-size: 0.85rem;
  transition: background-color var(--transition-fast), color var(--transition-fast),
    border-color var(--transition-fast), transform var(--transition-fast);
}

.btn-secondary:hover {
  background: #020617;
  color: var(--text);
  border-color: rgba(148, 163, 184, 0.7);
  transform: translateY(-0.5px);
}

.hidden {
  display: none !important;
}

/* Controls */
.card-controls {
  align-self: start;
}

.controls-grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 10px;
}

@media (max-width: 720px) {
  .controls-grid {
    grid-template-columns: 1fr;
  }
}

.control-group {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

/* Task list */
.card-list {
  grid-column: 1 / -1;
}

.list-header {
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  gap: 8px;
  margin-bottom: 10px;
}

.task-count {
  font-size: 0.8rem;
  color: var(--text-muted);
}

.task-list {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

/* Task item */
.task-item {
  display: flex;
  justify-content: space-between;
  align-items: stretch;
  gap: 10px;
  padding: 10px 12px;
  background: rgba(15, 23, 42, 0.9);
  border-radius: 14px;
  border: 1px solid rgba(148, 163, 184, 0.45);
  transition: border-color var(--transition-fast), background-color var(--transition-fast),
    transform var(--transition-fast), box-shadow var(--transition-fast);
}

.task-item:hover {
  transform: translateY(-1px);
  border-color: var(--accent);
  box-shadow: 0 8px 22px rgba(15, 23, 42, 0.7);
}

.task-main {
  display: flex;
  align-items: flex-start;
  gap: 10px;
  flex: 1;
}

.status-toggle {
  width: 22px;
  height: 22px;
  border-radius: 999px;
  border: 2px solid var(--text-muted);
  background: transparent;
  flex-shrink: 0;
  margin-top: 2px;
  position: relative;
  transition: border-color var(--transition-fast), background-color var(--transition-fast),
    box-shadow var(--transition-fast), transform var(--transition-fast);
}

.status-toggle.done {
  background: var(--accent-strong);
  border-color: var(--accent-strong);
  box-shadow: 0 0 0 3px var(--accent-soft);
}

.status-toggle.done::after {
  content: "✓";
  color: #0b1120;
  font-size: 0.9rem;
  font-weight: 700;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -54%);
}

.status-toggle:hover {
  transform: scale(1.04);
  border-color: var(--accent);
}

.task-info {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.task-title-row {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  align-items: baseline;
}

.task-title {
  margin: 0;
  font-size: 1rem;
}

.task-category {
  font-size: 0.75rem;
  padding: 2px 8px;
  border-radius: 999px;
  background: rgba(15, 118, 255, 0.22);
  color: #bfdbfe;
  border: 1px solid rgba(96, 165, 250, 0.4);
}

.task-description {
  margin: 0;
  font-size: 0.85rem;
  color: var(--text-muted);
}

.task-meta {
  margin-top: 4px;
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  font-size: 0.75rem;
  color: var(--text-muted);
}

.task-priority {
  padding: 2px 8px;
  border-radius: 999px;
  border: 1px solid rgba(148, 163, 184, 0.7);
}

.task-priority.priority-high {
  border-color: #fb7185;
  color: #fecaca;
  background: rgba(248, 113, 113, 0.12);
}

.task-priority.priority-medium {
  border-color: #facc15;
  color: #fef9c3;
  background: rgba(234, 179, 8, 0.12);
}

.task-priority.priority-low {
  border-color: #6ee7b7;
  color: #d1fae5;
  background: rgba(52, 211, 153, 0.12);
}

.task-due {
  opacity: 0.9;
}

/* Actions */
.task-actions {
  display: flex;
  flex-direction: column;
  gap: 6px;
  align-items: flex-end;
}

.btn-small {
  font-size: 0.75rem;
  padding: 4px 9px;
  border-radius: 999px;
  background: rgba(15, 23, 42, 0.9);
  border: 1px solid var(--border-soft);
  color: var(--text-muted);
  transition: background-color var(--transition-fast), color var(--transition-fast),
    border-color var(--transition-fast), transform var(--transition-fast);
}

.btn-small:hover {
  background: #020617;
  color: var(--text);
  border-color: rgba(148, 163, 184, 0.7);
  transform: translateY(-0.5px);
}

.btn-danger {
  border-color: rgba(248, 113, 113, 0.5);
  color: #fecaca;
}

.btn-danger:hover {
  background: rgba(248, 113, 113, 0.16);
  border-color: rgba(248, 113, 113, 0.8);
}

/* Completed state */
.task-item.completed .task-title {
  text-decoration: line-through;
  color: var(--text-muted);
}

.task-item.completed {
  opacity: 0.9;
  background: radial-gradient(circle at left, rgba(22, 163, 74, 0.1), transparent 55%),
    rgba(15, 23, 42, 0.95);
}

/* Footer */
.app-footer {
  border-top: 1px solid var(--border-soft);
  padding: 8px 8px 0;
  text-align: right;
  font-size: 0.75rem;
  color: var(--text-muted);
}
// Simple Study Tracker
// Data structure:
// {
//   id: string,
//   title: string,
//   description: string,
//   category: string,
//   priority: "low" | "medium" | "high",
//   dueDate: string | null (ISO),
//   status: "pending" | "done",
//   createdAt: number (timestamp)
// }

const STORAGE_KEY = "studyTrackerTasks";

let tasks = [];
let editingTaskId = null;

// Helpers
function saveTasks() {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(tasks));
}

function loadTasks() {
  try {
    const raw = localStorage.getItem(STORAGE_KEY);
    if (!raw) return [];
    const parsed = JSON.parse(raw);
    if (!Array.isArray(parsed)) return [];
    return parsed;
  } catch (e) {
    console.error("Failed to load tasks:", e);
    return [];
  }
}

function formatDate(dateStr) {
  if (!dateStr) return "No due date";
  const date = new Date(dateStr);
  if (Number.isNaN(date.getTime())) return "No due date";
  return date.toLocaleDateString(undefined, {
    day: "2-digit",
    month: "short",
    year: "numeric"
  });
}

function priorityLabel(priority) {
  if (priority === "high") return "High priority";
  if (priority === "low") return "Low priority";
  return "Medium priority";
}

// Rendering
function renderTasks() {
  const listEl = document.getElementById("taskList");
  const template = document.getElementById("taskTemplate");
  const countEl = document.getElementById("taskCount");

  const filterCategory = document.getElementById("filterCategory").value;
  const filterStatus = document.getElementById("filterStatus").value;
  const sortBy = document.getElementById("sortBy").value;

  let filtered = tasks.slice();

  if (filterCategory !== "all") {
    filtered = filtered.filter((t) => t.category === filterCategory);
  }

  if (filterStatus !== "all") {
    filtered = filtered.filter((t) => t.status === filterStatus);
  }

  filtered.sort((a, b) => {
    if (sortBy === "createdDesc") {
      return b.createdAt - a.createdAt;
    }
    if (sortBy === "createdAsc") {
      return a.createdAt - b.createdAt;
    }
    if (sortBy === "dueAsc") {
      const aTime = a.dueDate ? new Date(a.dueDate).getTime() : Infinity;
      const bTime = b.dueDate ? new Date(b.dueDate).getTime() : Infinity;
      return aTime - bTime;
    }
    if (sortBy === "priorityDesc") {
      const order = { high: 3, medium: 2, low: 1 };
      return order[b.priority] - order[a.priority];
    }
    return 0;
  });

  listEl.innerHTML = "";

  filtered.forEach((task) => {
    const clone = template.content.cloneNode(true);
    const itemEl = clone.querySelector(".task-item");
    const titleEl = clone.querySelector(".task-title");
    const categoryEl = clone.querySelector(".task-category");
    const descriptionEl = clone.querySelector(".task-description");
    const priorityEl = clone.querySelector(".task-priority");
    const dueEl = clone.querySelector(".task-due");
    const toggleBtn = clone.querySelector(".status-toggle");
    const editBtn = clone.querySelector(".btn-edit");
    const deleteBtn = clone.querySelector(".btn-delete");

    itemEl.dataset.id = task.id;

    titleEl.textContent = task.title;
    categoryEl.textContent = task.category;
    descriptionEl.textContent = task.description || "No description";
    priorityEl.textContent = priorityLabel(task.priority);
    priorityEl.classList.add(`priority-${task.priority}`);
    dueEl.textContent = `Due: ${formatDate(task.dueDate)}`;

    if (task.status === "done") {
      itemEl.classList.add("completed");
      toggleBtn.classList.add("done");
    }

    toggleBtn.addEventListener("click", () => {
      toggleTaskStatus(task.id);
    });

    editBtn.addEventListener("click", () => {
      startEditingTask(task.id);
    });

    deleteBtn.addEventListener("click", () => {
      deleteTask(task.id);
    });

    listEl.appendChild(clone);
  });

  const count = filtered.length;
  countEl.textContent = `${count} ${count === 1 ? "task" : "tasks"}`;
}

// CRUD operations
function addTask(data) {
  const newTask = {
    id: Date.now().toString(),
    title: data.title.trim(),
    description: data.description.trim(),
    category: data.category,
    priority: data.priority,
    dueDate: data.dueDate || null,
    status: "pending",
    createdAt: Date.now()
  };
  tasks.push(newTask);
  saveTasks();
  renderTasks();
}

function updateTask(id, data) {
  const index = tasks.findIndex((t) => t.id === id);
  if (index === -1) return;
  tasks[index] = {
    ...tasks[index],
    title: data.title.trim(),
    description: data.description.trim(),
    category: data.category,
    priority: data.priority,
    dueDate: data.dueDate || null
  };
  saveTasks();
  renderTasks();
}

function deleteTask(id) {
  const ok = confirm("Delete this task?");
  if (!ok) return;
  tasks = tasks.filter((t) => t.id !== id);
  saveTasks();
  renderTasks();
}

function toggleTaskStatus(id) {
  const task = tasks.find((t) => t.id === id);
  if (!task) return;
  task.status = task.status === "pending" ? "done" : "pending";
  saveTasks();
  renderTasks();
}

// Editing
function startEditingTask(id) {
  const task = tasks.find((t) => t.id === id);
  if (!task) return;

  editingTaskId = id;

  document.getElementById("title").value = task.title;
  document.getElementById("description").value = task.description;
  document.getElementById("category").value = task.category;
  document.getElementById("priority").value = task.priority;
  document.getElementById("dueDate").value = task.dueDate || "";

  document.getElementById("formTitle").textContent = "Edit task";
  document.getElementById("submitBtn").textContent = "Update task";
  document.getElementById("cancelEditBtn").classList.remove("hidden");
}

function cancelEditing() {
  editingTaskId = null;
  document.getElementById("taskForm").reset();
  document.getElementById("formTitle").textContent = "Add new task";
  document.getElementById("submitBtn").textContent = "Save task";
  document.getElementById("cancelEditBtn").classList.add("hidden");
}

// Init
document.addEventListener("DOMContentLoaded", () => {
  tasks = loadTasks();
  renderTasks();

  const form = document.getElementById("taskForm");
  const cancelEditBtn = document.getElementById("cancelEditBtn");

  form.addEventListener("submit", (event) => {
    event.preventDefault();

    const title = document.getElementById("title").value;
    const description = document.getElementById("description").value || "";
    const category = document.getElementById("category").value;
    const priority = document.getElementById("priority").value;
    const dueDate = document.getElementById("dueDate").value;

    if (!title.trim()) {
      alert("Title is required.");
      return;
    }

    const taskData = { title, description, category, priority, dueDate };

    if (editingTaskId) {
      updateTask(editingTaskId, taskData);
    } else {
      addTask(taskData);
    }

    cancelEditing();
  });

  cancelEditBtn.addEventListener("click", () => {
    cancelEditing();
  });

  document.getElementById("filterCategory").addEventListener("change", renderTasks);
  document.getElementById("filterStatus").addEventListener("change", renderTasks);
  document.getElementById("sortBy").addEventListener("change", renderTasks);
});