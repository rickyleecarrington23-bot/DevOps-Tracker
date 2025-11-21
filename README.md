# DevOps-Tracker
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