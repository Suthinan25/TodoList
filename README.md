# TodoList

import React, { useState } from 'react';

const ToDoList = () => {
  const [tasks, setTasks] = useState([]);
  const [newTask, setNewTask] = useState('');
  const [filter, setFilter] = useState('all');



  const addTask = () => {
    if (newTask.trim() !== '') {
      setTasks([...tasks, { id: Date.now(), text: newTask, completed: false }]);
      setNewTask('');
    }
  };

  const toggleTaskCompletion = (taskId) => {
    setTasks(tasks.map(task => 
      task.id === taskId ? { ...task, completed: !task.completed } : task
    ));
  };

  const deleteTask = (taskId) => {
    setTasks(tasks.filter(task => task.id !== taskId));
  };

  const filteredTasks = tasks.filter(task => {
    if (filter === 'active') return !task.completed;
    if (filter === 'completed') return task.completed;
    return true;
  });

  return (
    <div className="w-[35%] mx-auto bg-white shadow-lg rounded-lg overflow-hidden mt-10">
      <div className="px-6 py-4">
        <div className="font-bold text-4xl mb-10 text-center">
          <span>Software Engineer</span>
        </div>
        <div className="flex mb-4 justify-between items-center">
          <span className="text-gray-600">{tasks.length} {tasks.length === 1 ? 'task' : 'tasks'}</span>
          <div className="flex">
            <button 
              className={`mr-2 px-3 py-1 rounded-[15px] ${filter === 'all' ? 'bg-blue-500 text-white' : 'bg-gray-200 '}`}
              onClick={() => setFilter('all')}
            >
              All
            </button>
            <button 
              className={`mr-2 px-3 py-1 rounded-[15px] ${filter === 'active' ? 'bg-blue-500 text-white' : 'bg-gray-200 '}`}
              onClick={() => setFilter('active')}
            >
              Not Started Yet
            </button>
            <button 
              className={`px-3 py-1 rounded-[15px] ${filter === 'completed' ? 'bg-blue-500 text-white' : 'bg-gray-200 '}`}
              onClick={() => setFilter('completed')}
            >
              Completed
            </button>
          </div>
        </div>
        <div className="mb-4 flex">
          <input 
            type="text" 
            value={newTask}
            onChange={(e) => setNewTask(e.target.value)}
            className="flex-grow px-3 py-2 border-none  rounded-l-md focus:outline-none"
            placeholder="Add a new task"
          />
          <button 
            onClick={addTask}
            className="px-4 py-2 bg-blue-500 text-white rounded-[15px]"
          >
            Add
          </button>
        </div>
        <ul>
          {filteredTasks.map(task => (
            <li key={task.id} className="flex items-center mb-2 bg-green-100 p-2 rounded-lg">
              <input 
                type="checkbox" 
                checked={task.completed}
                onChange={() => toggleTaskCompletion(task.id)}
                className="mr-2 circular w-4 h-4"
              />
              <span className={`flex-grow ${task.completed ? 'line-through text-gray-500' : ''}`}>{task.text}</span>
              <button 
                onClick={() => deleteTask(task.id)}
                className="ml-2 px-2 py-1 text-red-500 border border-red-500 rounded hover:bg-red-500 hover:text-white transition"
              >
                Delete
              </button>
            </li>
          ))}
        </ul>
      </div>
    </div>
  );
};

export default ToDoList;
