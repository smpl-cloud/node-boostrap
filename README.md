## Directory Layout

```bash
.
├── /build/                     # The compiled output (via Babel)
├── /config/                    # Configuration files (for Docker containers etc.)
├── /locales/                   # Localization resources (i18n)
├── /migrations/                # Database schema migrations
├── /seeds/                     # Scripts with reference/sample data
├── /src/                       # Node.js application source files
│   ├── /emails/                # Handlebar templates for sending transactional email
│   ├── /routes/                # Express routes, e.g. /login/facebook
│   ├── /schema/                # GraphQL schema, types, fields and mutations
│   │   ├── /Node.js            # Relay's "node" definitions
│   │   ├── /User.js            # User related top-level fields and mutations
│   │   ├── /UserType.js        # User type, representing a user account (id, emails, etc.)
│   │   ├── /...                # etc.
│   │   └── /index.js           # Exports GraphQL schema object
│   ├── /app.js                 # Express.js application
│   ├── /DataLoaders.js         # Data access utility for GraphQL /w batching and caching
│   ├── /db.js                  # Database access and connection pooling (via Knex)
│   ├── /email.js               # Client utility for sending transactional email
│   ├── /passport.js            # Passport.js authentication strategies
│   ├── /redis.js               # Redis client
│   └── /server.js              # Node.js server (entry point)
├── /test/                      # Unit, integration and load tests
├── /tools/                     # Build automation scripts and utilities
├── docker-compose.yml          # Defines Docker services, networks and volumes
├── docker-compose.override.yml # Overrides per developer environment (not under source control)
├── Dockerfile                  # Commands for building a Docker image for production
└── package.json                # The list of project dependencies
```

## Getting Started

```bash
docker-compose up               # Launch Docker containers with the Node.js API app running inside
yarn docker-db-seed             # Seed the database with some test data
```

localhost [http://localhost:8080/graphql](http://localhost:8080/graphql)

Once the Docker container named `api` is started, the Docker engine executes `node tools/run.js`
command that installs Node.js dependencies, migrates database schema to the latest version,
compiles Node.js app from source files (see [`src`](./src)) and launches it with "live reload"
on port `8080`.

In order to open a shell from inside the running "api" container, run:

```bash
docker-compose exec api /bin/sh
```

Similarly, if you need to open a PostgreSQL shell ([psql][psql]), execute this command:

```bash
docker-compose exec db psql <db> -U postgres
```

## Testing

```bash
yarn lint                       # Find problematic patterns in code
yarn check                      # Check source code for type errors
yarn docker-test                # Run unit tests once inside a Docker container
yarn docker-test-watch          # Run unit tests in watch mode inside a Docker container
```

## Deployment

Customize the deployment script found in `tools/publish.js` if needed. Then whenever you need to
deploy your app to a remote server simply run:

```bash
node tools/publish <host>       # where <host> is the name of your web server (see ~/.ssh/config)
```
