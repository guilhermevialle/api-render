# Deploy Node.js on Render 

## Setup project
Firstly we gonna setup project with basic concepts.

- Create a new project and start with node `mkdir my-project && cd my-project && npm init -y`
- Now start git inside directory using `git init`
- Create a new GitHub repository to upload project

## Inside directory
Now we have to configure Docker to make container of this application. Make sure you have Docker installed on your PC.

- Inside root directory you gonna create `Dockerfile` using `touch Dockerfile` or `New-File Dockerfile` on windows
- Create a `.dockerignore` too and add the following code: 

``` .dockerignore
node_modules
npm-debug.log
.env
```
- Now create `.gitignore` and add the following code:
``` .gitignore
node_modules
.env
```
## Creating Express server 
Now let's create an express server inside `src/index.js` and put in an environment variable port.

``` javascript
const  express  =  require('express')
const  axios  =  require('axios')

const  port  =  process.env.PORT  ||  9090
const  server  =  express()

server.get('/',  async  (req,  res)  =>  {

try  {
  const  {  data  }  =  await  axios.get('https://jsonplaceholder.typicode.com/todos')

if (!data) return  res.status(404).send('Data not found')
  return  res.status(200).send(data)

}  catch (error) {
  return  res.status(500).send('Server error at handling at getting data')

  }

})

server.listen(port,  ()  =>  console.log(`http://localhost:${port}`))
```

## Creating container
Now we need to generate the image of our application with docker. Remember that Dockerfile? we will configure it now.

In the configuration of`package.json` we need to create `start` script command to start server. Paste the following script:

```javascript
{
  "name":  "api-render",
  "version":  "1.0.0",
  "description":  "",
  "main":  "src/index.js",
  "scripts":  {
   // This command will start the application server
   ====> "start":  "node src/index.js" 
   
},
  "keywords":  [],
  "author":  "",
  "license":  "ISC",
  "dependencies":  {
     "axios":  "^1.3.5",
     "express":  "^4.18.2"
  }
}
```

Now based on all the configurations we make, we will configure the `Dockerfile`. Copy and paste the following code:


```Dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
```

## Testing
Now the next step is to test and see if it's working. Inside root directory start the app:
1. `npm start` or `npm run start`
2.  Open browser in `http://localhost:9090`


## Upload on Github
Now that it worked let's upload the files on github. Open terminal and use git to upload files:

1. `git commit -m 'Creating and setup files for Docker and Server...'`
2.  `git push -u origin main`


## Build and deploy
It's almost over, now let's build the application using docker
