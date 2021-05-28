# download & install latest release from github

```sh
curl -Ls https://api.github.com/repos/cli/cli/releases/latest | \
    grep -wo "https.*amd64.deb" | xargs curl -Lo /tmp/gh_latest.deb && \
    dpkg -i /tmp/gh_latest.deb && \
    apt-get clean && rm -rf /tmp/* /var/tmp/*
```
