# Learning Express Backend

<!-- steps to setup inital project -->

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

