# Backend Build
COMMAND | DESCRIPTION
* npm init -y | Creates package.json
* npm i | Node modules
* npm i nodemon -D | Automatically restarts node application when files change
* touch **.gitignore** | Instructs Git specifically what to ignore 
  * echo "**.DS_Store**" >> .gitignore
  * echo "**node_modules**" >> .gitignore
### Typical dependencies to install
* npm i body-parser | Node.js body parsing middleware
* npm i cors | CORS is a node.js package for providing a Connect/Express middleware that can be used to enable CORS with various options.
* npm i express | Web framework for node
* npm i mongoose | Elegant mongodb object modeling for node.js
* npm i morgan | HTTP request logger middleware for node.js

### Architecture
FOLDER | FILE(s) | DESCRIPTION
* db | connection.js | Defining connection

  <details>
   <summary>Expand Boilerplate</summary>
 
```
const mongoose = require("mongoose");

let MONGODB_URI =
  process.env.PROD_MONGODB || "mongodb://127.0.0.1:27017/blogApp";

mongoose
  .connect(MONGODB_URI, { useUnifiedTopology: true, useNewUrlParser: true })
  .then(() => console.log("Successfully connected to MongoDB."))
  .catch((e) => console.error("Connection error", e.message));

module.exports = mongoose.connection;

```
  </details>
* controllers | <variable-name>.js | CRUD functions
<details>
 <summary>Expand Boilerplate</summary>

```
const Variable = require("../models/variable");
const db = require("../db/connection");

db.on("error", console.error.bind(console, "MongoDB connection error:"));

const getVariable = async (req, res) => {
  try {
    const variables = await Variable.find();
    res.json(variables);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
};

const getVariable = async (req, res) => {
  try {
    const { id } = req.params;
    const variable = await Variable.findById(id);
    if (variable) {
      return res.json(variable);
    }
    res.status(404).json({ message: "Variable not found!" });
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
};

const createVariable = async (req, res) => {
  try {
    const variable = await new Variable(req.body);
    await variable.save();
    res.status(201).json(variable);
  } catch (error) {
    console.log(error);
    res.status(500).json({ error: error.message });
  }
};

const updateVariable = async (req, res) => {
  const { id } = req.params;
  await Variable.findByIdAndUpdate(id, req.body, { new: true }, (error, post) => {
    if (error) {
      return res.status(500).json({ error: error.message });
    }
    if (!variable) {
      return res.status(404).json({ message: "Variable not found!" });
    }
    res.status(200).json(variable);
  });
};

const deleteVariable = async (req, res) => {
  try {
    const { id } = req.params;
    const deleted = await Variable.findByIdAndDelete(id);
    if (deleted) {
      return res.status(200).send("Variable deleted");
    }
    throw new Error("Variable not found");
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
};

module.exports = {
  createPost,
  getPosts,
  getPost,
  updatePost,
  deletePost,
};

```
</details>
* models | <variable-name (generally singular)>.js | Schema
<details>
 <summary>Expand Boilerplate</summary>

```
const mongoose = require("mongoose");
const Schema = mongoose.Schema;

const Variable = new Schema(
  {
    title: { type: String, required: true },
    imgURL: { type: String, required: true },
    content: { type: String, required: true },
    author: { type: String, required: true },
  },
  { timestamps: true }
);

module.exports = mongoose.model("variable", Variable);

```
</details>
* routes | <variable-name>.js | API endpoints
<details>
 <summary>Expand Boilerplate</summary>

```
const { Router } = require("express");
const controllers = require("../controllers/<filename>");

const router = Router();

router.get("/<variable>", controllers.getVariable);
router.get("/<variable>/:id", controllers.getVariable);
router.post("/<variable>", controllers.createVariable);
router.put("/<variable>/:id", controllers.updateVariable);
router.delete("/<variable>/:id", controllers.deleteVariable);

module.exports = router;

```
</details>
* seed | <varibale-name>.js | Data to populate initial database
<details>
 <summary>Expand Boilerplate</summary>
 
```
const db = require("../db/connection");
const Variable = require("../models/variable");

db.on("error", console.error.bind(console, "MongoDB connection error:"));

const main = async () => {
  const variable = [
    {
    }
    ];
  await Variable.insertMany(variable);
  console.log("Created vaiables!");
  };
const run = async () => {
await main();
db.close();
};

run();

```
</details>
    
    
