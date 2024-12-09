# Setup SSH Tor hidden service

## Step 1:

You need to find a company that accepts cryptocurrency as payment for its VPS,
there are many such companies, and we advise you to check (lowendbox.com) for it.

As soon as you find the right company for your needs (always check the TOS and the company’s privacy policy),
start paying using ONLY cryptocurrency such as Bitcoin.

Keep in mind that you should always use the Tor browser during this process. You can always download from here.

In addition, make sure you use email that doesn’t require much information about you

## Step 2:

If you use Tor, enter recovery mode or web shell on the VPS provider’s website.
Install Tor and configure SSH via Tor:

```bash
sudo apt install tor
```

Enable Tor startup when booting:

```bash
sudo systemctl enable tor
```

And run Tor after installation:

```bash
sudo systemctl start tor
```

Open the Tor Torrc file using the following command:

```bash
sudo vi /etc/tor/torrc
```

Add or find:

```
HiddenServiceDir /var/lib/tor/ssh/
HiddenServicePort 22 127.0.0.1:22
```

Save the file and restart your SSH:

```bash
sudo systemctl restart ssh
```

## Step 3:

Get the .onion address:

```bash
sudo cat /var/lib/tor/ssh/hostname
foobarvkiwibfiwucd6vxijskbhpjdyajmzeor4mc4i7yopvpo4p7cyd.onion
```

Connect to the hidden service:

```bash
ssh -o ProxyCommand="ncat --proxy-type socks5 --proxy 127.0.0.1:9050 %h %p" sshuser@foobarvkiwibfiwucd6vxijskbhpjdyajmzeor4mc4i7yopvpo4p7cyd.onion
```

Or just use torsocks:

```bash
torsocks ssh sshuser@foobarvkiwibfiwucd6vxijskbhpjdyajmzeor4mc4i7yopvpo4p7cyd.onion
```
