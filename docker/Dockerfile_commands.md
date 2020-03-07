# Commands

[A good reference is here.](https://goinbigdata.com/docker-run-vs-cmd-vs-entrypoint/)

**CMD** and **ENTRYPOINT** commands should be formed in *EXEC* form.

- `FROM`: to use an existing image
- `MAINTAINER`: information about the owner
- `COPY`: copies files into the image
- `ADD`: it is the same like COPY but it can do more advance stuffs
- `RUN`: running for example shell commands
- `USER`: it specifies the user when running the image
- `WORKDIR`: specifies the working directory, multiple workdirs can be used. workdir is always relative to the previous working directory.
- `CMD`: this is the default command if the container was called without any parameters. Only one CMD command can be defined, or if there are many only the last one will be executed
- `ENTRYPOINT`: it wont be ignored like CMD. Using **CMD ["param1", "param2"]** serves as additional parameters to **ENTRYPOINT**.
