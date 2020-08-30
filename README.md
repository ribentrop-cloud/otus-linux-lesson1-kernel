## Описание решения
### Сборка ядра из исходников.
1. Для автоматической сборки ядра и подготовки образа через packer был изменен  stage-1-kernel-update.sh:
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
2. В stage-2-kernel-update.sh добавлено удаление исходников после сборки ядра:
```
#!/bin/bash
# clean all
...
rm  -f ~/.bash_history
# удаление исходников после сборки ядра
rm -rf /usr/src/linux-5*
history -c
...
```
Далее с помощью packer подготовлен образ VM с новым ядром по методичке.
3. Ссылка на vagrantbox: _https://app.vagrantup.com/ribentrop/boxes/centos-7.7_

