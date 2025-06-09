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

**R:** Load Balancers entre os Routers 1 e 2 para as firewalls 3 e 4, 