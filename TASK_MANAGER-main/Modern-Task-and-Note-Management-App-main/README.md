# 📝 Tasks & Notes — Modern Task and Note Management App

> **Computer Science Internal Assessment (IA)**  
> Built with React, Vite, and Tailwind CSS

---

## 📌 About This Project

This is my **Computer Science Internal Assessment** project — a fully functional, modern web application for managing tasks and notes. The app was designed and built from scratch to solve a real-world problem: helping users stay organized by providing a clean, all-in-one space to track tasks, write notes, and view deadlines.

The application demonstrates core concepts in software development including **component-based architecture**, **state management**, **local data persistence**, **responsive UI design**, and **keyboard accessibility**.

---

## 🎯 What the App Does

- **Create, edit, and delete tasks** with priorities, due dates, categories, and tags
- **Create, edit, and delete notes** with colour coding and categories
- **Dashboard view** showing today's tasks, overdue tasks, and recent notes at a glance
- **Calendar view** showing upcoming, today's, and overdue tasks in a timeline
- **Search** across all tasks and notes in real time
- **Filter and sort** tasks by status, priority, and date
- **Dark mode / Light mode** toggle that saves your preference
- **Keyboard shortcuts** for quick navigation (`T` = new task, `N` = new note, `/` = search, `Esc` = close)
- **Data persistence** — everything is saved automatically to your browser's local storage, so nothing is lost on page refresh

---

## 🗂️ Project Structure

```
src/
│
├── main.jsx                  # Entry point — starts the React app
├── App.jsx                   # Root component — holds all global state
├── App.css                   # App-level styles
├── index.css                 # Global CSS resets and base styles
│
├── components/               # UI building blocks (one file per screen/feature)
│   ├── Dashboard.jsx         # Home screen with stats, today's tasks, recent notes
│   ├── Sidebar.jsx           # Left navigation panel with categories and quick stats
│   ├── TasksView.jsx         # Full task list with filters, sorting, and actions
│   ├── NotesView.jsx         # Full notes list with filters and actions
│   ├── CalendarView.jsx      # Timeline view of upcoming, today's, and overdue tasks
│   ├── TaskModal.jsx         # Pop-up form for creating/editing a task
│   ├── NoteModal.jsx         # Pop-up form for creating/editing a note
│   ├── SearchBar.jsx         # Search input with keyboard shortcut (Ctrl+K)
│   └── Toast.jsx             # Notification pop-up (success/error/warning/info)
│
├── contexts/
│   └── ThemeContext.jsx      # Global dark/light mode state (React Context API)
│
├── hooks/
│   └── useLocalStorage.js    # Custom hook — saves/loads data from localStorage
│
├── utils/
│   ├── helpers.js            # Helper functions for filtering, sorting, and formatting
│   └── colors.js             # Colour palette and light/dark theme definitions
│
└── data/
    └── categories.js         # Task categories, note categories, and note colour options
```

---

## 🔍 Code Explained Simply

### `main.jsx` — The Starting Point

```jsx
ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```

This is the very first file that runs. It finds the `<div id="root">` in `index.html` and tells React to render the entire app inside it. Think of it as the "on" switch for the application.

---

### `App.jsx` — The Brain of the App

This is the most important file. It is the **root component** that holds all the data and settings for the entire application.

**What it stores (state variables):**
- `todos` — the list of all tasks
- `notes` — the list of all notes
- `darkMode` — whether dark mode is on or off
- `activeTab` — which view is showing (dashboard, tasks, notes, or calendar)
- `selectedCategory` — which category filter is active
- `searchTerm` — what the user has typed in the search bar
- `showModal` — whether a create/edit pop-up is open
- `editingItem` — the task or note currently being edited

**Key features in App.jsx:**

*Dark/Light mode:*
```jsx
const themeClasses = darkMode
  ? 'bg-gray-900 text-white'
  : 'bg-gradient-to-br from-rose-50 via-rose-100 to-pink-50';
```
Depending on `darkMode`, the entire background colour of the app changes.

*Keyboard shortcuts:*
```jsx
if (e.key === 'n') handleNewNote();
if (e.key === 't') handleNewTask();
if (e.key === '/') searchInput.focus();
if (e.key === 'Escape') handleModalClose();
```
The app listens for key presses on the whole window and triggers actions instantly.

*Routing between views:*
```jsx
{activeTab === 'dashboard' && <Dashboard />}
{activeTab === 'tasks'     && <TasksView />}
{activeTab === 'notes'     && <NotesView />}
{activeTab === 'calendar'  && <CalendarView />}
```
Instead of loading different pages, the app simply shows or hides different components based on `activeTab`. This is called **client-side routing**.

---

### `hooks/useLocalStorage.js` — Saving Data Automatically

This is a **custom React hook** that works exactly like React's built-in `useState`, but with one extra power: it automatically saves data to the browser's `localStorage` every time something changes.

```js
const [todos, setTodos] = useLocalStorage('todos', []);
```

This means that even if you close the browser tab and come back, your tasks and notes are still there.

It also listens for changes across browser tabs:
```js
window.addEventListener('storage', handleStorageChange);
```
So if you had the app open in two tabs and added a task in one, the other tab would update too.

---

### `contexts/ThemeContext.jsx` — Sharing Theme Everywhere

React **Context** is a way to share data across many components without passing it down manually through every level. `ThemeContext` makes the current theme (`isDark`, `toggleTheme`) available anywhere in the app.

It also applies the theme directly to the page's root element:
```js
document.documentElement.classList.toggle('dark', isDark);
```
This activates Tailwind CSS's built-in dark mode styling.

---

### `utils/helpers.js` — The Utility Toolkit

This file contains reusable **pure functions** — small, focused tools that do one job each.

| Function | What it does |
|---|---|
| `formatTime(timeString)` | Converts `"14:30"` → `"2:30 PM"` |
| `getPriorityColor(priority)` | Returns a CSS class for red/yellow/green dot |
| `getNoteColor(color)` | Returns CSS classes for coloured note backgrounds |
| `getCurrentDate()` | Returns today's date in a friendly readable format |
| `getTodayTasks(todos)` | Filters tasks that are due today |
| `getOverdueTasks(todos)` | Filters tasks that are past their due date |
| `getUpcomingTasks(todos)` | Filters tasks due in the next 7 days |
| `validateTask(task)` | Checks that a task has a title before saving |
| `getFilteredTasks(...)` | Filters and sorts the task list based on user settings |
| `getFilteredNotes(...)` | Filters and sorts the note list based on user settings |

**Example — filtering tasks:**
```js
// Only show tasks that match the search term
filtered = filtered.filter(task =>
  task.title.toLowerCase().includes(searchTerm.toLowerCase())
);

// Sort by priority (high → medium → low)
filtered.sort((a, b) => {
  const order = { high: 3, medium: 2, low: 1 };
  return order[b.priority] - order[a.priority];
});
```

---

### `utils/colors.js` — The Colour System

Defines the complete colour palette for the app using a wine/rose theme, and creates two theme objects:

```js
export const lightTheme = {
  background: '#fefefe',
  primary:    '#a33353',  // wine red
  ...
};

export const darkTheme = {
  background: '#212529',
  primary:    '#c96378',  // lighter wine for contrast
  ...
};
```

These are applied as CSS variables via `ThemeContext`, keeping all colour decisions in one place.

---

### `data/categories.js` — The Data Definitions

Defines the fixed lists that the app uses for organisation:

- **Task categories:** Work, Personal, Ideas, Health, Learning, Goals, Quick — each with an icon and gradient colour
- **Note categories:** Personal, Work, Ideas, Journal, Learning, Research, Meeting Notes, Quick Notes
- **Note colours:** Default, Yellow, Green, Blue, Purple, Pink, Wine, Cyan

---

### `components/Dashboard.jsx` — The Home Screen

The first thing the user sees. It shows:

1. **Welcome header** with today's full date
2. **Four stats cards:** Today's Tasks, Overdue, Notes, Completed
3. **Today's task list** — clickable to mark as complete or edit
4. **Recent notes grid** — shows the 6 most recent notes

Each stat card is built from an array:
```js
const statsCards = [
  { title: "Today's Tasks", count: todayTasks.length, icon: CheckSquare, color: "from-blue-500 to-blue-600" },
  { title: "Overdue",        count: overdueTasks.length, ...},
  ...
];
```
This makes it easy to add or remove cards without changing the layout code.

---

### `components/Sidebar.jsx` — The Navigation Panel

The sidebar on the left side shows:
- **Main navigation:** Dashboard, Tasks, Notes, Calendar (with count badges)
- **Category navigation:** Each category links directly to the filtered task list
- **Quick stats mini-panel:** Today, Overdue, Notes counts

On mobile screens, the sidebar is hidden and slides in when the menu button is pressed:
```jsx
className={`${sidebarOpen ? 'translate-x-0' : '-translate-x-full'} lg:translate-x-0`}
```

---

### `components/TasksView.jsx` — The Full Task Manager

Displays all tasks with controls for:
- **Filtering:** All, Pending, Completed, Starred, Archived, High/Medium/Low priority
- **Sorting:** By date created, date updated, priority, or due date
- **View toggle:** Grid or list layout
- **Per-task actions:** Complete ✓, Star ⭐, Archive 📦, Delete 🗑️, Edit ✏️

All filtering/sorting logic is handled by `getFilteredTasks()` from `helpers.js`, keeping the component clean.

---

### `components/NotesView.jsx` — The Full Notes Manager

Similar to TasksView but for notes. Each note shows:
- Coloured background based on the note's assigned colour
- Title, content preview, category label, and last-updated date
- Actions: Star, Archive, Delete, Edit

---

### `components/CalendarView.jsx` — The Timeline View

Groups tasks into three sections:
1. **Overdue** — tasks past their due date (shown in red)
2. **Today** — tasks due today
3. **Upcoming** — tasks due in the next 7 days

Uses a helper function to display relative time:
```js
const relativeDue = (date) => {
  const days = Math.ceil((new Date(date) - new Date()) / (1000 * 60 * 60 * 24));
  if (days > 1) return `in ${days} days`;
  if (days === 1) return 'tomorrow';
  return 'today';
};
```

---

### `components/TaskModal.jsx` — Create / Edit Task Form

A pop-up form with fields for:
- Title (required)
- Description
- Category (dropdown)
- Priority: Low / Medium / High
- Due date and time
- Star toggle

When editing, it pre-fills the form with the existing task's data:
```js
useEffect(() => {
  if (editingItem) setTaskForm({ ...editingItem });
}, [editingItem]);
```

On save, it either **adds** a new task or **updates** the existing one:
```js
if (editingItem) {
  setTodos(todos.map(todo => todo.id === editingItem.id ? newTask : todo));
} else {
  setTodos([...todos, newTask]);
}
```

---

### `components/NoteModal.jsx` — Create / Edit Note Form

Works the same way as `TaskModal` but for notes. Fields include:
- Title
- Content (text area)
- Category
- Colour picker (4 wine-theme colours)
- Star toggle

---

### `components/SearchBar.jsx` — The Search Input

A search box in the top navigation bar. As the user types, the search term updates instantly and the current view filters in real time (no button press needed — this is called **live search**).

Supports two ways to focus the search:
- Click on it
- Press `Ctrl+K` (or `Cmd+K` on Mac) from anywhere in the app

---

### `components/Toast.jsx` — Notification Pop-ups

A small animated notification that appears in the top-right corner. Supports four types: `success`, `error`, `warning`, and `info` — each with its own colour and icon. It automatically disappears after 3 seconds.

```js
useEffect(() => {
  const timer = setTimeout(() => {
    setIsVisible(false);
    setTimeout(onClose, 300); // wait for fade-out animation
  }, duration);
  return () => clearTimeout(timer);
}, []);
```

---

## 🛠️ Technologies Used

| Technology | Purpose |
|---|---|
| **React 18** | Building the user interface with reusable components |
| **Vite** | Fast development server and build tool |
| **Tailwind CSS** | Utility-first CSS framework for styling |
| **Lucide React** | Icon library (all the icons in the app) |
| **localStorage API** | Saving data in the browser without a backend |
| **React Context API** | Sharing theme state across the whole app |
| **CSS Animations** | Smooth transitions and modal entrance effects |

---

## 🚀 How to Run the Project

Make sure you have **Node.js** installed, then:

```bash
# 1. Install dependencies
npm install

# 2. Start the development server
npm run dev

# 3. Open in your browser
# http://localhost:5173
```

To build for production:
```bash
npm run build
```

---

## ⌨️ Keyboard Shortcuts

| Key | Action |
|---|---|
| `T` | Open "New Task" form |
| `N` | Open "New Note" form |
| `/` | Focus the search bar |
| `Ctrl + K` | Focus the search bar |
| `Escape` | Close any open modal or sidebar |

---

## 💡 Key Computer Science Concepts Demonstrated

- **Component-based architecture** — The UI is broken into small, reusable, single-purpose components
- **State management** — React's `useState` and `useEffect` hooks manage all dynamic data
- **Custom hooks** — `useLocalStorage` abstracts complex logic into a reusable function
- **Data persistence** — Browser `localStorage` API is used to store data without a server
- **Functional programming** — Helper functions use `.filter()`, `.map()`, and `.sort()` for data processing
- **Event-driven programming** — Keyboard listeners and click handlers respond to user actions
- **Responsive design** — The layout adapts for mobile, tablet, and desktop screen sizes
- **Dark/light theming** — CSS variables and Tailwind's dark mode classes enable dynamic theming

---

*This project was developed as part of the IB Computer Science Internal Assessment.*
