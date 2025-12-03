Projeto de Infraestrutura de Rede Segmentada com pfSense. 
Planejamento e Topologia de RedeEste projeto documenta a implementação de uma infraestrutura de rede robusta e segmentada utilizando o firewall/roteador pfSense em ambiente virtualizado. O principal objetivo é isolar o tráfego por função (LAN, Administrativo e Funcionários) e testar a comunicação entre as sub-redes através do roteamento.1.1. Esquema de Segmentação e Endereçamento IPA rede foi dividida em três interfaces principais conectadas ao pfSense:LAN: Rede principal, não tagueada. Utiliza o bloco 192.168.1.0/24 com o Gateway $192.168.1.1$.ADMINISTRATIVO (VLAN 10): Rede tagueada para uso exclusivo da administração. Utiliza o bloco 10.0.0.0/24 com o Gateway $10.0.0.1$.FUNCIONARIOS (VLAN 20): Rede tagueada para uso dos funcionários. Utiliza o bloco 10.0.1.0/24 com o Gateway $10.0.1.1$.1.2. Localização dos ServiçosOs serviços foram distribuídos para validar a comunicação entre as diferentes sub-redes:Servidor Web (NGINX) e Servidor NFS: Foram instalados na Rede LAN (192.168.1.X).Servidor FTP (VSFTPD): Foi instalado na rede ADMINISTRATIVO (10.0.0.X).Os clientes de teste acessam a partir das VLANs $10.0.X.X$.2. 

Configuração do Firewall (pfSense)O fator chave para a funcionalidade do ambiente é o roteamento inter-VLAN realizado pelo pfSense.Regras de Roteamento: Para permitir que os clientes nas sub-redes ($10.0.X.X$) acessem os servidores na LAN ($192.168.1.X$) e se comuniquem entre si, foram aplicadas regras de Permitir Tudo (Pass Any) nas interfaces ADMINISTRATIVO e FUNCIONARIOS.Resultado: Esta configuração garante a conectividade de ponta a ponta, essencial para a validação dos serviços.



 topologia: https://github.com/samueloli22/Projeto-de-Adm-Redes/blob/main/Topologia.png

  ATENÇAO PARA SE CONECTAR NA VLAN EXECUTE OS CAMANDOS: https://github.com/samueloli22/Projeto-de-Adm-Redes/blob/main/configurar_vlan.txt
  
  pois o virtual box nao consegue agir como um switch gerenciavel

  DHCP 

  O dhcp foi configurado no pf-sense quanto a rede principal e as subredes
  Adm: https://github.com/samueloli22/Projeto-de-Adm-Redes/blob/main/dhcp_adm.png
  Lam: https://github.com/samueloli22/Projeto-de-Adm-Redes/blob/main/dhcp_lam.png
  Funcionarios: https://github.com/samueloli22/Projeto-de-Adm-Redes/blob/main/dhcp_fucionarios.png

  DNS Resolver foi configurado no pf-sense tambem
  https://github.com/samueloli22/Projeto-de-Adm-Redes/blob/main/dns_resolver.png

 Servidor nginx foi feito pelos comandos 
 https://github.com/samueloli22/Projeto-de-Adm-Redes/blob/main/comandos_nginx.txt
 resultado: https://github.com/samueloli22/Projeto-de-Adm-Redes/blob/main/Servidor%20nginx.png
 
  


  
  

  

  
 

 

 
