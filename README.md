# OTUS PRO Homework 29 DHCP PXE

## Домашняя работа 29: DHCP, PXE

### Домашнее задание:
**1. Подготовка рабочего места**   
**2. Настроить загрузку по сети дистрибутива Ubuntu 22**   
**3. Установка должна проходить из HTTP-репозитория**   
**4. Настроить автоматическую установку c помощью файла user-data**
5. Задания со звёздочкой*: Настроить автоматическую загрузку по сети дистрибутива Ubuntu 24 c использованием UEFI     
     
      
Формат сдачи ДЗ - vagrant + ansible  

---
## Выполнение задания:
### 1. Подготовка рабочего места:
Выполнение домашнего задания предполагает, что на компьютере установлен Vagrant+VirtualBox   
**[Как установить Vagrant на Debian 12](https://github.com/avlikh/Install_Vagrant_Debian12/blob/main/README.md)**   

Развернем Vagrant-стенд:
  - Создайте папку с проектом и зайдите в нее (например: /opt/otus/pxe):
```
mkdir -p /opt/otus/pxe ; cd /opt/otus/pxe
```
  - Клонируете проект с Github, набрав команду:
```
apt update -y && apt install git -y ; git clone https://github.com/avlikh/Otus_pro_29.git .
```
  - Запустите проект из папки, в которую склонировали проект (в нашем примере /opt/otus/pxe):
```
vagrant up
```
Результатом выполнения команды: 
<details>
<summary> vagrant up</summary> 

```
root@deb4likh:/opt/otus/pxe# vagrant up
Bringing machine 'pxeserver' up with 'virtualbox' provider...
Bringing machine 'pxeclient' up with 'virtualbox' provider...
==> pxeserver: Importing base box 'bento/ubuntu-22.04'...
==> pxeserver: Matching MAC address for NAT networking...
==> pxeserver: Checking if box 'bento/ubuntu-22.04' version '202407.23.0' is up to date...
==> pxeserver: Setting the name of the VM: pxe_pxeserver_1745306516554_14851
==> pxeserver: Clearing any previously set network interfaces...
==> pxeserver: Preparing network interfaces based on configuration...
    pxeserver: Adapter 1: nat
    pxeserver: Adapter 2: intnet
    pxeserver: Adapter 3: hostonly
==> pxeserver: Forwarding ports...
    pxeserver: 80 (guest) => 8080 (host) (adapter 1)
    pxeserver: 22 (guest) => 2222 (host) (adapter 1)
==> pxeserver: Running 'pre-boot' VM customizations...
==> pxeserver: Booting VM...
==> pxeserver: Waiting for machine to boot. This may take a few minutes...
    pxeserver: SSH address: 127.0.0.1:2222
    pxeserver: SSH username: vagrant
    pxeserver: SSH auth method: private key
    pxeserver: Warning: Connection reset. Retrying...
    pxeserver: Warning: Remote connection disconnect. Retrying...
    pxeserver:
    pxeserver: Vagrant insecure key detected. Vagrant will automatically replace
    pxeserver: this with a newly generated keypair for better security.
    pxeserver:
    pxeserver: Inserting generated public key within guest...
    pxeserver: Removing insecure key from the guest if it's present...
    pxeserver: Key inserted! Disconnecting and reconnecting using new SSH key...
==> pxeserver: Machine booted and ready!
==> pxeserver: Checking for guest additions in VM...
    pxeserver: The guest additions on this VM do not match the installed version of
    pxeserver: VirtualBox! In most cases this is fine, but in rare cases it can
    pxeserver: prevent things such as shared folders from working properly. If you see
    pxeserver: shared folder errors, please make sure the guest additions within the
    pxeserver: virtual machine match the version of VirtualBox you have installed on
    pxeserver: your host and reload your VM.
    pxeserver:
    pxeserver: Guest Additions Version: 7.0.18
    pxeserver: VirtualBox Version: 7.1
==> pxeserver: Setting hostname...
==> pxeserver: Configuring and enabling network interfaces...
==> pxeserver: Mounting shared folders...
    pxeserver: /opt/otus/pxe => /vagrant
==> pxeserver: Running provisioner: ansible...
    pxeserver: Running ansible-playbook...

PLAY [PXE manual install] ******************************************************

TASK [Gathering Facts] *********************************************************
ok: [pxeserver]

TASK [pxe_manual : Check if UFW is installed] **********************************
ok: [pxeserver]

TASK [pxe_manual : Disable UFW if installed] ***********************************
changed: [pxeserver]

TASK [pxe_manual : Stop and disable UFW service] *******************************
changed: [pxeserver]

TASK [pxe_manual : base soft] **************************************************
changed: [pxeserver]

TASK [pxe_manual : Set up timezone] ********************************************
changed: [pxeserver]

TASK [pxe_manual : enable chrony] **********************************************
changed: [pxeserver]

TASK [pxe_manual : copy /etc/dnsmasq.d/pxe.conf] *******************************
changed: [pxeserver]

TASK [pxe_manual : create a directory /srv/tftp if it does not exist] **********
changed: [pxeserver]

TASK [pxe_manual : Download Plucky Netboot archive] ****************************
changed: [pxeserver]

TASK [pxe_manual : Extract Plucky Netboot archive to /srv/tftp] ****************
changed: [pxeserver]

TASK [pxe_manual : Restart dnsmasq service] ************************************
changed: [pxeserver]

TASK [pxe_manual : create a directory /srv/images if it does not exist] ********
changed: [pxeserver]

TASK [pxe_manual : Download Ubuntu Live Server ISO] ****************************
changed: [pxeserver]

TASK [pxe_manual : copy /etc/apache2/sites-available/ks-server.conf] ***********
changed: [pxeserver]

TASK [pxe_manual : Enable ks-server Apache site] *******************************
ok: [pxeserver]

TASK [pxe_manual : copy /srv/tftp/amd64/pxelinux.cfg/default] ******************
changed: [pxeserver]

TASK [pxe_manual : Restart apache2 service] ************************************
changed: [pxeserver]

PLAY [PXE Autoconfig] **********************************************************

PLAY RECAP *********************************************************************
pxeserver                  : ok=18   changed=15   unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

==> pxeclient: Importing base box 'bento/ubuntu-22.04'...
==> pxeclient: Matching MAC address for NAT networking...
==> pxeclient: Checking if box 'bento/ubuntu-22.04' version '202407.23.0' is up to date...
==> pxeclient: Setting the name of the VM: pxe_pxeclient_1745306790983_73851
==> pxeclient: Fixed port collision for 22 => 2222. Now on port 2200.
==> pxeclient: Clearing any previously set network interfaces...
==> pxeclient: Preparing network interfaces based on configuration...
    pxeclient: Adapter 1: nat
    pxeclient: Adapter 2: hostonly
==> pxeclient: Forwarding ports...
    pxeclient: 22 (guest) => 2200 (host) (adapter 1)
==> pxeclient: Running 'pre-boot' VM customizations...
==> pxeclient: Booting VM...
==> pxeclient: Waiting for machine to boot. This may take a few minutes...
    pxeclient: SSH address: 127.0.0.1:22
    pxeclient: SSH username: vagrant
    pxeclient: SSH auth method: private key
    pxeclient: Warning: Authentication failure. Retrying...
    pxeclient: Warning: Authentication failure. Retrying...
    pxeclient: Warning: Authentication failure. Retrying...
    pxeclient: Warning: Authentication failure. Retrying...
    pxeclient: Warning: Authentication failure. Retrying...
    pxeclient: Warning: Authentication failure. Retrying...
    pxeclient: Warning: Authentication failure. Retrying...
    pxeclient: Warning: Authentication failure. Retrying...
    pxeclient: Warning: Authentication failure. Retrying...
    pxeclient: Warning: Authentication failure. Retrying...
    pxeclient: Warning: Authentication failure. Retrying...
    pxeclient: Warning: Authentication failure. Retrying...
    pxeclient: Warning: Authentication failure. Retrying...
    pxeclient: Warning: Authentication failure. Retrying...
    pxeclient: Warning: Authentication failure. Retrying...
    pxeclient: Warning: Authentication failure. Retrying...
    pxeclient: Warning: Authentication failure. Retrying...
    pxeclient: Warning: Authentication failure. Retrying...
    pxeclient: Warning: Authentication failure. Retrying...
    pxeclient: Warning: Authentication failure. Retrying...
    pxeclient: Warning: Authentication failure. Retrying...
    pxeclient: Warning: Authentication failure. Retrying...
    pxeclient: Warning: Authentication failure. Retrying...
    pxeclient: Warning: Authentication failure. Retrying...
    pxeclient: Warning: Authentication failure. Retrying...
    pxeclient: Warning: Authentication failure. Retrying...
    pxeclient: Warning: Authentication failure. Retrying...
    pxeclient: Warning: Authentication failure. Retrying...
    pxeclient: Warning: Authentication failure. Retrying...
    pxeclient: Warning: Authentication failure. Retrying...
Timed out while waiting for the machine to boot. This means that
Vagrant was unable to communicate with the guest machine within
the configured ("config.vm.boot_timeout" value) time period.

If you look above, you should be able to see the error(s) that
Vagrant had when attempting to connect to the machine. These errors
are usually good hints as to what may be wrong.

If you're using a custom box, make sure that networking is properly
working and you're able to connect to the machine. It is a common
problem that networking isn't setup properly in these boxes.
Verify that authentication configurations are also setup properly,
as well.

If the box appears to be booting properly, you may want to increase
the timeout ("config.vm.boot_timeout") value.

```
</details>

станут созданные 2 виртуальные машины **pxeserver** (Ubuntu 22.04) и **pxeclient** (стартующий по сети).

---
### 2. Настроить загрузку по сети дистрибутива Ubuntu 22   
### 3. Установка должна проходить из HTTP-репозитория     
     
После запуска команды vagrant up была настроена установка по сети дистрибутива Ubuntu 22.04.2 на клиете **pxeclient**. 
Для этого, Vagrant выполнил Ansible playbook **"provision.yml"** c тэгом **"pxe_manual"**    

**Зайдем в Virtialbox и убедимся, что на **pxeclient** запустилась сетевая установка Ubintu в ручном режиме:**     
     
![image](https://github.com/user-attachments/assets/b326ce83-3451-4071-9424-6b6f0c5822c5)
     
![image](https://github.com/user-attachments/assets/1af08c16-9f2d-4ab0-ad32-47372057663c)

### 4. Настроить автоматическую установку c помощью файла user-data     
Выполним ansible playbook **"provision.yml"** c тэгом **"pxe_auto"**

```
ansible-playbook provision.yml --tags "pxe_auto"
```

<details>
<summary> результат выполнения команды: </summary>

```
root@deb4likh:/opt/otus/pxe# ansible-playbook provision.yml --tags "pxe_auto"

PLAY [PXE manual install] ***********************************************************************************************************************************************************************************************

PLAY [PXE Autoconfig] ***************************************************************************************************************************************************************************************************

TASK [Gathering Facts] **************************************************************************************************************************************************************************************************
ok: [pxeserver]

TASK [pxe_auto : Ensure /srv/ks directory exists] ***********************************************************************************************************************************************************************
ok: [pxeserver]

TASK [pxe_auto : copy /srv/ks/user-data] ********************************************************************************************************************************************************************************
ok: [pxeserver]

TASK [pxe_auto : Create /srv/ks/meta-data file] *************************************************************************************************************************************************************************
changed: [pxeserver]

TASK [pxe_auto : Create /srv/ks/vendor-data file] ***********************************************************************************************************************************************************************
changed: [pxeserver]

TASK [pxe_auto : copy /etc/apache2/sites-available/ks-server.conf] ******************************************************************************************************************************************************
changed: [pxeserver]

TASK [pxe_auto : copy /srv/tftp/amd64/pxelinux.cfg/default] *************************************************************************************************************************************************************
changed: [pxeserver]

TASK [pxe_auto : Restart dnsmasq service] *******************************************************************************************************************************************************************************
changed: [pxeserver]

TASK [pxe_auto : Restart apache2 service] *******************************************************************************************************************************************************************************
changed: [pxeserver]

PLAY RECAP **************************************************************************************************************************************************************************************************************
pxeserver                  : ok=9    changed=6    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
</details>

Перезапустим виртуальную машину **pxeclient** используя графический интерфейс Virtualbox.    
Подождем окончания автоматической установки Ubuntu, выключим виртуальную машину **pxeclient** и поменяем параметры загрузки  **pxeclient** на загрузку с диска.    
Запустим виртуальную машину, и попробуем зайти в нее используя пользоваля **otus** с пароем **123**

![image](https://github.com/user-attachments/assets/5659aff8-49f5-4456-a87d-b5cd15396ce5)

**Условия домашнего задания выполнены.**
