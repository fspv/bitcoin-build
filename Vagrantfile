# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANT_COMMAND = ARGV[0]

Vagrant.configure("2") do |config|
  username = "#{ENV['USERNAME'] || `echo -n $(whoami)`}"
  uid = "#{ENV['UID'] || `echo -n $(id -u)`}"

  if VAGRANT_COMMAND == "ssh"
    config.ssh.username = "#{username}"
  end

  config.vm.box = "ubuntu/focal64"
  config.vm.synced_folder ".", "/vagrant", owner: "#{username}", group: "#{username}", mount_options: ["uid=#{uid}", "gid=#{uid}"]

  config.vm.provider "virtualbox" do |vb|
    vb.memory = 8000
    vb.cpus = 8
  end

  config.vm.provision "shell", env: {"username": "#{username}", "uid": "#{uid}"}, inline: <<-SHELL
    set -x

    export DEBIAN_FRONTEND=noninteractive

    apt-get update

    apt-get install -y libboost-all-dev
    apt-get install -y build-essential libtool autotools-dev automake pkg-config bsdmainutils python3
    apt-get install -y libevent-dev libboost-dev libboost-system-dev libboost-filesystem-dev libboost-test-dev
    apt-get install -y libsqlite3-dev
    apt-get install -y libminiupnpc-dev libnatpmp-dev
    apt-get install -y libzmq3-dev
    apt-get install -y systemtap-sdt-dev
    apt-get install -y libqt5gui5 libqt5core5a libqt5dbus5 qttools5-dev qttools5-dev-tools
    apt-get install -y qtwayland5
    apt-get install -y libqrencode-dev
    apt-get install -y libdb-dev libdb++-dev
    apt-get install -y virtualenv
    apt-get install -y sudo
    apt-get install -y bash
    apt-get install -y gcovr

    cd / && virtualenv -p python3 venv
    source /venv/bin/activate && pip install pyzmq

    groupadd -f -g "${uid}" "${username}"
    id spv 2>/dev/null || useradd -u "${uid}" -g "${uid}" -G sudo -m -s /bin/bash "${username}"
    echo "${username} ALL=(ALL) NOPASSWD:ALL" > "/etc/sudoers.d/${username}"
    test -d /home/vagrant/.ssh /home/${username}/.ssh || cp -R /home/vagrant/.ssh /home/${username}/.ssh
    chown -R ${username}:${username} /home/${username}/.ssh
  SHELL
end
