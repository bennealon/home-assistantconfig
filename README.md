# home-assistantconfig

Home Assistant

Secure the raspberry Pi, unless otherwise done.

https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-debian-9
Copy over ssh key if you already have one, best way is to use `ssh-copy-id` method

// Below is pointless now as cannot do the username change remotely.
Change the default username on the Pi to something else, to do this over SSH, need to change ssh_config to allow root login, then follow this guide:

```
sudo vim /etc/ssh/sshd_config
PermitRootLogin yes
sudo systemctl restart sshd
```

Then follow this guide to change the username:
https://thepihut.com/blogs/raspberry-pi-tutorials/how-to-change-the-default-account-username-and-password
note: i had trouble doing this remotely, as the pi by default auto-logs in as the default user `pi`, which means processes are still running under this user.
was much easier to do this without SSH on the pi itself, but still needed to disable auto-login from within the config.

install Home Assistant on raspbian using this guide:
https://www.home-assistant.io/docs/installation/raspberry-pi/

set a password for homeassistant user:
`sudo passwd homeassistant`

Once installed, add a system service using this guide:
https://www.home-assistant.io/docs/autostart/systemd/
Add the lines at the bottom for auto-restart on failure.

install and setup smb as a share for homeassistant
ensure homeassistant gets a samba password.
http://riccardotramma.com/2018/10/use-samba-to-configure-home-assistant/

// Setup the security.
https://www.home-assistant.io/docs/ecosystem/certificates/lets_encrypt/
in step 8, use option 2 to add as an automation.

```
there is an issue with ./certbot-auto on

https://certbot.eff.org/lets-encrypt/debianstretch-other
https://community.letsencrypt.org/t/no-public-key-for-backports/79558
https://rolfje.wordpress.com/2017/06/09/installing-gpg-keys-for-debian-backports/

the keyserver doesnt exist above, use this instead:
gpg --keyserver hkp://keys.gnupg.net --recv-keys 04EE7237B7D453EC 648ACFD622F3D138

EDIT:
// followed the pip.conf pip wheels removal step ONLY WORKS
https://community.letsencrypt.org/t/certbot-auto-certificates-fails-while-installing-phyton-packages-with-these-packages-do-not-match-the-hashes/90363/6

I found my pip config:

root@moon:~# cat /etc/pip.conf

[global]

extra-index-url=https://www.piwheels.org/simple
I deleted the /etc/pip.conf file and certboot managed to work again

```

make sure to -R these commands:

```
sudo chmod -R 755 /etc/letsencrypt/live/
sudo chmod -R 755 /etc/letsencrypt/archive/
```

// Ignore this, still occurs, but not really an issue.
If when restarting during step 5, you get similar error to this:
https://community.home-assistant.io/t/getting-error-doing-job-ssl-handshake-failed-spam-in-log-any-ideas/63510

Ensure that the dns entry does not have the port on the end.

# TODO

Living room lights on dimmed when no motion.
re-try heatmiser config.
add bedroom light scenes
add bedroom wakeup light.
google tts when tumble finished.
google home integration
