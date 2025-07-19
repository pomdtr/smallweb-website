# Getting started

## Why Smallweb ?

Smallweb is a new take on self-hosting. The goal is to make it as easy as possible to host your own apps. To achieve this, it uses a "file-based hosting" approach: every subfolder in your smallweb folder becomes a unique subdomain.

Smallweb is obsessed with scaling down. No dev server or a build is required, and you don't even need to care about deployment. You can just write a single file, hit save, and voila! Your app is live.

## Live Demo

A shared public instance of smallweb is available at [demo.smallweb.live](https://demo.smallweb.live). You can use it to test smallweb without setting up your own instance.

Please ping me either on [bluesky](https://bsky.app/profile/pomdtr.me) or [discord](https://discord.gg/BsgQK42qZe) if the demo is down.

## Installation

First, you'll need to [install Deno](https://docs.deno.com/runtime/getting_started/installation/). I'll use the curl command here:

```sh
curl -fsSL https://deno.land/install.sh | sh
```

Then, install smallweb using one of the following methods:

```sh
# use the install script
curl -fsSL https://install.smallweb.run | sh

# brew (macOS/Linux)
brew install pomdtr/tap/smallweb

# scoop (Windows)
scoop bucket add pomdtr https://github.com/pomdtr/scoop-bucket.git
scoop install pomdtr/smallweb
```

Next, create your smallweb directory (I'll use `~/smallweb`), and initialize it:

```sh
mkdir ~/smallweb && cd ~/smallweb
smallweb init
```

Now you can start smallweb:

```sh
smallweb up
```

And access your apps at `http://<app>.smallweb.live:7777`.

You'll have two apps available by default: `www` and `example`. You can access them at `http://www.smallweb.live:7777` and `http://example.smallweb.live:7777`.

If you want to be able to omit the port from the URL, you'll need to use the `:80` address:

```sh
# your app are now available at http://<app>.smallweb.live
smallweb up --addr :80
```

On linux, you'll have to allow smallweb to bind to port 80:

```sh
sudo setcap 'cap_net_bind_service=+ep' $(which smallweb)
```

## Generating tls certificates using mkcert

In order to get a proper TLS setup, you'll need to generate certificates for your domain.

The easiest way to do this is using [mkcert](https://github.com/FiloSottile/mkcert).

```sh
brew install mkcert
mkcert -install
mkcert -cert-file smallweb.pem -key-file smallweb-key.pem smallweb.live "*.smallweb.live"
```

Then, you can use these certificates when starting smallweb:

```sh
smallweb up --addr :443 --tls-cert smallweb.pem --tls-key smallweb-key.pem
```

Your apps will now be available at `https://<app>.smallweb.live`.

On linux, you'll have to allow smallweb to bind to port 443:

```sh
sudo setcap 'cap_net_bind_service=+ep' $(which smallweb)
```

## Using smallweb behind a reverse proxy

Another option is to use a reverse proxy lik [caddy](https://caddyserver.com) as a reverse proxy for smallweb. Here is an example `Caddyfile` (assuming you arleady generated the certificates using mkcert):

```txt
smallweb.live, *.smallweb.live {
  tls /path/to/smallweb.pem /path/to/smallweb-key.pem
  reverse_proxy localhost:7777
}
```
