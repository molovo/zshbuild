# zshbuild

A helper tool for building large ZSH projects from source

## Installation

### With [Zulu](https://zulu.sh)

```sh
zulu install zshbuild
```

### With [zplug](https://zplug.sh)

```sh
zplug 'molovo/zshbuild' \
    as:command, \
    use:zshbuild
```

### Manual Installation

```sh
git clone https://github.com/molovo/zshbuild
cd zshbuild
chmod u+x zshbuild
cp zshbuild /usr/local/bin
```

## Usage

At it's simplest, zshbuild needs to be passed a file to write to (`--out`) and a list of files to include as the source. zshbuild will search all files/globs passed to it, and concatenate them to the output file specified.

```sh
zshbuild --out my_app src/**/*.zsh lib/**/*.zsh
```

### Options

`zshbuild` can be passed a number of options to change the way the compiled script is built.

#### `--out <file>`

The path to the output file to be built.

```sh
zshbuild --out my_app src/**/*.zsh
```

#### `--sourcemap [<file>]`

When using the `--sourcemap` option, `zshbuild` will generate a sourcemap, mapping lines in the output executable to the correct file and line in the source. This can be used with tools like `crash` to ensure the location of functions in its output are displayed with the filename/line number in the source, rather than in the output executable.

By default, the generated sourcemap will be written to `<output>.map`.

```sh
# Writes sourcemap to my_app.map
zshbuild --sourcemap --out my_app src/**/*.zsh
```

To change the location of the sourcemap, you can pass a filename to the `--sourcemap` option.

```sh
# Writes sourcemap to my_source_map
zshbuild --sourcemap my_source_map --out my_app src/**/*.zsh
```

#### `--strip-blank`

Strips blank lines from the compiled executable.

```sh
zshbuild --strip-blank --out my_app src/**/*.zsh
```

#### `--strip-comments`

Strips comments from the compiled executable.

```sh
zshbuild --strip-comments --out my_app src/**/*.zsh
```

#### `--prepend`

Specifies text to be prepended to the start of the compiled executable

```sh
zshbuild --prepend '#!/usr/bin/env zsh' --out my_app src/**/*.zsh
```

#### `--prepend`

Specifies text to be appended to the end of the compiled executable

```sh
zshbuild --append '_my_app "$@"' --out my_app src/**/*.zsh
```

### Configuring zshbuild for your project

If your project is built with the same options each time, you can place a file called `.build.yml` in the root of your project, and settings will be read from this rather than having to specify the options. Here is an example `.build.yml`.

```yaml
prepend: '#!/usr/bin/env zsh'
append: '_my_app \"$@\"' # Double quotes must be escaped

include:
    - src/**/*.zsh
    - lib/**/*.zsh

strip_comments: true
strip_blank: true

out: my_app
sourcemap: my_app.zshbuildmap
```

Once you have written your config file, you can build your project by running `zshbuild` without any arguments. If you need to override one of the config values for a single run, you can do so by passing the corresponding option to `zshbuild`.

## Contributing

All contributions are welcome, and encouraged. Please read our [contribution guidelines](contributing.md) and [code of conduct](code-of-conduct.md) for more information.

## License

Copyright (c) 2017 James Dinsdale <hi@molovo.co> (molovo.co)

zshbuild is licensed under The MIT License (MIT)

## Team

* [James Dinsdale](http://molovo.co)
