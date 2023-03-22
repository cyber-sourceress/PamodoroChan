Pamodorochan

## Plan



Gameplan:

    MongoDB: Store user data, such as task names, descriptions, and durations, as well as the user's Pomodoro timer history.

    Express.js: Handle HTTP requests and responses, as well as to define routes and middleware.

    React.js: front-end of the Pomodoro timer + time tracker, including the timer itself and any other user interface elements.

    Node.js: Node.js to build the back-end of the Pomodoro timer + time tracker, including handling user authentication and storing user data in MongoDB.



## Electron app


 Electron allows you to package your MERN application as a standalone executable file that can be installed and run on Windows, macOS, and Linux systems.


## Method

    Timer: Methods to start, pause, and reset the timer. This can be done using JavaScript's built-in setInterval function, which can be used to execute a function at a regular interval.

    Time Start: Need to record the local time when the timer starts. This can be done using JavaScript's Date object, which provides methods for getting the current date and time.

    Elapsed Time: Need to calculate the elapsed time between the start of the timer and the time when the user indicates that they have finished the task or become distracted. This can be done using JavaScript's Date object to get the current time and then subtracting the start time from the current time.

    Distracted Flag: A method to detect when the user becomes distracted and pause the timer. This can be done by adding a button that the user can click to indicate that they have become distracted.

    Record Elapsed Times: Record the elapsed times into a CSV file, along with a distracted flag and any notes the user wants to include. This can be done using a server-side scripting language like Node.js, which provides a fs module that allows you to read and write files.

```javascript
// Start timer
let startTime = null;
let intervalId = null;

function startTimer() {
  startTime = new Date();
  intervalId = setInterval(updateTimer, 1000); // Update timer every second
}

// Update timer
function updateTimer() {
  const elapsedTime = new Date() - startTime; // Elapsed time in milliseconds
  // Update timer display
}

// Pause timer
function pauseTimer() {
  clearInterval(intervalId);
}

// Record elapsed time
function recordTime(distracted, notes) {
  const endTime = new Date();
  const elapsedTime = endTime - startTime; // Elapsed time in milliseconds
  // Write elapsed time, distracted flag, and notes to CSV file
}

// Handle distracted flag
function onDistracted() {
  pauseTimer();
  recordTime(true, '');
  // Display distracted message to user
}

// Handle task completion
function onComplete() {
  pauseTimer();
  recordTime(false, '');
  // Display completion message to user
}
```


## React implementation:

Proposed Structure:

    Create a Timer component that displays the timer and handles timer-related functions like starting, stopping, and resetting the timer.

    Create a DistractedButton component that the user can click to indicate that they have become distracted. This component should handle pausing the timer and recording the elapsed time.

    Create a TaskNotes component that allows the user to enter notes about the task they just completed, which will be included in the CSV file.

    Create a ThemeSelector component that allows the user to choose between accessible dark mode and accessible vaporwave mode.

    Use TailwindCSS to style the app with the chosen theme.

Outline

jsx
```javascript
import React, { useState, useEffect } from 'react';

function Timer() {
  const [startTime, setStartTime] = useState(null);
  const [elapsedTime, setElapsedTime] = useState(0);
  const [isRunning, setIsRunning] = useState(false);

  useEffect(() => {
    let intervalId = null;
    if (isRunning) {
      intervalId = setInterval(() => {
        setElapsedTime(new Date() - startTime);
      }, 1000);
    } else {
      clearInterval(intervalId);
    }
    return () => clearInterval(intervalId);
  }, [isRunning, startTime]);

  const startTimer = () => {
    setStartTime(new Date());
    setIsRunning(true);
  };

  const pauseTimer = () => {
    setIsRunning(false);
  };

  const resetTimer = () => {
    setStartTime(null);
    setElapsedTime(0);
    setIsRunning(false);
  };

  return (
    <div>
      <div>{elapsedTime}</div>
      {isRunning ? (
        <button onClick={pauseTimer}>Pause</button>
      ) : (
        <button onClick={startTimer}>Start</button>
      )}
      <button onClick={resetTimer}>Reset</button>
    </div>
  );
}

function DistractedButton() {
  const [isDistracted, setIsDistracted] = useState(false);

  const onDistracted = () => {
    setIsDistracted(true);
    pauseTimer(); // Call the pauseTimer function from the Timer component
    recordTime(true, ''); // Call the recordTime function to record elapsed time and distracted flag
  };

  return (
    <div>
      {isDistracted ? (
        <div>You were distracted.</div>
      ) : (
        <button onClick={onDistracted}>I got distracted</button>
      )}
    </div>
  );
}

function TaskNotes() {
  const [notes, setNotes] = useState('');

  const onNotesChange = (event) => {
    setNotes(event.target.value);
  };

  return (
    <div>
      <label>Task notes:</label>
      <textarea value={notes} onChange={onNotesChange} />
    </div>
  );
}

function ThemeSelector() {
  const [isDarkMode, setIsDarkMode] = useState(false);

  const toggleTheme = () => {
    setIsDarkMode(!isDarkMode);
  };

  return (
    <div>
      <label>
        <input type="checkbox" checked={isDarkMode} onChange={toggleTheme} />
        Dark mode
      </label>
    </div>
  );
}

function KoFiButton() {
  return (
    <a href="https://ko-fi.com/remiliagrimm" target="_blank" className="ko-fi-button"><img src="https://ko-fi.com/img/button-3.png" height="36"></a>
  );
}

export default KoFiButton;

function App() {
  return (
    <div className={isDarkMode ? 'dark-mode' : 'vaporwave

```

## Accessibility Modes

For accessible dark mode, it's important to choose a font that has high contrast and is easy to read. Some examples of fonts that work well for dark mode are Roboto, Open Sans, and Lato. The font-family property in CSS to set the font for the entire app or for specific elements.

For vaporwave mode:
We may want to use a font that has a retro or futuristic feel, such as Aileron, Neon 80s, or Retro Wave. The font-family property in CSS to set the font for the entire app or for specific elements.

Setting up Tailwind:

To use Tailwind CSS, we need to install it into the project.
Using npm: 

```bash
npm install tailwindcss
```

Create a tailwind.css file in the project and import it into the app.

Tailwind CSS for the accessible dark mode and vaporwave modes, including font choices:

```css
/* Import the base Tailwind CSS styles */
@import 'tailwindcss/base';


// Base styles
body {
  font-family: 'Helvetica Neue', sans-serif;
}

// Dark mode
.dark-mode {
  background-color: #333333;
  color: #f1f1f1;
  font-family: 'Roboto', sans-serif;
}

.dark-mode button {
  background-color: #f1f1f1;
  color: #333333;
  font-family: 'Roboto', sans-serif;
}

.dark-mode textarea {
  background-color: #f1f1f1;
  color: #333333;
  font-family: 'Roboto', sans-serif;
}

// Vaporwave mode
.vaporwave {
  background-color: #ff00ff;
  color: #8e44ad;
  font-family: 'Retro Wave', sans-serif;
}

.vaporwave button {
  background-color: #8e44ad;
  color: #ff00ff;
  font-family: 'Retro Wave', sans-serif;
}

.vaporwave textarea {
  background-color: #8e44ad;
  color: #ff00ff;
  font-family: 'Retro Wave', sans-serif;
}

.ko-fi-button {
  display: inline-block;
  margin: 10px;
  @apply bg-white text-gray-700 font-medium py-2 px-4 border border-gray-400 rounded shadow;
}

/* Import the Tailwind CSS utility classes */
@import 'tailwindcss/utilities';
```

This example sets the font-family property for each mode to Roboto for the accessible dark mode and Retro Wave for the vaporwave mode. You can of course substitute any other font you prefer.


## Building the downloadable

Building the Electron MERN app:

    Set up back-end API using Node.js, Express, and MongoDB. This will involve creating routes to handle user authentication, saving and retrieving PomodoroChan data to and from the database, and any other necessary API endpoints.

    Front-end is using React and the Tailwind CSS styles mentioned above.
    We may also need: 
    To create components to handle the Pomodoro timer functionality, user authentication, and any other features desired in the app; however, as this will be exclusively an Electron App at the moment, the authentication will likely not be needed for the scope of the project.

    Use Electron to package the MERN app as a desktop application that can be run on Windows, macOS, and Linux.

Rough outline of the steps involved:
{ warning }
> Note: This is purely from what I could glean from the documentation and some lectures I watched, I have yet to build the Electron App, so I don't know how feasible this is, but it should work based on what I have reviewed. 
{ /warning }

    Set up back-end API:
        Install Node.js and MongoDB on dev machine.
        Create a new Node.js project and install the necessary dependencies, including Express and Mongoose.
        Create a schema for PomodoroChan timer data using Mongoose.
        Set up API routes to handle user authentication(optional at this point, probably add something basic just as best practice but not have it enabled at launch) and Pomodoro timer data.
        Test API using Postman or a similar tool.

    Set up front-end:
        Create a new React project and install the necessary dependencies, including Axios and React Router.
        Create a user authentication component to handle sign up, login, and logout functionality.
        Create a Pomodoro timer component to handle the timer functionality and display data from the back-end API.
        Use Tailwind CSS to style components.
        Test front-end using a local development server.

    Use Electron to package the app:
        Install Electron on dev machine.
        Create a new Electron project and copy the React front-end into the project folder.
        Use Electron's APIs to interact with the operating system and create desktop-specific features like tray icons and menus.
        Package the app for distribution on Windows, macOS, and Linux.



index.js
```javascript
const express = require('express');
const mongoose = require('mongoose');
const cors = require('cors');

const app = express();

// Middleware
app.use(express.json());
app.use(cors());

// Connect to MongoDB
mongoose.connect('mongodb://localhost/pomodoro-timer', { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => console.log('Connected to MongoDB'))
  .catch(err => console.error('Error connecting to MongoDB:', err));

// Routes
app.use('/api/auth', require('./routes/auth'));
app.use('/api/timer', require('./routes/timer'));

// Start the server
const port = process.env.PORT || 5000;
app.listen(port, () => console.log(`Server started on port ${port}`));

```

```javascript
const express = require('express');
const mongoose = require('mongoose');
const cors = require('cors');

const app = express();

// Middleware
app.use(express.json());
app.use(cors());

// Connect to MongoDB
mongoose.connect('mongodb://localhost/pomodoro-timer', { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => console.log('Connected to MongoDB'))
  .catch(err => console.error('Error connecting to MongoDB:', err));

// Routes
app.use('/api/auth', require('./routes/auth'));
app.use('/api/timer', require('./routes/timer'));

// Start the server
const port = process.env.PORT || 5000;
app.listen(port, () => console.log(`Server started on port ${port}`));

```


app.js
```javascript
import React from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';

import PrivateRoute from './components/PrivateRoute';
import Navbar from './components/Navbar';
import Signup from './pages/Signup';
import Login from './pages/Login';
import Timer from './pages/Timer';
import NotFound from './pages/NotFound';

function App() {
  return (
    <Router>
      <Navbar />
      <Switch>
        <Route exact path="/" component={Signup} />
        <Route exact path="/login" component={Login} />
        <PrivateRoute exact path="/timer" component={Timer} />
        <Route component={NotFound} />
      </Switch>
    </Router>
  );
}

export default App;

```

schema.js
```javascript
const mongoose = require('mongoose');

const pomodoroSchema = new mongoose.Schema({
  userId: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'User'
  },
  startTime: {
    type: Date,
    required: true
  },
  endTime: {
    type: Date,
    required: true
  },
  distracted: {
    type: Boolean,
    default: false
  },
  notes: {
    type: String
  }
});

module.exports = mongoose.model('Pomodoro', pomodoroSchema);

```

db.js
```javascript
const mongoose = require('mongoose');
const MONGODB_URI = process.env.MONGODB_URI || 'mongodb://localhost/pomodoro-timer';

mongoose.connect(MONGODB_URI, { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => {
    console.log('Connected to database.');
  })
  .catch((err) => {
    console.log(`Error connecting to database: ${err.message}`);
  });

```


Basic Pamodoro Timer that can be built upon and scaled as needed. The index.js file sets up the server and database connection. The app.js file sets up the middleware and routing for the app. The schema.js file defines the schema for the Pomodoro timer data. Finally, the db.js file sets up the database connection.

