# template-fastapi-postgres

This is a template created to develop fastapi applications that connect to a postgres database.
The intention of local development is a devcontainer, using vscode; But you it should work with any editor/environment. In order to allow other IDEs to use it without vscode, the configuration happens in a specific [docker-compose.yml](.devcontainer/docker-compose.yml) file, and the image built is in the [Dockerfile](Dockerfile) at root level.

>**NOTE:** Some modification may be necessary to work with different setups

In this project you will see 3 containers running: 

- app: the container you are working on, and where your codebase is located
- db: a postgres database to connect locally
- adminer: a GUI to access the database (port 8080)

## packages used

- fastapi (API)
- SQLAlchemy (ORM)
- alembic (DB migrations)
- poetry (package management)
- uvicorn (server application)

## Quickstart

Create a new project using this as a template. Then, change the project name in [pyproject.toml](pyproject.toml) file. Rebuild the container, and everything should work fine. 

>**NOTE:** If something goes wrong when running this initial setup, try to update poetry packages and rebuild the container.

## Project Structure

> If you already are familiar with the structure, feel free to skip this section

Let's separate the content of this project in two: *code* and *infrastructure*.

Everything related to **code** is what you would expect in any python project:

- *.env* & *local.env*: environment variables you will need in your code
- *poetry.lock* & *pyproject.toml*: used by poetry to manage your packages
- *src/*: The source folder where you application sits. **Your code should be here**

Then you have **infrastructure**. Those files are needed to make everything work in codespaces, vscode, etc:

- *.devcontainer/*: folder that contain the files that allow codespaces and vscode to setup your environment inside a docker container.
- *.vscode/*: folder that contains the configuration for executing and debugging. This is a generic configuration, and you can modify it, if your project needs it. Remember this is shared amongst other developers, so do not add settings only valid for you.
- *Dockerfile*: is the configuration for your environment. This file is exposed so you can change your environment like updating the python version.

## Alembic

Alembic is installed, and it runs the initial migration, already prepared. In a new project, you should delete the existing migration, and create a new one based on your configuration. 

In alembic's [env.py](src/alembic/env.py) the target metadata was set based on user model. Change this, according to your needs. Alembic.ini was also modified, but you don't **need** to change it.

>**NOTE**: alembic configuration is based in the user model. If you remove this moodel, you have to change it in env.py as well

### quick help on migration

before migration, you need to create a new revision:

```bash
alembic revision -m "migration description"
```

This will create a new revision, based on your changes in the models.

To apply update the database to the latest migrations, use the following command:

```bash
alembic upgrade head
```

For more information about alembic, check the [official documentation](https://alembic.sqlalchemy.org/en/latest/tutorial.html)