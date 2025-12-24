
# Meu Guia de Instalação do Gentoo

Documentação da minha instalação passo a passo 

## 📑 Sumário

01. [Introdução](#introdução)
02. [Hardware Utilizado](#hardware-utilizado)
03. [Download e Criação do Pendrive](#download-e-criação-do-pendrive)
04. [Boot e Configuração de Rede](#boot-e-configuração-de-rede)
05. [Particionamento do Disco](#particionamento-do-disco)
06. [Sistema de Arquivos](#sistema-de-arquivos)
07. [Download do Stage 3](#download-do-stage-3)
08. [Chroot e Inicialização do Sistema](#chroot-e-inicialização-do-sistema)
09. [Configuração do Portage](#configuração-do-portage)
10. [Kernel](#kernel)
11. [Configuração do Sistema](#configuração-do-sistema)
12. [Bootloader](#bootloader)
13. [Criação de Usuários](#criação-de-usuários)
14. [Finalização](#finalização)
15. [Pós-instalação](#pós-instalação)
16. [Notas Pessoais](#notas-pessoais)



## Introdução

Este documento descreve o passo a passo que utilizei para instalar o Gentoo Linux no meu computador.  
Criei este guia para servir como referência pessoal e também para ajudar amigos que queiram repetir o processo.

O objetivo não é substituir a documentação oficial, mas registrar a forma como fiz a instalação e as configurações que funcionaram no meu hardware.

Documentação oficial do Gentoo:
https://wiki.gentoo.org/



## Hardware Utilizado

- **Processador:** Intel Core i5 (10ª geração)
- **Placa de vídeo:** NVIDIA GeForce RTX 3050
- **Memória RAM:** 16GB DDR4
- **Armazenamento:** SSD 240GB
- **Tipo de boot:** UEFI (partição EFI montada em /boot)
- **Modo gráfico:** Xorg e Wayland
- **Rede:** Realtek (Ethernet/Wi-Fi)

> Observação: alguns drivers de vídeo e rede podem variar dependendo do modelo exato do hardware. 
Neste sistema foi necessário configurar suporte à NVIDIA RTX 3050 e drivers Realtek.



## Download e criação do pendrive de instalação

Eu uso o **Ventoy** no pendrive para iniciar a instalação do Gentoo.  
Também prefiro a **ISO Minimal**, porque ela vem só com o básico.

### 1 Baixar a ISO Minimal
Baixe aqui:
https://gentoo.org/downloads/

Escolha **Minimal Installation CD (amd64)**.

---

### 2 Colocar a ISO no pendrive (com Ventoy)

Depois de instalar o Ventoy no pendrive, é muito simples:

👉 **basta arrastar a ISO para dentro do pendrive**,  
como se estivesse copiando qualquer arquivo.

Pronto — sem `dd`, sem comandos 👍

---

### 3 Dar boot

- Abra o menu de boot do seu computador
- Selecione o pendrive Ventoy
- Escolha a ISO do Gentoo na lista



## Boot e Configuração de Rede

Depois de dar boot na ISO Minimal do Gentoo pelo Ventoy, o sistema carrega um ambiente básico no terminal.  
Antes de continuar a instalação, eu configuro a internet.

---

### Verificando as interfaces de rede

Primeiro, vejo quais interfaces estão disponíveis:

```bash
ifconfig

Conexão via Wi-Fi

Quando preciso usar Wi-Fi, utilizo:

net-setup "NomeDaInterface"

ping -c 3 gentoo.org




