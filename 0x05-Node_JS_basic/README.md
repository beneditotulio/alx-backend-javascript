readme
// Task 0: 0-console.js
function displayMessage(message) {
  console.log(message);
}
module.exports = displayMessage;

// Task 1: 1-stdin.js
const readline = require('readline').createInterface({
  input: process.stdin,
  output: process.stdout
});

console.log('Welcome to Holberton School, what is your name?');
readline.on('line', (input) => {
  console.log(`Your name is: ${input}`);
  readline.close();
}).on('close', () => {
  console.log('This important software is now closing');
});

// Task 2: 2-read_file.js
const fs = require('fs');

function countStudents(path) {
  try {
    const data = fs.readFileSync(path, 'utf8');
    const lines = data.split('\n').filter(Boolean);
    const students = lines.slice(1); // Remove header
    console.log(`Number of students: ${students.length}`);
    const fields = {};
    students.forEach((student) => {
      const [,, field] = student.split(',');
      if (!fields[field]) fields[field] = [];
      fields[field].push(student.split(',')[0]);
    });
    Object.keys(fields).forEach((field) => {
      console.log(`Number of students in ${field}: ${fields[field].length}. List: ${fields[field].join(', ')}`);
    });
  } catch (err) {
    throw new Error('Cannot load the database');
  }
}
module.exports = countStudents;

// Task 3: 3-read_file_async.js
const fs = require('fs').promises;

async function countStudents(path) {
  try {
    const data = await fs.readFile(path, 'utf8');
    const lines = data.split('\n').filter(Boolean);
    const students = lines.slice(1); // Remove header
    console.log(`Number of students: ${students.length}`);
    const fields = {};
    students.forEach((student) => {
      const [,, field] = student.split(',');
      if (!fields[field]) fields[field] = [];
      fields[field].push(student.split(',')[0]);
    });
    Object.keys(fields).forEach((field) => {
      console.log(`Number of students in ${field}: ${fields[field].length}. List: ${fields[field].join(', ')}`);
    });
  } catch (err) {
    throw new Error('Cannot load the database');
  }
}
module.exports = countStudents;

// Task 4: 4-http.js
const http = require('http');

const app = http.createServer((req, res) => {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello Holberton School!');
});
app.listen(1245);
module.exports = app;

// Task 5: 5-http.js
const http = require('http');
const countStudents = require('./3-read_file_async');

const app = http.createServer(async (req, res) => {
  if (req.url === '/') {
    res.writeHead(200, {'Content-Type': 'text/plain'});
    res.end('Hello Holberton School!');
  } else if (req.url === '/students') {
    res.writeHead(200, {'Content-Type': 'text/plain'});
    res.write('This is the list of our students\n');
    try {
      await countStudents(process.argv[2]);
      res.end();
    } catch (error) {
      res.end(error.message);
    }
  }
});
app.listen(1245);
module.exports = app;

// Task 6: 6-http_express.js
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('Hello Holberton School!');
});

app.listen(1245, () => console.log('Server is running on port 1245'));
module.exports = app;