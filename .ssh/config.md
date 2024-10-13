**Setup**
```sh
1) mkdir -p ~/.ssh/keys/{github,gitlab,compose}
2) ssh-keygen -t ed25519 -C "EMAIL_HERE" -f ~/.ssh/keys/github/id_ed25519
3) ssh-keygen -t ed25519 -C "EMAIL_HERE" -f ~/.ssh/keys/gitlab/id_ed25519
4) ssh-keygen -t ed25519 -C "EMAIL_HERE" -f ~/.ssh/keys/compose/id_ed25519
5) nano ~/.bashrc **add keys to .bashrc, unless system wide add to .profile or .bash_profile**
6) ssh-add ~/.ssh/keys/github/id_ed25519 &>/dev/null 
7) ssh-add ~/.ssh/keys/gitlab/id_ed25519 &>/dev/null
8) ssh-add ~/.ssh/keys/compose/id_ed25519 &>/dev/null

# If SSH authentication fails after this step:
# Run `ssh-add ~/.ssh/keys/compose/id_ed25519` manually and check for errors.
# If you encounter 'Qt: Session management error' or other issues, ensure SSH AskPass or SSH Agent is properly configured.
```


**Configuring SSH_ASKPASS for GUI Passphrase Input**
```sh
1) sudo apt install ksshaskpass -y
2) ksshaskpass --version **5.27.11 as of writing**
3) which ksshaskpass **/usr/bin/ksshaskpass for me**
4) echo -e 'export SSH_ASKPASS="/usr/bin/ksshaskpass"\nexport SSH_ASKPASS_REQUIRE=prefer' >> ~/.profile **or .bash_profile depending on the system**
4) echo -e 'export SSH_ASKPASS="/usr/bin/ksshaskpass"\nexport SSH_ASKPASS_REQUIRE=prefer' >> ~/.bashrc **whichever you prefer**

|Or|

1) nano ~/.bashrc or nano ~/.profile
2) added the below:
 # ksshaskpass configuration
 export SSH_ASKPASS="/usr/bin/ksshaskpass"
 export SSH_ASKPASS_REQUIRE=prefer
```

**Systemd for ssh-agent**
```sh
1) mkdir -p ~/.config/systemd/user
2) nano ~/.config/systemd/user/ssh-agent.service
    [Unit]
    Description=SSH key agent

    [Service]
    Type=simple
    Environment=SSH_AUTH_SOCK=%t/ssh-agent.socket
    ExecStart=/usr/bin/ssh-agent -D -a $SSH_AUTH_SOCK

    [Install]
    WantedBy=default.target
3) systemctl --user enable --now ssh-agent.service **enable it**
4) systemctl --user status ssh-agent.service **should be running**
5) ssh-add -l **autoconnect should now be enable reboot and check**

# If not, manually add keys using `ssh-add ~/.ssh/keys/compose/id_ed25519` and ensure no session errors occur.
```


**.config**
```sh
# GitHub
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/keys/github/id_ed25519
    AddKeysToAgent yes
    IdentitiesOnly yes

# GitLab
Host gitlab.com
    HostName gitlab.com
    User git
    IdentityFile ~/.ssh/keys/gitlab/id_ed25519
    AddKeysToAgent yes
    IdentitiesOnly yes

# Compose Host (DK01)
Host dk01.alprojects.tech
    HostName dk01.alprojects.tech
    User echo
    Port 3100
    IdentityFile ~/.ssh/keys/compose/id_ed25519
    AddKeysToAgent yes
    IdentitiesOnly yes
```
