# Welcome to MkDocs VINCE!

For full documentation visit [mkdocs.org](https://www.mkdocs.org).

## Commands

* `mkdocs new [dir-name]` - Create a new project.
* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs -h` - Print help message and exit.

## Project layout

    mkdocs.yml    # The configuration file.
    docs/
        index.md  # The documentation homepage.
        ...       # Other markdown pages, images and other files.

---
# Material theme

Make a change to docs/index.md, and publish the first version:

```
mike deploy --push --update-aliases 0.1 latest
```

Set the default version to latest

```
mike set-default --push latest
```

Now, make another change and publish a new version:

```
mike deploy --push --update-aliases 0.2 latest
```

