# Relatório do projeto de SIRS

Segurança Informática em Redes e Sistemas 2020-2021, segundo semestre

## Autores
 
**Grupo 7**

![Francisco Bento](http://web.tecnico.ulisboa.pt/ist193581/retrievePersonalPhoto.do.png) ![Francisco Rosa](http://web.tecnico.ulisboa.pt/ist193581/ist193578.png) ![Pedro Morais](http://web.tecnico.ulisboa.pt/ist193581/ist193607.png)


| Número | Nome              | Correio eletrónico                           |
| -------|-------------------|----------------------------------------------|
| 93581  | Francisco Bento   | <mailto:francisco.bento@tecnico.ulisboa.pt>  |
| 93578  | Francisco Rosa    | <mailto:francisco.rosa@tecnico.ulisboa.pt>   |
| 93607  | Pedro Morais      | <mailto:pedro.morais@tecnico.ulisboa.pt>     |

## Introdução

O objetivo deste projeto consiste em configurar uma infraestrutura crítica de informação (ICI) usada para controlar
a rede elétrica de uma região. Neste projeto vamos considerar que a ICI engloba um edifício central e duas subestações
elétricas, com variadas subredes colocadas dentro da mesma. O edifício central encontra-se ligado à Internet, e as subestações estão ligadas à subrede SCADA através de uma VPN. Esta rede isola o trafégo de controlo da Internet, que é um fator extremamente importante do ponto de vista da segurança da ICI.

## Diagrama de rede

![Imagem Ilustrativa da Rede](http://web.tecnico.ulisboa.pt/ist193581/DiagramadeRede.png)

## Justificação de opções

### Decisões de implementação da rede

Relativamente à implementação da rede, não consideramos haver muitos aspetos a destacar, apenas que decidimos
utilizar switches em vez de routers adicionais nas subredes pois achamos ser mais simples e mais perceptível de
implementar para o nosso grupo.

### Decisões de implementação dos serviços

Apenas iremos referir serviços nos quais foram necessárias escolhas, ou seja, serviços com uma implementação não linear.

#### Escolha das Firewalls

*Máquinas de fora da rede da ICI só podem comunicar com os servidores da DMZ (ou seja, da LAN de serviços) e só para aceder aos serviços fornecidos por esses servidores (WWW e DNS):*

+ Queremos dar DROP de pacotes que não sejam direcionadas aos servidores da DMZ e que não sejam do tipo SSH/DNS

+ iptables-legacy -A FORWARD -p tcp --source 80.60.0.0/22 ! -d 95.92.196.0/24 ! --dport 22 -j DROP
+ iptables-legacy -A FORWARD -p udp --source 80.60.0.0/22 ! -d 95.92.196.0/24 ! --dport 53 -j DROP

*As máquinas da DMZ não podem comunicar com o resto da rede da ICI:*

+ Regra não implementada

*Só as máquinas da LAN SCADA podem comunicar com as máquinas das subestações e estas só podem comunicar com as máquinas da LAN SCADA:*

+ Só queremos que passem pacotes provenientes da LAN SCADA para as subestações e vice-versa

+ iptables-legacy -A FORWARD --source 95.92.194.0/24 -d 95.92.192.0/24 -j ACCEPT
+ iptables-legacy -A FORWARD --source 95.92.194.0/24 -d 95.92.193.0/24 -j ACCEPT
+ iptables-legacy -A FORWARD --source 95.92.192.0/24 -d 95.92.194.0/24 -j ACCEPT
+ iptables-legacy -A FORWARD --source 95.92.193.0/24 -d 95.92.194.0/24 -j ACCEPT

*Todas as máquinas da rede ICI excepto o data historian podem aceder ao DNS:*

+ Queremos dar DROP de pacotes relativos ao historian tentar efetuar DNS resolves

+ iptables-legacy -A FORWARD -p udp --source 95.92.195.2  -d 95.92.196.2 --dport 53 -j DROP

*Deve ser possível fazer ping entre todas as máquinas da rede ICI e das estações dos engenheiros para a Internet:*

+ Daqui depreendemos que só tinhamos de restringir os pings da Internet que não provenham dos engenheiros e vice-versa

+ iptables-legacy -A FORWARD -p icmp ! --source 95.92.197.0/24 -d 80.60.0.0/22 -j DROP
+ iptables-legacy -A FORWARD -p icmp --source 80.60.0.0/22 ! -d 95.92.197.0/24 -j DROP

*Deve ser possível fazer ping aos ips externos que resolvam os serviços fornecidos (WWW e DNS):*

+ Regra encontra-se inserida na configuração da regra anterior.

*Todo o restante tráfego deve ser bloqueado:*

Blacklist(Router 2/central):

+ iptables-legacy -P FORWARD ACCEPT

Whitelist(Router 3):

+ iptables-legacy -P FORWARD DROP

#### Escolha do HTTPS

Configurámos o servidor web para poder servir HTTPS com SSL para www.acdc.pt. Escolhemos o PC "admin" para ser o único PC a conhecer os certificados, o que se pode verificar com o comando "**curl "https://www.acdc.pt/"**".

#### Escolha do DHCP

Para o PC da LAN SCADA e para os PCs da LAN Corporate decidimos colocar o servidor DHCP a ouvir nas duas interfaces do router central ligadas às duas subredes, respetivamente "eth1" e "eth2". Este automaticamente deteta de onde vem o pedido DHCP, determinando qual pool a usar na atribuição do IP. Para os restantes PCs que necessitam de DHCP, todos os servidores foram colocados nos respetivos routers da subrede.

#### Escolha do IDS

Para a escolha do Intrusion Detection System decidimos implementar um network-based IDS (NIDS), pois no contexto deste
projeto o fator mais importante do ponto de vista da segurança é monitorizar e alertar possíveis ameaças em real-time relativemente ao tráfego vindo da Internet. Dessa forma, instalámos e configurámos o Snort (https://www.snort.org/) para vigiar as interfaces "eth1" e "eth4" do router do edifício central.

## Conclusão

Neste projeto conseguimos alcançar a total implementação da rede e dos serviços de forma a garantir e melhorar a segurança da ICI face a vulnerabilidades e ameaças exteriores. Os nossos pontos fortes na vertente dos serviços são
o DHCP, IDS, HTTPS e SSH. 
Consideramos que estes serviços ficaram implementados na sua totalidade. Os nossos
pontos a melhorar são na VPN, pois não conseguimos a total implementação da sua funcionalidade e nas Firewalls, que
está praticamente completa mas falta a regra **"as máquinas da DMZ não podem comunicar com o resto da rede da ICI"**.
Como grupo não temos muito a acrescentar relativamente a possíveis melhorias do projeto em edições futuras, pois concordamos que o projeto está bem estruturado, exceptuando na secção das Firewalls, que achámos não estar absolutamente clara.
