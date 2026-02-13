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


1. Entrando como root

No live environment:

sudo su


