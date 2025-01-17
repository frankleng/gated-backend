# Gated Backend
Backend API for Gated.


# Overview
## Runtime Dependencies
The following services are required at runtime:

1. PostgreSQL Database
2. Google pub/sub service
3. Redis
4. Google API
5. SendGrid (not required for dev)

---
# Development
## Setup
### Prerequisites 
1. node version >= 16.0
2. docker version >= 20.10
3. GCP Project
    - The project may need to be created for you. See [this document](gcp-setup/README.md) for additional GCP project configuration requirements.


### Installation

`npm install`

### Configuration

1. `cp ./.env.example ./.env`
2. edit the `.env` file and populate values for:
    - `{name}`: use the personal name value used to name the GCP project created for you. i.e. copy `{name}` from `gated-dev-{name}`
    - `{populate}`: various values that include keys from the associated GCP project
    - refer to the [GCP Product Configuration](https://www.notion.so/gated/env-Variables-ae079cf9f2ab433f83614eab09dbb1c7) document for specific values to set in the `.env` file.

3. Add `AUTH_ADMIN_DOMAIN` to `.env` and set to the email domain of a test user. For example `AUTH_ADMIN_DOMAIN=gated.com`
    - when signing up with the test account, the account should be assigned as an admin. This is particularly useful for using the admin client.

4. Set value for `AUTH_FIREBASE_API_KEY`. This should come from the 'Browser Key' generated by Firebase under [API Keys](https://console.cloud.google.com/apis/credentials).

5. Set `GCLOUD_PROJECT` to your GCP project name

### Runtime Dependencies
Services for local development are defined in a docker compose configuration `docker/docker-compose.yaml`.


#### Initialization
When starting for the first time, run the init task.

```
npm run dev:init
```

#### Starting
```
npm run dev:up
```

#### PostgreSQL
By default, the postgresql service defined in the docker compose configuration mounts `docker/docker-volumes/data/pg` to the container so that data is persisted.

#### Google Pub/Sub
A containerized version of a Google pub/sub service is defined in the docker compose configuration. This version is considered an emulator and does not support the same level of functionality as the actual cloud service. Using the emulator is faster and more convenient for development purposes, but you'll need to use the actual service if you want to work with full email functionality. 

You can control which version of this service is used via the env var:

`PUBSUB_EMULATOR_HOST=localhost:8538`

### Database

1. `npm run db:migrate` to create the schema
2. `npm run db:seed` to initialize some seed data

## Running the app
```bash
# development
$ npm run start

# watch mode
$ npm run start:dev

# production mode
$ npm run start:prod
```

## Testing

### Setup
The database needs to be seeded before running tests. This should
generally only be required once.

```
$ npm run db:seed
```

### Running tests
```bash
# unit tests
$ npm run test

# e2e tests
$ npm run test:e2e

# test coverage
$ npm run test:cov
```
