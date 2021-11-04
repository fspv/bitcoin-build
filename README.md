# Build tools for bitcoin repo

Here are some scripts to build [bitcoin repo](https://github.com/bitcoin/bitcoin).

## Install vagrant

Use this [manual](https://www.vagrantup.com/docs/installation)

## Build Vagrant VM
1. Check out bitcoin repo:
    ```bash
    git clone https://github.com/bitcoin/bitcoin
    ```
2. CD into bitcoin repo root. Copy `Vagrantfile` from this repo.
3. Build VM: `vagrant up`.
4. SSH intor vm: `vagrant ssh`
5. Switch to the same user you ran vagrant from. `sudo su - username`
6. Now you can build bitcoin source:
    ```bash
    ./autogen.sh
    ./configure
    make -j 8
    make -j 8 check
    ```

The separate user is created to prevent setting incorrect file permissions on the host.
