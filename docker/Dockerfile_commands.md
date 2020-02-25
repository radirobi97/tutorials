# Commands

[A good reference is here.](https://goinbigdata.com/docker-run-vs-cmd-vs-entrypoint/)

**CMD** and **ENTRYPOINT** commands should be formed in *EXEC* form.

- `FROM`: to use an existing image
- `COPY`:
- `RUN`:
- `CMD`: this is the default command if the container was called without any parameters. Only one CMD command can be defined, or there are many only the last one will be executed
- `ENTRYPOINT`: it wont be ignored like CMD. Using **CMD ["param1", "param2"]** serves as additional parameters to **ENTRYPOINT**.
