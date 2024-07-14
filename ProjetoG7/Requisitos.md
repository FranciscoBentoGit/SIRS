# Lista de verificação (*Checklist*)

**Grupo 7**

## Rede

### Quantos routers tem no total?
#### 5

### Quantos switches tem no total?
#### 7

### Quantas máquinas tem no total?
#### 16

### Quantos computadores ligou ao router da Internet?
#### 3

### Quantas subredes implementou dentro da ICI (excluindo a Internet)?
#### 6

### Quantos routers implementou dentro da ICI (excluindo a Internet)?
#### 4

### Implementou o servidor DNS da ICI?
#### Sim

### Implementou o servidor WWW da ICI?
#### Sim

### Implementou os dois PCs da ICI?
#### Sim

### Criou o PC da LAN SCADA?
#### Sim

### Criou o PC historian?
#### Sim

### Implementou as subredes das suas subestações?
#### Sim

### Implementou os dois switches dessas subredes?
#### Sim

## Serviços

### Criou uma conta admin nos dois servidores da LAN de serviços?
#### Sim

### Implementou alguma coisa no PC historian?
#### Sim

### Implementou o MRTG?
#### Escolhemos fazer DHCP

### Quantas páginas web com gráficos está a fornecer o MRTG?
#### Escolhemos fazer DHCP

### Configurou o OpenSSH?
#### Sim

### É possível fazer ssh e scp do PC da Internet para os servidores da LAN de serviços?
#### Sim

### É possível fazer ssh e scp dos PCs dos engenheiros para os dois servidores da LAN de serviços?
#### Sim

### Ao fazer ssh e scp, a autenticação é baseada em criptografia de chave pública?
#### Sim

### Configurou as VPNs usando o pacote OpenVPN? Funcionam?
#### Não e Não

### Configurou o netfilter / iptables no router do edifício central?
#### Sim

### Esse router bloqueia a maior parte dos acessos da Internet à ICI?
#### Sim

### Esse router bloqueia a maior parte dos acessos da DMZ às outras subredes da ICI?
#### Sim

### Criou um novo nó para instalar o IDS na rede da DMZ?
#### A instalação do IDS foi feita no router central, para ser usado na interface da DMZ e do Corporate

### Instalou o IDS na rede DMZ?
#### Sim

### Instalou o IDS na subrede corporate?
#### Sim

### Indique o conteúdo do teste usado para testar o IDS
#### Para testar, basta ligar o snort na interface pretendida e fazer ping de qualquer PC da Internet para o servidor DNS ou qualquer dos PCs da Corporate, respetivamente

### Configurou DHCP? Funciona?
#### Sim e Sim 

### Configurou HTTPS? Funciona?
#### Sim e Sim

### Configurou DNSSEC? Funciona?
#### Escolhemos fazer DHCP
