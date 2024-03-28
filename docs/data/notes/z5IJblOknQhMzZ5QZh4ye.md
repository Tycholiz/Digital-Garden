
### Services
Homebrew has a services aspect to it. We can run `brew services list` to see the services that are currently running from Homebrew. We can run `brew services start` to start a service.
- ex. Redis, Postgres, vault, caddyserver 

### Cask
brew cask is an extension to brew that allows management of MacOS graphical applications

### Tap
a Git repository of Formulae and/or commands

When you *tap* a repository, you are essentially informing Homebrew about the existence of this external repository so that it can be used to install packages or formulas from that repository.
- Homebrew is designed to be modular and lightweight. By default, it doesn't come with every package or formula available. Instead, it provides the core functionality and allows users to extend it by tapping additional repositories.

### Keg
the installation prefix of a Formula

* * *

### Installation directories
`/opt/homebrew/Cellar/`

## CLI
#### Search available packages
`brew search _____`

#### See where package is installed
`brew info postgresql`