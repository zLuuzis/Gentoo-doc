# Gentoo Installation Guide (UEFI, OpenRC, dist-kernel)

Guia direto e limpo para instalação do Gentoo Linux.

Este guia cobre apenas a instalação do sistema base funcional, sem ambiente gráfico ou customizações específicas.

O objetivo é fornecer uma base estável para que cada usuário configure o sistema conforme suas próprias necessidades.

---

# O que este guia cobre

* conexão com a internet
* particionamento
* instalação do stage3
* configuração do Portage
* configuração de CPU flags
* instalação do kernel (gentoo-kernel-bin)
* firmware
* bootloader (GRUB)
* NetworkManager
* criação de usuário
* configuração do OpenRC
* sistema pronto para uso

---

# O que este guia NÃO cobre

* XFCE
* GNOME
* KDE
* drivers NVIDIA ou AMD
* ambiente gráfico
* configurações pessoais de USE flags

Essas escolhas dependem do hardware e das preferências do usuário.

---

# Público alvo

Usuários que desejam:

* instalar Gentoo corretamente
* ter controle total do sistema
* construir seu próprio ambiente

---

# Filosofia

Gentoo é sobre escolha e controle.

Este guia fornece apenas a fundação.

---

# Guia de instalação

Ver:

docs/INSTALL.md

---

# Requisitos

* sistema UEFI
* conexão com internet
* conhecimento básico de Linux
* paciência

---

# Resultado final

Após seguir este guia, você terá:

* Gentoo funcional
* kernel instalado
* bootloader configurado
* rede funcional
* usuário criado

Pronto para personalização.

---

# Referência

Gentoo Handbook oficial:

https://www.gentoo.org/doc/handbook/

---
# Gentoo Installation Guide (UEFI, OpenRC, dist-kernel, binhost)

Este guia mostra como instalar o Gentoo Linux base usando:

* UEFI
* OpenRC
* dist-kernel
* gentoo-kernel-bin
* binhost
* NetworkManager

Sem ambiente gráfico.

---

# 1. Entrando como root

No live environment:

```
sudo su
```

---

# 2. Conectando à internet

Listar redes:

```
nmcli device wifi list
```

Conectar:

```
nmcli device wifi connect "Rede" password "senha"
```

Alternativa (recomendado):

```
nmtui
```

---

# 3. Configurar DNS

Editar:

```
nano /etc/resolv.conf
```

Adicionar:

```
nameserver 8.8.8.8
nameserver 1.1.1.1
```

Testar conexão:

```
ping gentoo.org
```

---

# 4. Verificar discos

```
lsblk
```

---

# 5. Partições

Exemplo usado:

* /dev/sdaX → EFI → 1G
* /dev/sdaX → SWAP → 8G
* /dev/sdaX → ROOT → restante

Formatar:

```
mkfs.fat -F 32 /dev/sdaX
mkswap /dev/sdaX
swapon /dev/sdaX
mkfs.ext4 /dev/sdaX
```

Montar:

```
mkdir -p /mnt/gentoo
mount /dev/sdaX /mnt/gentoo

mkdir -p /mnt/gentoo/efi
mount /dev/sdaX /mnt/gentoo/efi
```

---

# 6. Instalar stage3

Entrar no diretório:

```
cd /mnt/gentoo
```

Baixar stage3:

links https://www.gentoo.org/downloads/

Extrair:

```
tar xpfv stage3-*.tar.xz --xattrs-include='*.*' --numeric-owner
```

---

# 7. Configurar make.conf

Editar:

```
nano /mnt/gentoo/etc/portage/make.conf
```

Exemplo:

```
COMMON_FLAGS="-march=native -O2 -pipe"

MAKEOPTS="-j12"

ACCEPT_LICENSE="*"

VIDEO_CARDS="nvidia"

USE="-systemd elogind udev dbus networkmanager -wayland -gnome -kde -qt -gtk"
```

Selecionar mirrors:

```
mirrorselect -i -o >> /mnt/gentoo/etc/portage/make.conf
```

Copiar DNS:

```
cp --dereference /etc/resolv.conf /mnt/gentoo/etc/
```

---

# 8. Entrar no chroot

Montar:

```
mount --types proc /proc /mnt/gentoo/proc
mount --rbind /sys /mnt/gentoo/sys
mount --make-rslave /mnt/gentoo/sys
mount --rbind /dev /mnt/gentoo/dev
mount --make-rslave /mnt/gentoo/dev
mount --bind /run /mnt/gentoo/run
mount --make-slave /mnt/gentoo/run
```

Entrar:

```
chroot /mnt/gentoo /bin/bash
```

Configurar ambiente:

```
source /etc/profile
export PS1="(chroot) ${PS1}"
```

---

# 9. Atualizar Portage

```
emerge-webrsync
emerge --sync
```

---

# 10. Configurar binhost

Editar:

```
nano /etc/portage/binrepos.conf/gentoobinhost.conf
```

Adicionar:

```
priority = 9999
```

Editar make.conf:

```
nano /etc/portage/make.conf
```

Adicionar:

```
BINPKG_FORMAT="gpkg"
FEATURES="${FEATURES} binpkg-request-signature"
```

---

# 11. CPU flags

Instalar:

```
emerge --ask --oneshot app-portage/cpuid2cpuflags
```

Gerar flags:

```
echo "*/* $(cpuid2cpuflags)" > /etc/portage/package.use/00cpu-flags
```

---

# 12. Atualizar sistema

```
emerge -auvDN @world
```

Limpar dependências:

```
emerge --depclean
```

---

# 13. Timezone

```
ln -sf ../usr/share/zoneinfo/America/Sao_Paulo /etc/localtime
```

---

# 14. Locale

Editar:

```
nano /etc/locale.gen
```

Descomentar:

```
pt_BR.UTF-8 UTF-8
```

Gerar:

```
locale-gen
eselect locale set pt_BR.UTF-8
```

---

# 15. Instalar kernel

Adicionar flag:

```
nano /etc/portage/package.use/kernel
```

```
sys-kernel/installkernel dracut
```

Instalar:

```
emerge --ask sys-kernel/gentoo-kernel-bin
```

Firmware:

```
emerge --ask linux-firmware
emerge --ask sys-firmware/sof-firmware
```

---

# 16. Instalar ferramentas essenciais

```
emerge --ask genfstab networkmanager grub efibootmgr vim doas fastfetch
```

---

# 17. fstab

```
genfstab -U / >> /etc/fstab
```

---

# 18. Bootloader

```
grub-install --target=x86_64-efi --efi-directory=/efi --bootloader-id=Gentoo
```

```
grub-mkconfig -o /boot/grub/grub.cfg
```

---

# 19. Usuário

Senha root:

```
passwd
```

Criar usuário:

```
useradd -m -G wheel,audio,video,input -s /bin/bash user
passwd user
```

---

# 20. Hostname

```
nano /etc/hostname
```

```
gentoo
```

Editar hosts:

```
nano /etc/hosts
```

```
127.0.0.1 gentoo localhost
::1 gentoo localhost
```

---

# 21. Serviços

```
rc-update add NetworkManager default
rc-update add dbus default
rc-update add elogind default
```

---

# 22. doas

```
nano /etc/doas.conf
```

```
permit persist :wheel
```

---

# 23. Finalizar

Sair:

```
exit
```

Desmontar:

```
umount -R /mnt/gentoo
```

Reiniciar:

```
reboot
```

---

# Sistema pronto

Você agora tem um sistema Gentoo base funcional.


