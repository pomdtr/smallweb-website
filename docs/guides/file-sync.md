# Syncing files using mutagen

I recommend using [mutagen](https://mutagen.io) to sync your files between your development machine and your remote server.

## Installation

First, install the required tools on your development machine using Homebrew:

```sh
brew install mutagen-io/mutagen/mutagen
```

# Start the syncing process

```sh
mkdir ~/smallweb && cd ~/smallweb
```

Then, create the `~/smallweb/.mutagen.yml` file with the following content:

```yaml
sync:
  defaults:
    mode: two-way-resolved
    ignore:
      paths:
        - node_modules
        - .DS_Store
  example:
    alpha: <remote>:<remote-path>
    beta: <local-path>
```

For example, to sync the directories storing every apps running under the `smallweb.run` and `pomdtr.me` domains, I use:

```yaml
sync:
  defaults:
    mode: "two-way-resolved"
    ignore:
      paths:
        - node_modules
        - .DS_Store
  pomdtr-me:
    alpha: vps:/home/pomdtr/smallweb/pomdtr.me
    beta: ./pomdtr.me
  smallweb-run:
    alpha: vps:/home/pomdtr/smallweb/smallweb.run
    beta: ./smallweb.run
```

Then, to start the sync session, run the following command:

```sh
mutagen project start
```

From now on, each time you make a change to your files, they will be automatically synced to the server, and vice-versa.

## Resume the sync session on reboot

To ensure that the sync session resumes automatically after a reboot, you can use the following command:

```sh
mutagen daemon register
```

## Diagnosing sync issues

If you encounter any issues with the sync, you can check the errors using the following command:

```sh
mutagen project list --long
```
