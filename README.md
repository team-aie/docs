## Welcome to aie

`aie` is a tool to help voicebank creators record voice samples for their
favorite voice synthesizer. Simply create a project, select a recording list
(reclist) and get started!

### Downloads

Navigate to the releases page: https://github.com/team-aie/app/releases/latest,
then pick the right installer/binary:

* `Windows`: Download `aie-setup-{version}.exe`.
* `macOS`: Download `aie-{version}.dmg`.
* `Linux`: Download `aie.AppImage`.

To install:

* `Windows`: Double click the installer and follow the instructions.
* `macOS`: Double click the `dmg` file, and drag `aie` into the `Applications`

  folder.

* `Linux`:
    - Simplest way: run `chmod +x aie.AppImage` and run `./aie.AppImage`.
    - Better way: Use

      [ `AppImageLauncher` ](https://github.com/TheAssassin/AppImageLauncher)

    or [ `appimaged` ](https://github.com/AppImage/appimaged).

### Development

Welcome aboard! Please follow the [setup instructions](/development-setup.md) to
set up your environment. If you run into any issues during development, take a
look at the [troubleshooting page](/development-troubleshooting.md). Us the
[resources](/resources.md) page for reference on background information and
library documentations.

#### Auto-formatting Markdown files

In all `aie` repos, use the VSCode settings that came with the repo to configure
your formatter. There are two extensions involved:

* `mervin.markdown-formatter`
* `stkb.rewrap`

`markdown-formatter` provides formatting for the Markdown files, but it does not
hard wrap lines (insert new lines at the end of a line) at a given line
width/print width. `Rewrap` helps with that. Because `markdown-formatter` will
interpret a new line as an intention to start a new paragraph, always run
`markdown-formatter` first (with your file auto-format key combinations) and
then `Rewrap` (by default `Alt + Q` or `Option + Q` ).

If you need to modify some texts, you will want to run `Rewrap` on that block of
text, and run `markdown-formatter` on non-text changes to prevent
`markdown-formatter` from converting new lines into new paragraphs.
