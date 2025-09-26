import React, { useState, useEffect } from "react";

function App() {
  const [tasks, setTasks] = useState(() => {
    // Load from localStorage if available
    const saved = localStorage.getItem("tasks");
    return saved ? JSON.parse(saved) : [];
  });
  const [input, setInput] = useState("");

  // Save tasks to localStorage whenever they change
  useEffect(() => {
    localStorage.setItem("tasks", JSON.stringify(tasks));
  }, [tasks]);

  const addTask = () => {
    if (input.trim() === "") return;
    const newTask = {
      id: Date.now(),
      text: input,
      completed: false,
    };
    setTasks([...tasks, newTask]);
    setInput("");
  };

  const toggleTask = (id) => {
    setTasks(
      tasks.map((task) =>
        task.id === id ? { ...task, completed: !task.completed } : task
      )
    );
  };

  const deleteTask = (id) => {
    setTasks(tasks.filter((task) => task.id !== id));
  };

  return (
    <div style={styles.container}>
      <h1 style={styles.heading}>✅ To-Do App</h1>
      <div style={styles.inputArea}>
        <input
          style={styles.input}
          value={input}
          onChange={(e) => setInput(e.target.value)}
          placeholder="Add a new task..."
        />
        <button style={styles.addBtn} onClick={addTask}>
          Add
        </button>
      </div>

      <ul style={styles.list}>
        {tasks.map((task) => (
          <li
            key={task.id}
            style={{
              ...styles.task,
              textDecoration: task.completed ? "line-through" : "none",
              color: task.completed ? "#888" : "#000",
            }}
          >
            <span onClick={() => toggleTask(task.id)} style={{ cursor: "pointer" }}>
              {task.text}
            </span>
            <button style={styles.deleteBtn} onClick={() => deleteTask(task.id)}>
              ❌
            </button>
          </li>
        ))}
      </ul>
    </div>
  );
}

// Inline styles for simplicity
const styles = {
  container: {
    maxWidth: "400px",
    margin: "50px auto",
    padding: "20px",
    borderRadius: "12px",
    backgroundColor: "#f9f9f9",
    boxShadow: "0 4px 10px rgba(0,0,0,0.1)",
  },
  heading: { textAlign: "center" },
  inputArea: { display: "flex", marginBottom: "20px" },
  input: {
    flex: 1,
    padding: "10px",
    borderRadius: "6px",
    border: "1px solid #ccc",
  },
  addBtn: {
    marginLeft: "10px",
    padding: "10px 15px",
    border: "none",
    borderRadius: "6px",
    backgroundColor: "#007bff",
    color: "white",
    cursor: "pointer",
  },
  list: { listStyle: "none", padding: 0 },
  task: {
    display: "flex",
    justifyContent: "space-between",
    padding: "8px",
    background: "#fff",
    borderRadius: "6px",
    marginBottom: "8px",
    boxShadow: "0 2px 5px rgba(0,0,0,0.05)",
  },
  deleteBtn: {
    border: "none",
    background: "transparent",
    cursor: "pointer",
    fontSize: "16px",
  },
};

export default App;