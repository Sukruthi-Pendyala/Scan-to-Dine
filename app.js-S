const express = require('express');
const mongoose = require('mongoose');
const bodyParser = require('body-parser');
const session = require('express-session');
const path = require('path');

const app = express();

// Middleware
app.use(bodyParser.urlencoded({ extended: true }));
app.use(session({ secret: 'supersecretkey', resave: false, saveUninitialized: false }));
app.use(express.static('public')); // Serve static files from 'public' directory

// Connect to MongoDB
mongoose.connect('mongodb://localhost:27017/restaurantDB', { useNewUrlParser: true, useUnifiedTopology: true });

// Define User Schema
const userSchema = new mongoose.Schema({
  username: String,
  password: String,
  email: String // Added email field for the user
});

const User = mongoose.model('User', userSchema);

// Routes
app.get('/', (req, res) => {
  res.sendFile(path.join(__dirname, 'views', 'index.html'));
});

app.post('/signup', (req, res) => {
  const newUser = new User({
    username: req.body.username,
    password: req.body.password,
    email: req.body.email // Capture email from the form
  });
  newUser.save(err => {
    if (err) {
      console.log(err);
      res.redirect('/'); // Redirect back to signup on error
    } else {
      res.redirect('/'); // Redirect to login on success (you can change this behavior)
    }
  });
});

app.post('/login', (req, res) => {
  const username = req.body.username;
  const password = req.body.password;
  User.findOne({ username: username, password: password }, (err, user) => {
    if (err) {
      console.log(err);
      res.redirect('/'); // Redirect back to main page on error
    } else if (user) {
      req.session.user = user;
      res.redirect('/home');
    } else {
      res.redirect('/'); // Redirect back to main page on failure
    }
  });
});

app.get('/home', (req, res) => {
  if (req.session.user) {
    res.send(`Welcome to the Restaurant, ${req.session.user.username}!`);
  } else {
    res.redirect('/');
  }
});

// Start the server
app.listen(3000, () => {
  console.log('Server started on port 3000');
});
