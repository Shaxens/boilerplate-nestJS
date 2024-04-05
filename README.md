# NestJS Boilerplate

This project is a boilerplate to start using NestJS in a prod environment.

It will setup a NestJS backend with a connection to the database (mongo).
A proxy is already set with nginx and has a loadbalancing plus it load static files.

# Required

 - Docker
 - 16 Go RAM (32 Go are highly recommanded)
 - Patience

# How to run

⚠️Don't forget to set your variables in the .env file (use the template)⚠️

Simply use this command to run the project :

```bash
docker compose up -d --build
```

It will set you a single backend app, if you need to scale your backend use this command instead :

```bash
docker compose up -d --build --scale app=3
```
