# Build a Rails project

To build a *Rails* project using this image, start by adding this to a `Dockerfile` on your project root :

## Create your project image

```dockerfile
FROM derniercri/rails:onbuild

# Optionnaly add your own Command here
CMD ["bash", "/home/user/init.sh"]
```

_Notes :
- the default behaviour is to simply launch a Puma server for your application ;
- I suggest you add a startup script to your project to reduce the complexity of your command ;
- as it runs inside a container, don't forget that your *command* should never ends._

Once done run :

```shell
docker build -t <your-app-name> --build-arg [RUBY_VERSION=2.3,RAILS_VERSION=4.0]
```

*Notes :
- either **RAILS_VERSION and **RUBY_VERSION** are optionnal and default respectively to 2.4 and 5.0.1 ;
- **RUBY_VERSION** should only indicate the standard version (eg: 2.3 not 2.3.3).*

## Run your image

Now you can simply run your image using the *docker CLI* : 
`docker run -d --name <your-app> <your-app-image>`

Or use `docker-compose`, especially if your app has some more dependencies. Here is a example 

```yml
version: '2'

services:
  app:
    container_name: app
    image: myApp
    ports:
      - "3000:3000"

  redis:
    image: redis
    restart: always
    container_name: redis

  database:
    image: postgres:9.6
    restart: always
    container_name: database
    env_file: .env
```

# Troobleshoots

If you encounter any issue by using this command, feel free to check the issues of this repository.

## Missing dependencies

It may miss some system dependencies to build your gems. In that case, create an issue giving those dependencies and I'll add them into the base image
