# Learning Express Backend

<!--  -->

# Step1 : Setup Initial Project

- npm init
- touch readme.md
- mkdir public > temp
- touch .gitignore [form-gitignore-generator]
- touch .env
- create src > touch app.js constants.js index.js
- package.json > add ["type":"module"]
- npm i -D nodemon > package.json add ["scripts": {"dev": "nodemon src/index.js"},]
- src > mkdir controllers db middlewares models routes utils
- npm i -D prettier
- touch .prettierrc {"singleQuote": false,"bracketSpacing": true,"tabWidth": 2,"trailingComma": "es5","semi": true}
- touch .prettierignore [/.vscode /node_modules ./dist *.env .env .env.*]

<!--  -->

# Step 2: Connect Database in MERN with debugging

- mongodb atlas [shared db]
- create project in mongodb atlas
- create user in database access [password + readWriteAnyDB]
- create ip in network access
- goto project > connect > compass > getLink
- .env > PORT, MONGODB_URL+password
- constants.js > export const DB_NAME = "videotube";
- npm i dotenv mongoose express
- to connect db always use try/catch and async/await

# index.js -> type: all code inside index.js

<!-- import mongoose from "mongoose";
import { DB_NAME } from "./constants";

import express from "express";
const app = express();

// using IIFE
(async () => {
try {
    await mongoose.connect(`${process.env.MONGODB_URL}/${DB_NAME}`);
    app.on("error", (error) => {
    console.log("Error:", error);
    throw error;
});
    app.listen(process.env.PORT, () => {
      console.log(`App iis listening on port ${process.env.PORT}`);
    });
} catch (error) {
    console.error("ERROR:", error);
    throw error;
}
})(); -->

# cd db > touch index.js -> connectDB

<!-- import mongoose from "mongoose";
import { DB_NAME } from "../constants";

const connectDB = async () => {
try {
const connectionInstance = await mongoose.connect(
`${process.env.MONGODB_URL}/${DB_NAME}`
);

    console.log(
      `\nMongoDB  Connected !! DB HOST: ${connectionInstance.connection.host}`
    );

} catch (error) {
console.error("MongoDB Connection Error", error);
process.exit(1);
}
};

export default connectDB; -->

# index.js > dotnet config + connectDB

<!-- import dotenv from "dotenv";
import connectDB from "./db";

dotenv.config({
  path: "./env",
});

connectDB(); -->

- package.json > "dev": "nodemon -r dotenv/config --experimental-json-modules src/index.js"
- npm run dev

# Step 3: Custom API Response and Error Handeling

- app.js
<!-- import express from "express";
const app = express();
export { app }; -->

- index.js
<!-- connectDB()
  .then(() => {
    app.listen(process.env.PORT || 8000, () => {
      console.log(`Server is running at port ${process.env.PORT}`);
    });
  })
  .catch((err) => {
    console.log("MongoDB Connection Failed!!", err);
  }); -->

- express API DOCS [request.params || request.body || request.cookie]

# npm i cookie-parser cors

- use app.use when using middlewares
- app.js config.
<!-- 
import cookieParser from "cookie-parser";
import cors from "cors";

const app = express();

app.use(
cors({
origin: process.env.CORS_ORIGIN,
credentials: true,
})
);

app.use(express.json({ limit: "16kb" }));

app.use(express.urlencoded({ extended: true, limit: "16kb" }));

app.use(express.static("public"));

app.use(cookieParser());
-->

# utils > asyncHandler.js

<!--
const asyncHandler = (requestHandler) => {
  (req, res, next) => {
    Promise.resolve(requestHandler(req, res, next)).catch((error) =>
      next(error)
    );
  };
};

export { asyncHandler };
 -->

# utils > ApiError.js

<!--
class ApiError extends Error {
  constructor(
    statusCode,
    message = "Something went wrong",
    errors = [],
    stack = ""
  ) {
    super(message);
    this.statusCode = statusCode;
    this.data = null;
    this.message = message;
    this.success = false;
    this.errors = errors;

    if (stack) {
      this.stack = stack;
    } else {
      Error.captureStackTrace(this, this.constructor);
    }
  }
}

export { ApiError };

 -->

# utils > ApiResponse.js

<!--
 class ApiResponse {
  constructor(statusCode, data, message = "Success") {
    this.statusCode = statusCode;
    this.data = data;
    this.message = message;
    this.success = statusCode < 400;
  }
}
 -->

# User and Video model with hooks and JWT
- user.models.js and video.models.js
- npm i mongoose-aggregate-paginate-v2
- npm i bcryptjs
- npm i jsonwebtoken