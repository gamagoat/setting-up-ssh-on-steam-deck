# Enable SSH Access on a Steam Deck

Desktop mode on the steam deck is a bit of a hassle.  It doesn't have to be.

## Preparing the Steam Deck

Swap to Desktop mode (Open the Steam menu, `Power > Switch to Desktop`

Open the Application Launcher and run `Konsole`

Create a password

```sh
passwd
```

Enable and start the ssh daemon

```sh
sudo systemctl enable --now ssh
```

Find the deck's IP address

```sh
ip addr | grep inet | grep wlan0
```

## Preparing your router

Access your router and assign a static IP to your steam deck

## Preparing your personal machine

Generate an SSH key pair

```sh
ssh-keygen -t ed25519
```

Install `ssh-copy-id` using your favorite package manager

```sh
brew install ssh-copy-id
```

Copy your public key to the steam deck

```sh
ssh-copy-id -i ~/.ssh/<identity>.pub deck@<steam deck ip>
```

Add the following entry to your `.ssh/config`

```txt
Host deck
  HostName <steam deck ip>
  User deck
  IdentityFile ~/.ssh/<identity>.pub
```

Connect

```
ssh deck
```
