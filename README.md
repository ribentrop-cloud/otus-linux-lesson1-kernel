### Описание решения
1. Для сборки ядра из исходников был написан скрипт:
```sh
#!/bin/bash
sudo yum update -y &&
sudo yum install -y wget  gcc ncurses ncurses-devel elfutils-devel elfutils-libelf-devel flex bison bc openssl-devel perl &&
wget https://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.5.4.tar.xz &&
sudo tar -xvf linux-5.5.4.tar.xz -C /usr/src/ &&
cd /usr/src/linux-5.5.4/ &&
sudo cp  /boot/config-$(uname -r)  /usr/src/linux-5.5.4/ &&
sudo yes "" |  sudo make oldconfig &&
sudo make -j2  &&
sudo make modules_install &&
sudo make install  &&
sudo chmod +x /boot/vmlinuz-5.5.4 &&
sudo grub2-mkconfig -o /boot/grub2/grub.cfg &&
sudo grub2-set-default 0
```
2. Была поднята VM из Vagrant файла, приложенного к ДЗ. На VM был выполнен скрипт из п.1. После перезагрузки VM видим новое ядро:
```sh
??? uname
```
3. Для автоматической сборки ядра и подготовки образа через packer был изменен provision script stage-1-kernel-update.sh:
```sh
#!/bin/bash
sudo yum update -y &&
sudo yum install -y wget  gcc ncurses ncurses-devel elfutils-devel elfutils-libelf-devel flex bison bc openssl-devel perl &&
wget https://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.5.4.tar.xz &&
sudo tar -xvf linux-5.5.4.tar.xz -C /usr/src/ &&
cd /usr/src/linux-5.5.4/ &&
sudo cp  /boot/config-$(uname -r)  /usr/src/linux-5.5.4/ &&
sudo yes "" |  sudo make oldconfig &&
sudo make -j2  &&
sudo make modules_install &&
sudo make install  &&
sudo chmod +x /boot/vmlinuz-5.5.4 &&
sudo grub2-mkconfig -o /boot/grub2/grub.cfg &&
sudo grub2-set-default 0 &&
sudo rm -f /boot/*3.10* &&
sudo shutdown -r now
```
Далее с помощью packer подготовлен образ VM с новым ядром.
4. Образ с новым ядром был проверен:
```sh
# vagrant box add --name centos-7-7 centos-7.7.1908-kernel-5-x86_64-Minimal.box
# vagrant init centos-7-7
# vagrant up
# vagrant ssh
# uname -r

```
5. Образ был залит на Vagrant cloud

