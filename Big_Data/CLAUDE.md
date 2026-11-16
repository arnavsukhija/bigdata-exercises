# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Weekly exercise sheets for the **Big Data** course at ETH Zurich (Fall 2026). This directory is one half of a fork of `RumbleDB/bigdata-exercises`; the git root is the parent directory, which also contains `Big_Data_For_Engineers/` (the Spring course, same layout). Git paths are therefore prefixed with `Big_Data/`. Upstream changes arrive via merges from `RumbleDB:master`.

There is no build, lint, or test suite. The work is Jupyter notebooks run against services in Docker.

## Layout conventions

Each `exerciseNN/` folder is a self-contained week:
- `ExerciseNN_<Topic>.ipynb`: the sheet the student fills in. Edits normally go here.
- `ExerciseNN_<Topic>_Solution.ipynb`: the official solution. Treat it as reference and don't modify it.
- `ExerciseNN_<Topic>_Moodle.ipynb` (some weeks): a copy of the Moodle quiz questions.
- `docker-compose.yml` plus optional `jupyter/` or `jupyter_docker/` Dockerfiles and `docker-hadoop/` images, when the week needs its own services.

Weeks with no `docker-compose.yml` (e.g. 00, 01, 08, 09, 11) run inside the course-provided **ExamMagicBox** environment, which is downloaded separately and not part of this repo. Their notebooks assume its hostnames. For example, SQL notebooks connect with `%sql postgresql://postgres:example@db`.

Topics by week: 00 Jupyter/SQL intro, 01 SQL, 02 object storage/REST, 03 HDFS, 04 JSON/XML well-formedness, 05 HBase, 06 data models, 07 MapReduce (Hadoop), 08 Spark RDDs, 09 Spark SQL, 10 MongoDB, 11 JSONiq (RumbleDB), 12 Neo4j, 13 OLAP cubes. `JSONiq-Jupyter/` is supplementary JSONiq material.

## Running an exercise

From an exercise folder that has a `docker-compose.yml`:

```bash
docker compose up -d     # start services in the background
docker compose down      # stop and remove the containers
```

Jupyter is served at http://localhost:8888 with no token. The exercise folder is mounted at `/home/jovyan`, so edits in the browser are written straight to the repo files.

The root helper scripts are optional. `init-docker-env.sh` writes `HOST_UID`/`HOST_GID` to a gitignored `.env`. `source activate-docker-env.sh` aliases `docker-compose` to pass that file, and `deactivate-docker-env.sh` removes the alias. No compose file in this directory currently references those variables.

Exercise 05 has a `docker-compose-aarch64.yml` for Apple Silicon.

## Notebook kernels / magics in use

- SQL: `%reload_ext sql` then `%%sql` cells (ipython-sql).
- JSONiq: `from jsoniq import RumbleSession`, `%load_ext jsoniqmagic`, then `%%jsoniq` cells.
- Neo4j, MongoDB, and HBase are accessed from Python in the Jupyter container, with service hostnames taken from the week's compose file.

## RumbleDB troubleshooting (from README)

- `JSONDecodeError: Expecting value...` usually means the ETHZ proxy is interfering. Set `http_proxy`/`https_proxy`/`ftp_proxy` (and their uppercase forms) to `''` in `os.environ` at the top of the notebook.
- `RemoteDisconnected` usually means a Docker Engine/Desktop version conflict with cgroups. Update Docker.

## Editing notebooks

`.ipynb` files are JSON. Prefer the NotebookEdit tool over raw text edits, and avoid committing large cell outputs or `.ipynb_checkpoints/` (which are gitignored).
