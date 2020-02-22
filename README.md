### Описание решения
1. Для подготовки ядра из исходников был написан скрипт:
```sh
sudo yum install wget -y
wget https://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.5.4.tar.xz
sudo tar -xvf linux-5.5.4.tar.xz -C /usr/src/
cd /usr/src/linux-5.5.4/
sudo yum install gcc ncurses ncurses-devel elfutils-devel elfutils-libelf-devel -y
sudo yum install flex -y
sudo yum install bison  -y
sudo yes "" |  sudo make oldconfig
sudo yum -y install bc    openssl-devel
yum install perl
```
2. Была поднята VM из Vagrant файла, приложенного к ДЗ. На VM был выполнен скрипт из п.1. После перезагрузки VM видим новое ядро:
```sh
uname
```
3. Для автоматической сборки ядра через packer был изменен provision script:
```sh
```
Далее с помощью packer подготовлен образ VM с новым ядром.
4. Образ с новым ядром был проверен:
```sh
vagrant init - ? 
vagrant up - ?
vagrant ssh
uname -r
```
5. Образ был залит на Vagrant cloud
```sh
```
