# Installation
Here's how I did it (on Mac, Linus should be similar, sorry if you're on Windows):

- Follow the installation instructions [here](http://octopress.org/docs/setup/)
  - Use the rbenv option, - do so using [Homebrew](http://brew.sh/) if on Mac

> ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
>
> brew install rbenv node
>
> sudo gem install execjs

- Clone this project to your develpoment directory of choice.
- Navige to the folder.
- Install the dependencies using Bundler

> sudo gem install bundler
>
> rbenv rehash
>
> git checkout source
>
> bundler install

# Usage
See [here](http://octopress.org/docs/blogging/)
