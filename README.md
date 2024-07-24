# Enable SSH Access on a Steam Deck

Performing maintenance on a steam deck through Desktop mode is a bit of a hassle.
This guide helps you avoid that hassle by setting up ssh access to your deck.

## Preparing the Steam deck

Unfortunately, we need to use Desktop mode a bit to get ssh access working.

Swap to Desktop mode (Open the Steam menu, `Power > Switch to Desktop`)

Open the Application Launcher and run `Konsole`

Create a password:

```sh
passwd
```

Enable and start the ssh daemon:

```sh
sudo systemctl enable --now ssh
```

## Set a static IP for your steam deck (optional)

I prefer my local devices to be accessible by hostname or a static IP.  There
are issues with dns resolution of the steam deck that I haven't completed
troubleshooting yet, so we'll go the static IP route for now.

### Finding your Deck's MAC address

The simplest way to do this is from your deck in Game mode.  You can browse to
`Settings > Internet`, highlight the network you're connected to, and hit `Info`
(the `A` button by default).  Your MAC address will be listed.

### Assigning a static IP based on your MAC address

Typically, you'll access your router via a browser using an address like:

- `10.0.0.1`
- `192.168.0.1`
- `192.168.1.1`

After authenticating, you should look for a way to reserve IP addresses.  You
may want to lookup instructions on how to do this for your particular router.
Note that it may be under advanced setup options.  I use a NetGear router,
and can reserve addresses at `Advanced Setup > Setup > LAN Setup > Address Reservation`.

Add a new entry, giving a static IP to the MAC address you found earlier.

Reset your steam deck.

### Troubleshooting static IP

Steam Deck networking can be quirkier than other devices.  I'll add some useful
materials for troubleshooting static IP issues here as I come across them.

- [already_april's inconsistent wifi on netgear router](https://www.reddit.com/r/SteamDeck/comments/1anbrld/steam_deck_inconsistent_wifi_on_netgear_router/)

## Preparing your personal machine

### Generate an SSH key pair

Open a terminal and run

```sh
ssh-keygen -t ed25519
```

You should be prompted with something like:

```txt
Enter file in which to save the key (/home/<username>/.ssh/id_rsa):
```

Enter a name, such as `/home/<username>/.ssh/deck`.  This is your identity file.

Install `ssh-copy-id` using your favorite package manager, i.e.:

```sh
brew install ssh-copy-id
```

Copy your public key to the steam deck.  If you don't already know your deck's
IP, you can follow the same instructions for viewing your MAC address to find
your IP [here](#finding-your-decks-mac-address).  When you have your  IP
address, run:

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

```sh
ssh deck
```
