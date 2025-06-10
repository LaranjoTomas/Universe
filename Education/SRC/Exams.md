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

# 2021

## 13 Setembro

### 1. Explique as diferentes fases e possíves vetores de ataque para o roubo de dados num sistema de base de dados numa empresa. Apresente possíveis mecanismos de defesa.
**R:** Existem cerca de **8 fases de ataque** para o roubo de dados num sistema de base de dados numa empresa. **Estas começam por aquisição de conhecimento sobre a empresa**, por meios de Engenharia social, phishing emails, etc. Depois vem a **infiltração** de malware em possiveis componentes eletrónicos na network da empresa que irá **aprender** como a empresa está estruturada e aonde está guardado informações importantes. De seguida este malware irá **propagar-se** pelos sistemas de empresa e os seus empregados para conseguir aumentar o seu alcance. Por último **ele junta todas as informações** que possam ser uteis para os atacantes para poder **exfiltra-las** de network interior. Mecanismos de defesa possiveis para evitar tais situações são diversos e cada um afeta uma fase diferente do ataque para o roubo de dados num sistem de base de dados numa empresa. De fazer treinos periódicos de segurança a cada empregado, construir mecanismos de proteção contra emails de phishing como melhorar as defesas internas da rede para detetar ameaças que estejam tanto a tentar entrar como no interior.

![[image-8.png]]

### 2. Uma empresa pretende colocar na sua infraestrutura de rede um conjunto de servidores HTTPS (portos TCP 443 e TCP 8888) acessíveis do exterior com vários serviços Web da empresa. Proponha uma solução de proteção da rede que permita (i) controlar os fluxos de tráfego de acesso aos serviços e (ii) proteger a infraestrutura contra ataques de negação de serviço distribuídos (DDoS). Assuma que a empresa apenas tem uma infraestrutura de encaminhamento de tráfego IP e que possui utilizadores internos que precisam de aceder à Internet.

**R:** Se a empresa prentende alterar a sua infraestrutura de rede para ter servidores https acessíveis do exterior com vários serviços Web da empresa, será necessário adicionar novos componentes que não existem nela neste momento. Primeiro para conseguir controlar os fluxos de tráfego de acesso aos serviços que desejam colocar na rede da empresa será necessário segmentar em zonas com diferentes niveis de segurança. Estas poderiam ser, por exemplo, DMZ, Internet e Core. Ainda para conseguir controlar o tráfego do exterior e interior será necessário colocar firewalls dentro de cada uma destas zonas, na saida da internet para a rede da empresa será necessário colocar uma stateless firewall que permita tráfego de TCP 443 e 8888 com as regras de firewalls tem também de bloquear todas as restantes portas e protocolos não necessários. Depois desta stateless Firewall pode-se colocar uma statefull firewall que irá possuir as mesmas regras que a anterior e que impessa comunicações diretas entre segmentos internos e os servidores. Um loadbalancer depois desta firewall será necessário para ele conseguir distribuir o tráfego de forma eficiente. Os servidores de HTTPS que a empresa deseja colocar serão então colocados depois do load balancer dentro da zona da DMZ onde eles terão de ter a manutenção para funcionarem de forma como a empresa precisa.  

### 3. Proponha uma solução de interligação entre múltiplos polos de uma empresa que providencie confidencialidade para o tráfego de videoconferência e tráfego de sincronismo de base de dados entre elas (e apenas a esse tráfego).

**R:** Uma solução para interligação entre múltiplos pontos de uma empresa que providencie confidencialidade para o tráfego de videoconferência e tráfego de sincronismo de base de dados pode ser a criação de um IPsec VPN Tunnel site-to-site. O IPsec tunnel permite cifrar apenas o tráfego desejado entre redes locais e suporta configurações que especificam qual tráfego cifrar e o tráfego que lhe pode passar, este última tendo assistência de firewalls em cada pontos do tunnel. Por último também é possivel criálo com multiponto consoante o modelo desejado a usar. 

### 4. Admitindo que numa rede empresarial existem múltiplas fichas Ethernet em espaços públicos ou semi-públicos e terminais Wi-Fi, proponha uma solução integrada de controle do acesso de máquinas à rede.

**R:** Nesta rede empresarial com múltiplos fichas ethernet com acesso ao público e terminais Wi-Fi é possivel controlar o acesso de forma que garanta que só disposotivos autorizados acedam à rede da empresa, desta forma é possivel evitar ligações indesejadas. Uma solução possível seria autenticação 802.1X com um servidor RADIUS. Com isto seria possível que todos os dispositivos tenham de se autenticar antes de obter acesso à rede empresarial.

### 5. Numa rede empresarial pretende-se implementar um sistema de deteção de intrusões (IDS) que permita detetar as máquinas comprometidas por uma BotNet. Os elementos da BotNet podem a qualquer momento efetuar uma das seguintes atividades: (i) comunicar diretamente entre si para sincronismos de ações, (ii) receber comandos do exterior da rede via ligações HTTPS e (iii) participar no envio de e-mail em quantidades elevadas (Spam) usando o servidor da empresa. Explique como o sistema pode ser integrado na arquitetura de uma rede empresarial e proponha um possível conjunto de regras para a deteção de comunicações ilícitas.

**R:** Na rede empresarial com possíveis atividades de BotNet a inclusão de um sistema de deteção de intrusões seria um bom método para analisar e capturar tráfego suspeito e gerar alertas do mesmo tráfego. Nesta rede empresarial específica o IDS seria posicionado depois da firewall stateless (firewall conectada à internet) para conseguir todo tráfego que entra e sai da network da empresa. Esta seria uma fonte de dados necessária para conseguir-mos criar regras de SIEM para parar as BotNets internas. A primeira regra criada seria detetar comunicações de componentes internos com outros componentes internos, podemos detetar através de trafego TCP. Segunda regra poderia ser detetar padrões fora do comum de ligações HTTPS de máquinas internas com dominios externos. A terceira regra poderia ser detetar os volumes de tráfego de uso do servidor da empresa, se algum for maior que o normal seria então parte da BotNet possivelmente.