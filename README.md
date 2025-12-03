
## Projeto de Infraestrutura de Rede Segmentada com pfSense

Este projeto documenta a implementação de uma **infraestrutura de rede robusta e segmentada** utilizando o firewall/roteador **pfSense** em um ambiente virtualizado (VirtualBox).

O objetivo principal é **isolar o tráfego por função** (LAN, Administrativo e Funcionários) e **testar a comunicação inter-VLAN** de forma controlada, garantindo a acessibilidade aos serviços distribuídos.

---

## Topologia e Planejamento de Rede

A infraestrutura foi planejada com uma abordagem de segmentação para aumentar a segurança e a organização do tráfego.

### 1. Esquema de Segmentação e Endereçamento IP

A rede foi dividida em três interfaces principais conectadas e gerenciadas pelo pfSense, sendo duas delas implementadas como **VLANs** (Redes Locais Virtuais):

| Interface | Tipo | VLAN ID | Bloco de Rede (CIDR) | Gateway (pfSense) |
| :--- | :--- | :--- | :--- | :--- |
| **LAN** (Principal) | Não Tagueada | N/A | `192.168.1.0/24` | `192.168.1.1` |
| **ADMINISTRATIVO** | Tagueada | **10** | `10.0.0.0/24` | `10.0.0.1` |
| **FUNCIONÁRIOS** | Tagueada | **20** | `10.0.1.0/24` | `10.0.1.1` |

> *O pfSense atua como o ponto central de roteamento, gerenciando o tráfego entre todas as sub-redes e as VLANs.*

*Para referência visual da arquitetura:*


**Visualização da Topologia Completa:** [Topologia do Projeto](https://github.com/samueloli22/Projeto-de-Adm-Redes/blob/main/Topologia.png)

### 2. Localização dos Serviços

Os serviços foram distribuídos em diferentes sub-redes para validar a comunicação e o roteamento inter-VLAN:

* **Rede LAN (`192.168.1.X`):**
    * Servidor Web (**NGINX**)
    * Servidor NFS (Network File System)
* **Rede ADMINISTRATIVO (`10.0.0.X`):**
    * Servidor FTP (**VSFTPD**)

Os clientes de teste acessam os serviços a partir das redes segmentadas (`10.0.X.X`).

---

## Configuração do Firewall (pfSense)

O **pfSense** é o elemento chave que garante tanto a segurança (segmentação) quanto a funcionalidade (roteamento).

### 1. Configuração de Roteamento (Inter-VLAN)

Para permitir a comunicação necessária entre as sub-redes segmentadas (VLANs) e a rede principal (LAN), foram aplicadas regras de firewall no pfSense.

* **Regras:** Foram configuradas regras de **Permitir Tudo (Pass Any)** nas interfaces **ADMINISTRATIVO** e **FUNCIONÁRIOS**.
* **Resultado:** Esta configuração garante a **conectividade de ponta a ponta**, permitindo que os clientes nas VLANs (`10.0.X.X`) acessem os servidores localizados na LAN (`192.168.1.X`) e se comuniquem entre si.

### 2. Serviços Essenciais

* **DHCP (Dynamic Host Configuration Protocol):**
    O serviço DHCP foi configurado no pfSense para as três redes, garantindo a atribuição automática e correta dos endereços IP para cada segmento.
    * [Configuração DHCP Administradores](https://github.com/samueloli22/Projeto-de-Adm-Redes/blob/main/dhcp_adm.png)
    * [Configuração DHCP LAN](https://github.com/samueloli22/Projeto-de-Adm-Redes/blob/main/dhcp_lam.png)
    * [Configuração DHCP Funcionários](https://github.com/samueloli22/Projeto-de-Adm-Redes/blob/main/dhcp_fucionarios.png)

* **DNS Resolver:**
    O Resolvedor DNS também foi configurado no pfSense para garantir que a resolução de nomes de domínio funcione corretamente para todas as sub-redes.
    * [Configuração DNS Resolver](https://github.com/samueloli22/Projeto-de-Adm-Redes/blob/main/dns_resolver.png)

---

## Detalhes da Implementação

### 1. Configuração de VLAN no VirtualBox

Devido à limitação do VirtualBox em simular um *switch* gerenciável com suporte nativo a VLANs, foi necessário executar comandos manuais **dentro dos sistemas operacionais clientes** para *taguear* as interfaces de rede corretamente e conectá-los às VLANs.

**ATENÇÃO:** Para que os clientes se conectem corretamente nas VLANs, é necessário **executar os comandos** conforme o guia: [Comandos para Configurar VLAN](https://github.com/samueloli22/Projeto-de-Adm-Redes/blob/main/configurar_vlan.txt)

### 2. Configuração do Servidor Web (NGINX)

O Servidor Web **NGINX** foi instalado na Rede LAN (`192.168.1.X`).

* **Comandos de Instalação:** [Comandos Utilizados para NGINX](https://github.com/samueloli22/Projeto-de-Adm-Redes/blob/main/comandos_nginx.txt)
* **Resultado da Instalação:** [Página de Teste do Servidor NGINX](https://github.com/samueloli22/Projeto-de-Adm-Redes/blob/main/Servidor%20nginx.png)

---

## Desenvolvedores

* Samuel Antunes de Oliveira Gomes
* Matheus Felipe Bastos Pereira

---
