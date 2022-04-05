# powerline-configuration

Configuration of powerline using ansible

## Auto documentation with ansible-mdgen

https://murphypetercl.github.io/ansible-mdgen/getting-started/

example from https://fedoramagazine.org/add-power-terminal-powerline/

@TODO
steps to build
    sudo dnf install powerline powerline-fonts

The rest of these instructions assume you’re using Fedora’s standard bash shell. If you’re using a different shell, check out the documentation for tips.

Next, configure your bash shell to use powerline by default. Add the following snippet to your ~/.bashrc file:

if [ -f `which powerline-daemon` ]; then
  powerline-daemon -q
  POWERLINE_BASH_CONTINUATION=1
  POWERLINE_BASH_SELECT=1
  . /usr/share/powerline/bash/powerline.sh
fi

To activate the changes, open a new shell or terminal. You should have a terminal that looks like this:

@TODO https://powerline.readthedocs.io/en/latest/usage/shell-prompts.html
