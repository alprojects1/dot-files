```sh
# GitHub
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/keys/Github/id_ed25519
    AddKeysToAgent yes
    IdentitiesOnly yes

# GitLab
Host gitlab.com
    HostName gitlab.com
    User git
    IdentityFile ~/.ssh/keys/Gitlab/id_ed25519
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
