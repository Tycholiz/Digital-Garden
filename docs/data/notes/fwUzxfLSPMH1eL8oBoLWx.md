
Using Docker, we can abstract away the software, OS, and hardware configuration from our application, turning each service into a building block that we can run anywhere
- when using containers you have to always think of dynamic vs. static parts of your application. You can use your host's file system to store your data and files. A more scalable and efficient way of thinking about this is to store your data to amazon rds and your files to amazon s3. This way you can spin up as many containers of your app/site as you want and have them all point to a single place where they store your dynamic stuff; namely, your data and files.

## .dockerignore
- Because being lean is a design principle of Docker, it is important to cut out the stuff from the image that is not necessary to running the code.
	- This includes git files, travis.yml, .vscode etc. To ensure these files do not become a part of the image, we put these in a .dockerignore file.

```txt
# Ignore everything
**

# Allow some
!/src/**
!/jest.config.js
!/.jest/
!/.eslintrc.json
!/tsconfig.json
!/package*.json
!/.prettier*
!/.npmrc
```