// project structure :- 
blog
|-- node_modules/
|-- public/
|   |-- styles.css
|-- views/
|   |-- compose.ejs
|   |-- home.ejs
|-- app.js
|-- package.json
|-- package-lock.json



const express = require('express');
const mongoose = require('mongoose');
const bodyParser = require('body-parser');

const app = express();
const PORT = process.env.PORT || 3000;

// Connect to MongoDB
mongoose.connect('mongodb://localhost/blog', { useNewUrlParser: true, useUnifiedTopology: true });

// Create a schema
const postSchema = new mongoose.Schema({
  title: String,
  content: String,
});

// Create a model
const Post = mongoose.model('Post', postSchema);

// Set up middleware
app.use(express.static('public'));
app.use(bodyParser.urlencoded({ extended: true }));
app.set('view engine', 'ejs');

// Routes
app.get('/', (req, res) => {
  Post.find({}, (err, posts) => {
    if (err) {
      console.error(err);
    } else {
      res.render('home', { posts: posts });
    }
  });
});

app.get('/compose', (req, res) => {
  res.render('compose');
});

app.post('/compose', (req, res) => {
  const post = new Post({
    title: req.body.title,
    content: req.body.content,
  });

  post.save((err) => {
    if (err) {
      console.error(err);
    } else {
      res.redirect('/');
    }
  });
});

// Start the server
app.listen(PORT, () => {
  console.log(`Server is running on http://localhost:${PORT}`);
});
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Blog Home</title>
  <link rel="stylesheet" href="/styles.css">
</head>
<body>
  <h1>Blog Posts</h1>
  <ul>
    <% posts.forEach(post => { %>
      <li>
        <h2><%= post.title %></h2>
        <p><%= post.content %></p>
      </li>
    <% }) %>
  </ul>
  <a href="/compose">Compose</a>
</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Compose</title>
  <link rel="stylesheet" href="/styles.css">
</head>
<body>
  <h1>Compose</h1>
  <form action="/compose" method="post">
    <label for="title">Title</label>
    <input type="text" id="title" name="title" required>
    <label for="content">Content</label>
    <textarea id="content" name="content" rows="4" required></textarea>
    <button type="submit">Publish</button>
  </form>
</body>
</html>
body {
  font-family: Arial, sans-serif;
  background-color: #f4f4f4;
  margin: 0;
  padding: 0;
}

h1 {
  color: #333;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  background-color: #fff;
  margin: 10px 0;
  padding: 10px;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

a {
  display: inline-block;
  margin-top: 10px;
  padding: 8px 16px;
  background-color: #4caf50;
  color: #fff;
  text-decoration: none;
  border-radius: 4px;
}

a:hover {
  background-color: #45a049;
}

form {
  background-color: #fff;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

label {
  display: block;
  margin-bottom: 8px;
}

input,
textarea {
  width: 100%;
  padding: 8px;
  margin-bottom: 16px;
  box-sizing: border-box;
}

button {
  background-color: #4caf50;
  color: #fff;
  padding: 10px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #45a049;
}
nodeapp.js
