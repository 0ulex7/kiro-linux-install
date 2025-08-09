# Install tarball version of Kiro with just a basic script!

A script to very easily download and install the latest version of Kiro the glorious tarball way on your Linux machine!

Note: This installation script is by no means affiliated with Kiro project, or its maintainers.

Note: This project does not grant you access to Kiro escaping queue, if you got no signup code, you won't be able to register/login. Or does not give you any access that you didn't pay for. This script is basically just for installing Kiro using public resources to make tarball way a breeze for non-Debian/Ubuntu using GNU/Linux enjoyers!

## Prerequisites

* [git](https://git-scm.com/)
* [curl](https://github.com/curl/curl)

## Usage

Clone the repo and run the script
```bash

git clone https://github.com/spookyorange/kiro-linux-install.git
cd kiro-linux-install
sh ./install.sh

```

To remove the application(if it has been installed with this method)
```bash

sh ./uninstall.sh

```

Updates are handled by running the install script again, you can just run the install script and all good!
```bash

sh ./install.sh

```

## Details

The script will install the application in the following destinations if you have installed locally:

- ~/.tarball-installations/kiro
- ~/.local/bin/kiro
- ~/.local/bin/kiro-enhanced
- ~/.local/share/applications/kiro.desktop

+ any places Kiro may have set as temporarily usable space, or just generally usable space in general.

## Tested Distros

- Fedora by Spookyorange

## Contributing

If you have a distro that you would like to add to the list of tested distros, please submit a pull request with the changes you made to the script and the distro you tested it on. If you find any bugs or problems, you can open an issue; or if you want to get your hands dirty, open up a PR!
