# Dominion Divider Generation

![Tests](https://github.com/github/docs/actions/workflows/lint_and_test.yml/badge.svg)

## Introduction

This is a script and library to generate card dividers for storing cards for the game [Dominion](https://boardgamegeek.com/boardgame/36218/dominion). If you are just looking go generate some dominion dividers, there is no need to install this script as I host a [live version of this generator code](http://domtabs.sandflea.org). However, if you want to use arguments that I don't expose on that page, or change the code, or contribute to the project the full generation code (not the web interface or the fonts) is included here, and contributions are more than welcome.

Again, to generate tabs go to the **[Online Generator](http://domtabs.sandflea.org)**.

## Installation

If you do need to install the package locally (the script provides a lot more options than the web-based generator), a simple `pip install domdiv` should suffice, providing a command by the name of `dominion_dividers`. However, see the note under Prerequisites->Fonts below as the default install will fall back on a font that doesn't match the cards (though most people don't notice). Run `dominion_dividers <outfile>` to get a pdf of all dividers with the default options, or run `dominion_dividers --help` to see the (extensive) list of options.

## Documentation

The script has an extensive set of options that are relatively well documented via `dominion_dividers --help`. Some are hard to describe unless you see output samples, so we recommend running the script with various options to see which configuration you like. The help output is replicated [here](https://github.com/sumpfork/dominiontabs/wiki/Documentation-%28Script-Options%29) for reference.

## Translations

When changing any of the [card database files](card_db_src) you should run the language update tool via `doit update_languages`. This produces [the package version of the card db](src/domdiv/card_db) from the card db source. This will also be run automatically and checked into git when you push to github. You should make sure that the resulting changes to the package are what you intend.

If you would like to help with translations to new (or updating existing) languages, please see [instructions here](src/domdiv/card_db/translation.md).

## Fonts

I believe I cannot distribute one font (Minion Pro) domdiv uses as they are owned by Adobe with a License that I understand to disallow redistribution.

However, it appears you can download them here:

- http://fontsgeek.com/fonts/Minion-Pro-Regular
- http://fontsgeek.com/fonts/Minion-Pro-Italic
- http://fontsgeek.com/fonts/Minion-Pro-Bold

Alternatively, the font files are included with every install of the free Adobe Reader. For example, on Windows 7 they are in C:\Program Files (x86)\Adobe\Reader 9.0\Resource\Font called `MinionPro-Regular.otf`, `MinionPro-Bold.otf` and `MinionPro-It.otf`.

Sadly, all these fonts use features that are not support by the reportlab package. Thus, they need to first be converted to ttf (TrueType) format. I used the open source package fontforge to do the conversion. Included as 'convert.ff' is a script for fontforge to do the conversion, on Mac OS X with fontforge installed through macports or homebrew you can just run `./convert.ff MinionPro-Regular.otf`, `./convert.ff MinionPro-Bold.otf` and `./convert.ff MinionPro-It.otf`. With other fontforge installations, you'll need to change the first line of convert.ff to point to your fontforge executable. I have not done this step under Windows - I imagine it may be possible with a cygwin install of fontforge or some such method.

Copy the converted `.ttf` files to the `fonts` directory in the `domdiv` package/directory, then perform the package install below.

## Using as a library

The library will be installed as `domdiv` with the main entry point being `domdiv.main.generate(options)`. It takes a `Namespace` of options as generated by python's `argparser` module. You can either use `domdiv.main.parse_opts(cmdline_args)` to get such an object by passing in a list of command line options (like `sys.argv`), or directly create an appropriate object by assigning the correct values to its attributes, starting from an empty class or an actual argparse `Namespace` object.

## Developing

Install requirements via `pip install -r requirements.txt`. Then, run `pre-commit install`. You can use `python setup.py develop` to install the `dominion_dividers` script so that it calls your checked out code, enabling you to run edited code without having to perform an install every time.

Feel free to comment on boardgamegeek at <https://boardgamegeek.com/thread/926575/web-page-generate-tabbed-dividers> or file issues on github (<https://github.com/sumpfork/dominiontabs/issues>).

Tests can be run (and their dependencies installed) via `python setup.py test`, which will also happen if/when you push a branch or make a PR.
