# Front-end Build
COMMAND | DESCRIPTION
* npx create-react-app \<file-name\> | Creates a react app with boiler plate
* npm i axios | Promise based HTTP client for the browser and node.js
* npm i react-router-dom | DOM bindings for React Router
# Back-end Build
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

#### Package.json Script
```
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node server.js",
    "dev": "nodemon server.js"
  },
  
```

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

* controllers | \<variable-name\>.js | CRUD functions
<details>
 <summary>Expand Boilerplate</summary>

```
const Variable = require("../models/variable");
const db = require("../db/connection");

db.on("error", console.error.bind(console, "MongoDB connection error:"));

const getVariables = async (req, res) => {
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
  createVariable,
  getVariables,
  getVariable,
  updateVariable,
  deleteVariable,
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

module.exports = mongoose.model("variables", Variable);

```
</details>

* routes | \<variable-name\>.js | API endpoints
<details>
 <summary>Expand Boilerplate</summary>

```
const { Router } = require("express");
const controllers = require("../controllers/<filename>");

const router = Router();

router.get("/<variables>", controllers.getVariables);
router.get("/<variables>/:id", controllers.getVariable);
router.post("/<variables>", controllers.createVariable);
router.put("/<variables>/:id", controllers.updateVariable);
router.delete("/<variables>/:id", controllers.deleteVariable);

module.exports = router;

```
</details>

* seed | \<varibale-name\>.js | Data to populate initial database
<details>
 <summary>Expand Boilerplate</summary>
 
```
const db = require("../db/connection");
const Variable = require("../models/variable");

db.on("error", console.error.bind(console, "MongoDB connection error:"));

const main = async () => {
  const variables = [
    {
    }
    ];
  await Variable.insertMany(variables);
  console.log("Created vaiables!");
  };
const run = async () => {
await main();
db.close();
};

run();

```
</details>

*  N/A | server.js | Combined variables to set connection to database 

<details>
 <summary>Expand Boilerplate</summary>
   
   ```
const express = require("express");
const cors = require("cors");
const bodyParser = require("body-parser");
const logger = require("morgan");
const variableRoutes = require("./routes/variable");
const db = require("./db/connection");
const { response } = require("express");
const PORT = process.env.PORT || 3000;

const app = express();

app.use(cors());
app.use(bodyParser.json());
app.use(logger("dev"));

app.use("/api", variableRoutes);

db.on("error", console.error.bind(console, "MongoDB connection error:"));

app.listen(PORT, () => console.log(`Listening on port: ${PORT}`));

app.get("/", (req, res) => res.send("This is root!"));

```

</details>
    
