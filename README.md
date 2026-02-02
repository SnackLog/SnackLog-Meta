# SnackLog Meta Repository
This repository contains docker compose files to deploy the latest version of snacklog, and will allow you to build it from source.
All microservices are linked into this repo as submodules.

Clone the repo using `git clone --recursive` to clone it with submodules.

> [!CAUTION]
> Several parts of the docker compose files refer back to the exposed port 8080 and localhost. If you change the port, make sure you update ALL locations where this is the case. Otherwise, you may not be able to access the swagger UI.

## Required tooling

All required tooling is listed in the `flake.nix` file. To use it, [install nix](https://nixos.org/download/).
Once nix is installed, enable the optional features via `$XDG_CONFIG_DIR/nix/nix.conf`:

```
experimental-features = nix-command flakes
```

Finally, run `nix develop` in the repository. Nix should set up your environment for you, and get you ready to work with the repository.

## Initializing the open food facts database
Before you can use the products database, you need to import the data into the Database.
Do do so, download the latest export from open food facts [here](https://world.openfoodfacts.org/data).
Place the gz file under `./database-api-wrapper/openfoodfacts-mongodbdump.gz` and run `podman-compose up -d`.
MongoDB will now begin importing the data into the mongodb database. This process might take an hour or more, since it has to rebuild the entire index. Monitor progress via the logs of the mongodb container.

Once the import is completed, restart all containers via `podman-compose down` and `podman-compose up -d`.

## Swagger documentation
Swagger documentation is located at [http://localhost:8080/docs/](http://localhost:8080/docs/) and available once the containers are running.
