#  Racktables

## Introdução

Instalação e Configuração de estado do Racktables
## Requerimentos

1. Alterar Selinux para permissive

SELinux

```
sed -i 's/enforcing/permissive/g' /etc/selinux/config /etc/selinux/config
reboot
```


```


2.  Antes de rodar o playbook de instalação será necessário criar o volume Logico


## Instalação do ambiente

### Prerequisitos

1. Sistema Operacional CentOS 7
 
2. Nas maquina de banco. Criar o Volume Group **VG00**

Neste exemplo foi adicionado um disco especifico. E, dentro do linux:

# Particionando o Disco
fdisk /dev/sdc

# Criando o Volume Group
vgcreate VG01  /dev/sdc1

# Verificar o VG00
vgdisplay -v VG00

# Criando o Logical Volume
lvcreate -l100%FREE -n lvol00 VG00

# Verificar o Logical Volume
lvdisplay -v lvol00

# Criando Filesystem
mkfs.xfs  -L Bancos /dev/VG00/lvol00

# Criando o diretorio
mkdir /bancos/mysql

## Editar o /etc/fstab e incluir a linha abaixo
vi /etc/fstab

LABEL=Bancos   /bancos             xfs    defaults        1 2


## Montado filesystem

mount -av

# Provisionar Racktables pela primeira vez:

## Example

```
sudo ansible-playbook  -i inventory site.yml -e "env_site=prd installrack=true  mysql_datadir=/bancos/mysql mysql_port=3306" 
```

## Author
Andre Souto <soutoandre@gmail.com>

```
