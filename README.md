# Goodread

[![Version](https://img.shields.io/badge/Version-v0.3-orange.svg)](https://github.com/goodread/goodread#changelog)

Test runner for README.md. Support for Python/JavaScript/Ruby/PHP code blocks. Integration with hackmd.io.

## Motivation

Documentation should contain correct and up-to-date code examples. It's vital almost for every programming project. `goodread` address this problem validating documentation code blocks. Think Python's `doctest` for markdown. It's also suitable for:

- writing tutorials without annoying code copy-pasting
- writing self-testing technical requirement documents
- providing more light-weight alternative for IPython/Jupyter notebooks
- let's try Donald Knuth's [literate programming](https://en.wikipedia.org/wiki/Literate_programming) in markdown =)

## Features

- run tests/examples inside you markdown documentation and validate it
- support for Python, JavaScript, Ruby and PHP code blocks
- support for assertions using comments syntax
- integration with hackmd.io and other editors

---

#### Write documentation

![](https://i.imgur.com/LB5isUo.png)

---

#### Validate documentation

![](https://i.imgur.com/Dvk4Aec.png)

## Getting Started

> [**Security note**] Validate/sync remote documentation only if you trust the source. It's important to understand that `goodread` just runs the code like any other test runner.

### Installation

The packages use semantic versioning. It means that major versions could include breaking changes. It’s highly recommended to specify a package version range in your dependencies.

```bash
$ pip install goodread # Python
$ npm install goodread # JavaScript
$ gem install goodread # Ruby
$ composer require goodread/goodread # PHP
```

### Writing documentation

With `goodread` you just write your documentation as usual. You could start enabling `goodread` testing gragually from zero to full test coverage:
- adding `goodread` to your code block marker to enable code running:
![](https://i.imgur.com/orTqjwK.png)
- adding an inline comment to provide an assertion:
![](https://i.imgur.com/OfdFwFT.png)

That's it. These code blocks will be run and tested against provided assertions. Use `python/javascript/ruby/php` for codeblocks and corresponding comment syntax for assertions.



### Validating documentation

Now we have installed `goodread` CLI and have some documentation written. Let's validate code blocks inside it:

```bash
$ goodread-py README.md

 #  tableschema-py

➖➖➖

 ##  Import

 ✔  import tableschema

 ##  Example

 ✔  field = tableschema.Field({'name': 'name', 'type': 'integer'})
 ✔  field.type # 'integer'
 ✔  field.cast_value('1') # 1
 ❌  field.cast_value('2') # 4
Exception: field.cast_value('2') != 4

 ❌  tableschema-py: 4/5
```

Under the good `goodread` parses a markdown document looking for code blocks itself to test. It doesn't split every individual code block so there are no limitation for target language usage. Also it preserves a scope between different code blocks allowing to use all variables declared above. For languages like JavaScript `goodread` handles `async/await` to provide ability of writing documentation without function wrappers.

### Configuring goodread.yml

The `goodread` CLI could accept multiple input documents including remote ones. Also if a path is not provided at all it will validate `README.md` by default:

```bash
$ goodread-py # validate README.md
$ goodread-py http://example.com/doc.md # validate remote
$ goodread-py docs/*.md # validate all docs in a folder
```

But it's also possible to provide a configuration file called `goodread.yml` with a documentation map:

> goodread.yml

```yml
documents:
  - README.md
  - docs/tutorial1.md
  - docs/tutorial2.md
```

So now simple `goodread` call will validation all specified documents:

```bash
$ goodread # validate documents from goodread.yml
```

### Integrating with hackmd.io

Probably many of us wonder how we could make documentation writing process smoother. So `goodread` not only validates documentation it also helps to improve general documentation writing experience.

Let's extend our `goodread.yml`:

> goodread.yml

```yml
documents:
  - main: README.md
    edit: https://hackmd.io/hash===?both
    sync: https://hackmd.io/hash/download
  - docs/tutorial1.md
  - docs/tutorial2.md
```

Now we could use `goodread -e/--edit` command:

```bash
$ goodread-py -e README.md # will open hackmd doc in the browser
```

After editing it using fancy hackmd.io UI we could validate it and save locally with `goodread -s/--sync` command:

```bash
$ goodread-py -s README.md # validate hackmd doc and save as README.md
```

As you could see in the config you could any other online editor service like stackedit.io or dillinger.io if it provides markdown export for documents.

## Alternatives

I wasn't able to find solutions coverting all `goodread` core features esp. multilanguage support. But there are some cool projects with the same goal:

- [markdown-doctest-js](https://github.com/Widdershin/markdown-doctest) - Test all the code in your markdown docs! As an open source developer, there are few things more embarassing than a user opening an issue to inform you that your README example is broken! With markdown-doctest, you can rest easy knowing that your example code is actually runnable.

## Contributing

This project uses consolidated issue tracker and development model. Please use this repository for everything except test runners code.

### Python

Requirements:
- installed `virtualenv` - https://virtualenv.pypa.io/en/stable/installation/

```bash
git clone git@github.com:goodread/goodread-py.git
cd goodread-py
virtualenv .python -ppython3.5
source .python/bin/activate
make install
make test
```

### JavaScript

Requirements:
- installed `nvm` - https://github.com/creationix/nvm#installation

```bash
git clone git@github.com:goodread/goodread-js.git
cd goodread-js
nvm install 8
nvm use 8
npm install
npm test
```

### Ruby

Requirements:
- installed `rvm` - https://rvm.io/rvm/install

```bash
git clone git@github.com:packspec/goodread-rb.git
cd goodread-rb
rvm install 2.4
rvm use 2.4
gem install bundler
./bin/setup
rake spec
```

### PHP

Requirements:
- installed `phpbrew` - http://phpbrew.github.io/phpbrew/

```bash
git clone git@github.com:goodread/goodread-php.git
cd goodread-php
phpbrew install 7.1.0
phpbrew use 7.1.0
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('SHA384', 'composer-setup.php') === '544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php --filename=composer
php -r "unlink('composer-setup.php');"
php composer install
php composer test
```

## Changelog

Here described only breaking and the most important changes. The full changelog and documentation for all released versions could be found in nicely formatted [commit history](https://github.com/goodread/goodread/commits/master).

### v0.3

Proof of concept version of the specification and test runners.
