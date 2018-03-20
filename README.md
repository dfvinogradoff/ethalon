# Ethalon
ETL/ELT PHP Processor Framework

## Overview
The idea of Ethalon is deadly simple: make somethink and send result of work to the next action (or "job" in Ethalon-terminalogy). It's like a pipeline architecture, or Unix-way:

```
$ job1 | job2 | job3 > result.csv
```
There is a couple of examples of uses

- We need update our address database every day by shedule
- We need drop and recreate our database if it has an errors after update (sad but true ☹️)
- We need update our database, but we don't needed download it again (our unpacked data available locally)

With Ethalone Framework we can describe all of this cases with simple config:

```
stages:
    create:
        - extractors.full
        - extractors.unarchive
        - transformers.default
        - loaders.createSchema
        - loaders.postgres
    recreate:
        - loaders.dropSchema
        - loaders.createSchema
        - loaders.postgres
    update:
        - { action: extractors.delta, arguments: { mode: 'local' } }
        - loaders.postgres
```
