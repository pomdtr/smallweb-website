# Getting started

## Why Smallweb ?

Smallweb is a new take on self-hosting. The goal is to make it as easy as possible to host your own apps. To achieve this, it uses a "file-based hosting" approach: every subfolder in your smallweb folder becomes a unique subdomain.

Smallweb is obsessed with scaling down. No dev server or a build is required, and you don't even need to care about deployment. You can just write a single file, hit save, and voila! Your app is live.

## Live Demo

A shared public instance of smallweb is available at [smallweb.club](https://smallweb.club). You can use it to test smallweb without setting up your own instance.

Please ping me either on [bluesky](https://bsky.app/profile/pomdtr.me) or [discord](https://discord.gg/BsgQK42qZe) if the demo is down.

## Local Installation

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
smallweb init smallweb.live
```

> [!INFO]
> `smallweb.live` is a magic domain that will always route request to `127.0.0.1` (localhost). You can also use [traefik.me](https://traefik.me) or [local.gd](https://local.gd).

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

### Generating tls certificates using mkcert

In order to get a proper TLS setup, you'll need to generate certificates for your domain.

The easiest way to do this is using [mkcert](https://github.com/FiloSottile/mkcert).

```sh
brew install mkcert
mkcert -install
mkcert -cert-file cert.pem -key-file key.pem smallweb.live '*.smallweb.live'
```

Then, you can use these certificates when starting smallweb:

```sh
smallweb up --addr :443 --tls-cert cert.pem --tls-key key.pem
```

Your apps will now be available at `https://<app>.smallweb.live`.

On linux, you'll have to allow smallweb to bind to port 443:

```sh
sudo setcap 'cap_net_bind_service=+ep' $(which smallweb)
```

### Using smallweb behind a reverse proxy

Another option is to use a reverse proxy like [caddy](https://caddyserver.com) as a reverse proxy on port 80 and 443.

```sh
brew install caddy && brew services start caddy
```

Here is an example `Caddyfile` routing `smallweb.live` to smallweb:

```txt
smallweb.live, *.smallweb.live {
  tls internal
  reverse_proxy localhost:7777
}
```

### Keep smallweb running in the background

To keep smallweb running in the background, you can use a process manager like [systemd](https://www.freedesktop.org/wiki/Software/systemd/) on Linux or [launchd](https://launchd.info/) on macOS.

#### Systemd

Here is an example systemd service file for smallweb.

```ini
[Unit]
Description=Smallweb
After=network.target

[Service]
Type=simple
ExecStart=/usr/local/bin/smallweb up
WorkingDirectory=/home/:user/smallweb
```

You should store this file as `~/.config/systemd/user/smallweb.service`.

Then, enable and start the service:

```sh
systemctl --user enable smallweb
systemctl --user start smallweb
```

#### Launchd

And here is an example launchd plist file for smallweb on macOS:

```xml
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
    <dict>
        <key>KeepAlive</key>
        <true />
        <key>Label</key>
        <string>com.github.pomdtr.smallweb</string>
        <key>LimitLoadToSessionType</key>
        <array>
            <string>Aqua</string>
            <string>Background</string>
            <string>LoginWindow</string>
            <string>StandardIO</string>
            <string>System</string>
        </array>
        <key>ProgramArguments</key>
        <array>
            <string>/opt/homebrew/bin/smallweb</string>
            <string>up</string>
        </array>
        <key>WorkingDirectory</key>
        <string>/Users/:user/smallweb</string>
        <key>RunAtLoad</key>
        <true />
        <key>StandardErrorPath</key>
        <string>/Users/pomdtr/Library/Logs/smallweb.log</string>
        <key>StandardOutPath</key>
        <string>/Users/pomdtr/Library/Logs/smallweb.log</string>
    </dict>
</plist>
```

You should store this file as `~/Library/LaunchAgents/com.github.pomdtr.smallweb.plist`.

Then, load the service:

```sh
launchctl load ~/Library/LaunchAgents/com.github.pomdtr.smallweb.plist
```
