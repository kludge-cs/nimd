# NIMD - Not In My Dependencies

NIMD is a modern solution to poor design. Often, at KCS we notice that a lot of
Node packages are actually highly overused in places they don't need to be, or
said far-too-popular packages are designed with software engineering prowess
comparable to cleaning your dirty dishes with a fire engine hose.

This is our solution. Welcome to the future, today. A better ecosystem for you,
your children, your packages, their child packages, and for everyone else too.

## Installation

Pour in with your package manager of choice, and just add flour.

```sh
# For NPM users:
$ npm i -D nimd
# For Yarn users:
$ yarn add -D nimd
```

And just like that, you have a freshly baked NIMD by your side. Magic, right?

## Usage

### A foreword

Note that NIMD works by forcing resolutions, which means it alters the lockfile
you provide for any package manager. The corollary of this is that NIMD does not
work in projects without lockfiles, and may also not work on lockfile formats
it does not yet recognize. Currently it operates on JSON based lockfiles, like
the ones used in Yarn or NPM.

### Setup

In your package manifest file (`package.json`):

```json
{
	...
	"nimd": {
		"package-name-1st": "other-package",
		"package-name-2nd": "package-tar-location",
		"package-name-3rd": "NIMD:SKIP"
	}
}
```

This showcases the three ways you can use NIMD.
1. Using NIMD to redirect one package to another. At KCS, we use this when we
rewrite packages to our own specification, in order to force our projects to use
our custom version.
2. Using NIMD to redirect one package to another, but via tarballs. Obviously,
NIMD has no idea where a replacement package is hosted, so it tries to resolve by
NPM; alas, it will not always be there. This usage allows you to specify the
location of any tarball compatible with your package manager.
3. Using NIMD to skip a dependency altogether. Writing `NIMD:SKIP` as the
resolution for any package will cause NIMD to bypass the dependency, causing it
to be ignored by package managers.

Usage #3 is inspired by a friend of our CEO, [Favna / @favware][favna]. Credit
to him for [the idea], and use his package if it's better for your use case
(Yarn v1 exclusive, and only skipping packages), however NIMD is designed for
more power and flexibility.

[favna]: https://github.com/favna
[the idea]: https://github.com/favware/skip-dependency

### Using NIMD - CLI

You can always run NIMD from any CLI, like so:

```sh
$ npx nimd <lockfile>
```

Note that without the `lockfile` argument, NIMD will automatically search for
a `yarn.lock` or a `package-lock.json`, and run with the corresponding inferred
format.

### Using NIMD - automatic

Now, if you're anything like us you may be thinking "Wait, but I don't want to
have to run NIMD every time the lockfile updates. That sounds like a pain!" -
and for you, we have a beautiful solution. Enter the `postinstall` script.

The `postinstall` script is supported by all major package managers ([Yarn],
[NPM], [PNPM], et cetera), and that's exactly how NIMD operates.

[Yarn]: https://classic.yarnpkg.com/en/docs/package-json/#toc-scripts
[NPM]: https://docs.npmjs.com/cli/v6/using-npm/scripts#npm-install
[PNPM]: https://pnpm.js.org/en/package_json

In your package manifest file (`package.json`):

```json
{
	...
	"scripts": {
		"postinstall": "npx nimd <lockfile>"
	}
}
```

That way, it will patch your lockfile for you every time you reinstall from or
regenerate the lockfile. Voila.

## Licensing

NIMD (Not In My Dependencies) is dedicated to the public domain. However, if
your country's copyright laws do not acknowledge the public domain for whatever
reason, it is also made available under the terms of the [MIT License].

Our only hope in releasing this project is it serves useful to whomever may find
it. Feel free to open an issue if you notice a bug, have any questions, or
require additional features.

[MIT License]: LICENSE
