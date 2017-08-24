
![aliquam rwd](/demo.jpg)

# aliquam

Want example? [check this out](http://grrinchas.github.io/aliquam/)

Want real life example? [check this out](http://grrinchas.github.io/)



## Table of Contents

- [Features](#features)
- [Installation](#installation)
- [Configuration](#configuration)
- [License](#license)



## <a name="features"></a>Features

- [Disqus](https://disqus.com/) comment system
- Google analytics
- Pagination support
- Custom tags and categories
- SEO support
- Contact form integration
- MathJax support

## <a name="installation"></a>Installation

#### Method 1: new master's repository (The Best)
1. [Pre Requisit](#prerequisit)
2. [See configuration](#configuration).

## <a name="prerequisit"></a>Pre Requisit

1. Configurate Ruby on Rails(Ubuntu):
	1. Install some dependencies for Ruby:
	1. ```
	sudo apt-get update
sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libffi-dev nodejs
	```
## <a name="configuration"></a>Configuration

All configuration is done via `_config.yml` file which you will find in your main repo folder.

- `url: "https://<yourName>.github.io"` - Change this to your domain (need for Disqus integration). !NOTE if running locally change this to `url: "https://localhost:4000"`.
- `disqus: "<disqusName>"` - Your disqus name. First you have to create account with [Disqus](https://disqus.com/).
- `owner: "<name surname>"` - Change this to your own name, need for copyright in the footer.
- `title: "<title>"` and `subtitle: "<subtitle>"` Change to your preferred title/subtitle.
- `baseurl: "/<branchName>"` - Change this to your branch name where _gh-pages_ resides. !NOTE apply only if you used __Method 2__ for installation.
- `google_analytics: <your_ID>` - Change this to your google analytics ID.
- `contact_PK: <yourPublicKey>` - Change this to your [http://getsimpleform.com](http://getsimpleform.com) public key.
- `math: true` - set `false` to disable maths support. For more information check [MathJax](https://www.mathjax.org/).

To get your public key:

1. Got to [http://getsimpleform.com](http://getsimpleform.com).
2. Enter your email and press `Send me a token`.
3. Go to your email and copy your token `Your form api token is <token>`. !NOTE do not copy your private key.
4. Paste this key to `contact_PK` in `_config.yml`.
5. Every time when someone sends you a message you will receive a notification to your email.

## <a name="license"></a>License

This project is licensed under the MIT License - see [The MIT License (MIT)](https://opensource.org/licenses/MIT)
for more details.

#Thanks for  [this guy](https://github.com/grrinchas/aliquam/fork).
