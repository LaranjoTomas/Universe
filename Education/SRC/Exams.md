# 2024
## 19 de Junho

### 1. No contexto das fases de  um ataque a uma rede empresarial:
#### a) Explique o impacto do fator humano nas diferentes fases de um possível ataque externo.
**R:** Engenharia Social, phishing, são formas que atacantes podem usar para conseguir mais fácilmente fazer ataques externos a redes empresariais. Algum empregado com informações internas protegidas pode ser enganado a falar sobre informação confidencial da empresa.

#### b) Proponha soluções de segurança ao nível de rede passíveis de mitigar um ataque externo que use vetores de ataque focados em fatores humanos.
**R:** Ações educativas e políticas organizacionais são soluções de segurança para mitigar ataques que exploram vetores de ataque focados em fatores humanos. Simulações de phishing e formação de empregados, firewalls com filtros de emails e antiphishing.

### 2. Assumindo que a empresa deseja implementar um conjunto de servidores para prestação de serviços, nomeadamente (i) vários servidores Web HTTPS na DMZ com vários sites/domínios (portas TCP 443) públicos que deverão estar disponíveis para o exterior, (ii) dois servidores de Email na DMZ (portas TCP 465 para clientes e servidores) públicos que deverão estar disponíveis para clientes internos e externos, (ii) um servidor Web HTTPS com a Intranet da empresa (porta TCP 443) no Datacenter B que deverá estar disponível apenas para os terminais internos das VLAN 5 e 6, e (iii) três servidores de backup de dados (portas TCP 5001 a 5002) no Datacenter C que apenas deverão estar acessíveis pelos servidores Web HTTPS e por um servidor pré-definido externo para sincronização/replicação dos dados.

![[image-7.png]]

#### a) Proponha as alterações de arquitetura de rede necessárias de modo a poder implementar o controlo de fluxos e proteção contra ataques DDoS. Defina as diferentes zonas da rede e desenhe um diagrama de rede com as alterações propostas

**R:**
DMZ:
	 Colocar HTTPS Web Servers e Emails Servers, tcp 443 e 465 dentro da DMZ, 
	 Adicionar Load balancer à frente dos servidores publicos de forma que o tráfico da internet chege primeiro às firewalls 3/4 depois são direcionadas ao loadbalancer do DMZ que depois irá enviar para os servidores
Datacenter B:
	Colocar o Intranet HTTPS WebServer (TCP 443) no datacenter B 
	Adicionar um Load Balancer à entrada no datacenter B de forma que tráfego externo passe primeiro pelo loadbalancer e depois seja encaminhado para o servidor.
	Firewall no interior de datacenter B que está conectada diretamente ao loadbalancer
Datacenter C:
	Colocar as 3 data backup servers no datacenter C
	Adicionar Firewall no interior do datacenter C
Edificio A:
	Adicionar um Load Balancer entre os SWL3 C1 e C2 para os SWL3 F4 e F3
	Firewall no interior de edificio A que está conectada diretamente aos SWL3 F4 e F3 e depois encaminha o trafego para os Hubs
Edificio B:
	Adicionar um Load Balancer entre os SWL3 C1 e C2 para os SWL3 F2 e F1
	Firewall no interior de edificio B que está conectada diretamente aos SWL3 F2 e F1 e depois encaminha o trafego para os Hubs
Firewalls:
	Adicionar stateless firewalls por baixo do router 1 e router 2

#### b) Apresente uma lista das regras de firewall/controle de fluxo de tráfego (de alto nível) nos vários locais.

**R:** 
stateless firewalls:
	dropar todo os tráfego diferente de TCP 443 e 465
	Permitir tráfego de portos tcp 443 e 465
Firewalls 3 e 4:
	Permitir tráfego de portos tcp 443 e 465
	Permitir tráfego que vem dos serivdores externos especificos para o servidor de backup no datacenter C de portas 5001 e 5002
	Permitir tráfego do web Server publico do DMZ para o Backup server no datacenter C
Firewall datacenter B:
	Permitir tráfego do web Server publico do datacenter B para o Backup server no datacenter C
	Permitir tráfego das vlans 5 e 6 internas da rede para o intranet HTTPS Web Server no datacenter B
Firewall datacenter C:
	Permitir Tráfego nas portas TCP 5001 e 5002
	dropar os outros
Firewalls building A e Building B:
	Permitir tráfego das vlans 1-6 internas para o Email Server no dmZ
	Permitir tráfego das vlans 5 e 6 internas da rede para o intranet HTTPS Web Server no Datacenter B
	Permitir tráfego das vlans 1-6 para conseguirem aceder à internet

![[image-6.png]]
### 3. Proponha uma solução de comunicação e encaminhamento IPv4 ao nível da rede, e respetivas alterações nas regras das Firewalls, que garanta que o tráfego TCP para os servidores de backup no Datacenter C (via WAN) seja encaminhado de forma que garanta confidencialidade.

**R:**  Para assegurar confidencialidade no tráfego TCP nas portas 5001 e 5002 que atravessam o WAN para os backup Servers no datacenter C seria usar IPsec VPN tunnels. Configura um túnel IPsec site-to-site entre um router na secção do Core e o router 9 (datacenter C), com auntenticação de chaves pre-compartilhadas ou certificados de uma entitade certificadora, e depois colocar o tunnel cobrindo apenas a rede 192.2.0.0/20 para 10.0.0.0/28. 
Por parte das firewalls seria necessário alterar as firewalls 3 e 4 para bloquear os portos 5001 e 5002 para impedir flow não encriptado e permitir as portas que o ipsec utiliza, ou seja, udp 500, udp 4500 e ESP. 
Ainda na Firewalls 3 e 4 crirar uma tabela para dirigir tráfego para a interface do Tunnel com os ports TCP 5001 e 5002. Negar o resto dos ports de ir para lá.
Firewall do datacenter C recusar tráfego 5001 e 5002, aceitar tráfego 500, 4500 e ESP e aceitar 5001 e 5002 se vieram pela interface do Tunnel.

### 4. Proponha um sistema SIEM, incluindo o processo de coleta de dados e a definição de regras de alerta, capaz de alertar para

#### a)Tentativas de acesso a objectos não autorizados nos servidores HTTPS.
**R:** Para coleção de dados teriamos de configurar os nossos servidores de HTTPS para mandar os access logs e security logs para serem analisados. Utilizando protocologos como Syslog é possivel obter estes logs, para a regra de alerta de SIEM podiamos filtrar por várias tentativas de login de um único Source IP num curto espaço de tempo. Tentativas de acesso a um url especifico ou diretórios não autorizados a contas sem as credenciais. Tentativas de Login de geograficamente localizações improváveis.

#### b) Clientes externos a participar num ataque DDoS. Indique pelo menos 3 regras.

**R:** Para coleção de dados teriamos de configurar os nossos servidores de HTTPS para mandar os access logs e security logs para serem analisados. Utilizando protocologos como Syslog é possivel obter estes logs. SIEM rules: Comparar os IPs de users com uma base de dados de IPs conhecidos que participaram em outros DDoS ataques. IPs com vários pedidos em curtos espaços de tempo. Padrões de tráfego fora do comum a focar em oports ou protocologos especificos.

#### c) Possível atividade de uma botnet.

**R:** Para coleção de dados teriamos de configurar os nossos servidores de HTTPS para mandar os access logs e security logs para serem analisados. Utilizando protocologos como Syslog é possivel obter estes logs. Ratio de Download/Upload fora do comum no caso de estar a servir de exfiltration. Acesso de multiples External IPs com enorme quantidades de volumes sobre conecções e pedidos. Ips internos a aceder a dominios externos com uma média de tempo de ação fixos. 

#### d) Possível exfiltração encoberta (stealth) de dados de terminais internos utilizando serviços externos legítimos e autorizados

**R:** Para coleção de dados teriamos de configurar os nossos servidores de HTTPS para mandar os access logs e security logs para serem analisados. Utilizando protocologos como Syslog é possivel obter estes logs. Valores anormais de upload para serviços de cloud tipo microsoft. Acesso a serviços externos legitimos e autorizados fora do horário de funcionamento de um normal ser humano. Utilização de Vários tipos de serviços externos num curto espaço de tempo