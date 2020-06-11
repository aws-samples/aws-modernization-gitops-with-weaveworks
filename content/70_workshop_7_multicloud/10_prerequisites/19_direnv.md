+++
title = "Install direnv"
chapter = false
weight = 19
+++

[direnv](https://direnv.net) takes care of exporting environment variables automatically based on the directory we're in.

To install direnv, run the following commands.

```sh
curl -sfL https://direnv.net/install.sh | bash
echo "eval '$(direnv hook bash)'" >> ~/.bashrc
source ~/.bashrc
```