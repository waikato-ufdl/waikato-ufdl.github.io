# Website for UFDL project

Generates the website at [ufdl.cms.waikato.ac.nz](https://ufdl.cms.waikato.ac.nz/)
using [Nikola](https://getnikola.com/).


## Requirements

* Nikola (>= 8.0.3)
* ghp-import2
* aiohttp
* watchdog


## Installation

* create virtual environment

  ```
  virtualenv -p /usr/bin/python3 venv
  ```

* install Nikola

  ```
  . venv/bin/activate
  pip install nikola -r requirements.txt
  ```

## Adding content

* [Nikola handbook](https://getnikola.com/handbook.html)
* Content is written in [reStructured Text](http://docutils.sourceforge.net/rst.html)
* Pages are located in `pages`
* News items (aka blog posts) are located in `posts`; should have a date prefix in the name


## Testing

You can test the changes locally by running:

```
./venv/bin/nikola auto
```


## Manually deploy to Github

Site gets automatically generated via [action](.github/workflows/main.yml). But you can manually deployed as follows:

* [general notes](https://pages.gitlab.io/nikola/stories/handbook/#deploying-to-github)
* commit/push changes
* run the following commands

  ```
  . venv/bin/activate
  git pull
  nikola clean
  nikola build
  nikola github_deploy
  ```

