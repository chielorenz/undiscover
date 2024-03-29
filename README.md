# Undiscover

Undiscover is my secret drawer, where I put my favorite songs. Or in other words the blog where sometimes I post some music that I particularly like, here a live version: [undiscover.it](https://undiscover.it).

## <a name="getting-started" />Getting started

Spin up the environment with:

```bash
docker-compose up
```

The local copy of the blog will be served on [http://localhost:3000](http://localhost:3000) and the cms admin page on [http://localhost:1337/admin](http://localhost:1337/admin) (use [default credentials](#credentials) to login).

### Credentials<a name="credentials" />

Default credential to access the [cms admin page](http://localhost:1337/admin) are:

- email: b1n01@undiscover.it
- password: B1n01@undiscover

## Overview

This monorepo holds three main components:

-   The blog frontend built with [Next.js](https://nextjs.org/)
-   The blog backend and database built with [Strapi](https://strapi.io/)
-   The dev environment handled by [Docker](https://www.docker.com/)

### Folder structures

```
| blog/                <-- Next.js app
| cms/                 <-- Strapi backend
| docker-compose.yml   <-- Docker compose
```

### Database

SQLite is used as database, and it's file is stored in `{REPO-PATH}/cms/database/data/data.db`. This file is committed in this repository, to drop all existing content (data and authentication) simply delete it it.

### Logs

To access logs use the docker built-in logs function:

```bash
docker-compose logs -f
```

### Images

Next.js handles images (and images optimization) only when it's server is running, but this repo only generates static content. In order to correctly serve and build the blog the `images.unoptimized` flag is set to true in `next.config.js` and the uploads folder from the Strapi container is mounted in the public folder on the Next.js docker container in `docker-compose.yml`.

## Deploy

To prepare the repo to be released run:

```bash
docker exec undiscover-blog yarn run publish
```

Now you can commit and push with:

```bash
git add blog/out && git commit -m "Build" && git push
```

## Fork this repo

In order to use this repo as template to build your own version, you need to delete SQLite database from `{REPO-PATH}/cms/database/data/data.db` and the Strapi uploads files from `{REPO-PATH}/cms/public/uploads/`:

```bash
rm ./cms/database/data/data.db
find ./cms/public/uploads/ -type f -not -name '.gitkeep' -delete
```

Then visit the [Strapi dashboard](http://localhost:1337) to create a new user. 

To deploy the repo use your favorite deployment platform and serve the `{REPO-PATH}/blog/out` folder. 
Enjoy 🎉.
