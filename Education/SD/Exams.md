# 2024
## 17 de Junho
### Parte A

#### 1. Explain what is fault tolerance and why it is so important in the context of distributed systems. Which are the basic strategies commonly used to handle failures?
**R:** Fault tolerance no contexto de sistemas distribuidos, é quando um nó que falhe ou esteja quase a falhar o sistema consegue detetar malfunctions e/ou mudar os componentes de software afetados para nós que estão estáveis. O que prevêm aplicações de dar crash.
Estratégias básicas de failure handling são: masking the failures, recovering from failures, tolerating failures.
#### 2. Describe schematically, adding the comments you deem appropraite about their fuctionality, the advantages and disavantages, of the three variants of the client-server model that were studied. Can they be implemented using either the message passing or the remote objects paradigms, or are they specifc to one of these paradigms? 

**R:** 
Request Serialization:
- Vantagens:
	- Não é preciso mutual exclusion
	- Modelo Simples
	- Sem concurrência
- Disvantagens:
	- Como só um processo de cliente é servido ao mesmo tempo torna-se ineficiente.
Server replication:
- Vantagens:
	- Tem concurrencia o que aumenta a eficiência, diminui o tempo de serviço necessário
	- Permite sincronizar os diferentes clients que estejam no mesmo shared resource
- Disvantagens:
	- Necessário mutual exclusion
Resouce Replication:
- Vantagens:
	- Scalable
	- É possivel correr simultaneamente o serviço em vários computadores cada um a correr um Server replication model variante.
	- Pedidos de Clients são distribuidos por todos os servidores baseado em regras predefenidas 
- Disvantagens:
	- Se houver alguma alteração do shared resouce é preciso corrigir os outros servidores manualmente
	- Mesmo Disvantagens que server replication

Request Serialization suporta message passing ou remote objects. 
Server replication suporta message passing ou remote objects.
Resouce Replication suporta message passing e remote objects mas como adapta-se melhor a message passing, que já é assíncrona.   Objetos remotos tendem a usar chamadas síncronas, tornando o modelo assíncrono mais complicado.

#### 3. The schematics bellow describes according to the Lamport algorithm.
![[ex24_17_6_sd.png|| 600]]

a(0) | f(0) | l(0) | b(1) | g(2) c(2) | h(3) | m(4) i(4) | n(5) | o(6) | d(7) | e(8) | j(9) | k(10)

$$ a-j: a \prec b \wedge b \prec c \wedge c \prec d \wedge d \prec e \wedge e \prec j \wedge \Rightarrow a \prec j $$
$$ f-c: \neg(f \prec c) \wedge \neg (c \prec f) \Rightarrow f \parallel c  $$
$$ b-i:  b \prec g \wedge g \prec h \wedge  h \prec i \Rightarrow b \prec i $$
e-m is sequencial. i-o é concurrente. i-e é concurrente
#### 4. Supposed we want to ensure service availability in a client-server model. In order to achieve this aim a **resource replication variant**,  where the server is installed in **two hardware plataforms**, is implemeted. In principle, **only one** of the servers is active at a time and interacts with the clients. When it fails, the other starts operating immediately, replacing it. Draw a functional diagram that depicts the organization and list three problems which must be solved for the system to work properly.

![[YEsyes_sd.png]]

1. Deteção de falhas no servidor ativo, algum mecanismo de deteção de falhas para o servidor ativo
2. Gerência de consistênia é necessário no caso do shared resouce for alterado.
3. Transferência de estado entre o servidor ativo e o outro, é necessário que o outro servidor mantenha ou recupere o estado atual do serviço do servidor ativo quando/se alterar o estado do servidor ativo.


# 2023
## 12 de Junho

### Parte A

#### 1. Tanenbaum defines a distributed system as a collection of independent computers that appears to its users as a single coherent system. Using this definition as the starting point, try to elicit some of the distinctive features that this kind of systems present, namely communication through message passing, failure handling and global internal state.

**R:** Comunicação através de passagem de Mensagens em sistemas distribuidos não utiliza memória partilhada entre os nós. Propriedades importantes são: tratamento de Latencia, Relabilidade, segurança. 
Em failure handling os componentes são autónomos e interligados por redes falhas são inevitáveis neste. Um nó sozinho pode falhar independente de outros, ou a rede pode atrasar-se ou perder mensagens. 
Em Global internal state o sistema apresenta-se como um todo mas está distribuído entre bastantes nós. Requer coleta e sincronização de informação entre os nós. Protocologos como logical clocks são usados para manter balanço temporal.

#### 2. What distinguishes the client-server model from peer-to-peer communication? Give an example of each and explain how relevant they are in the examples you have presented.

**R:** O sistema de peer-to-peer não possui hierarquidade entre os users ao contrário de client-server centralizado, tem hierarquia e as funções são definidas. 
Exemplo de Client-Server pode ser Web browsing. Enquanto o client seria o web browser (firefox) o servidor seria o web server (website).
Exemplo de peer-to-peer seria BitTorrent, todos os participantes partilham pedaços de ficheiros entre eles sem uso de um servidor central.

#### 3. The diagram depicts the time evolution of three processes whose local clocks are clocks are scalar logical clocks synchronized according to the Lamport algorithm.

![[image 3.png]]

##### **I)**
a(1) e(1) | j(2) b(2) f(2) | g(3) c(3) k(3) | h(4) l(4) | d(5) i(5)

##### **II)**
$e \parallel a$
$a \prec d$
$d \parallel i$
##### **III)**
Uma **ordem parcial** só ordena eventos baseado em relações de causalidade, e deixa de fora eventos concurrentes desorganizados. Enquanto **ordem total** força que a sequência de todos os eventos, incluindo eventos concurrentes, utilizando **timestamps** para resolver empates.

#### 4. The schematics below depicts a simple solution for load balancing in a client-server model.  The role of the distributor is to forward service requests to any of the servers. The decision on which server a specific service request is forward to, is based on the working load each is presently enduring as perceived by the distributor. Bear also in mind that the copies of the shared region present in the servers must be continuously synchronized. Describe in general terms how the whole system should operate. What kind of criteria should the distributor use to estimate the working load of a server? What kind of protocol should the distributor implement to assess if a given server operating properly, or has failed? 

![[image-1 1.png| 600]]

The role of the distributor is to foward the service requests to any of the servers but how he decides which server to foward to is decided on the working load each server is enduring when seen from the distributor point of view, he acts as an intermediary between clients and a pool of servers, fowarding service requests based on the current load of eahc server. Each replica of the shared region in each server is continuously synchronized too look for any alteration made to it so all the servers have their shared resource updated. The system should operate such that each incoming request is dynamically routed to the server with the lowest perceived workload, for efficiency. For this decisions the distributor can estimate a servers load using metrics like cpu usage, memory usage, response time, etc. This metrics can be periodically reported by each server like the heartbeat method of process failure. If a server fails to responde within a defined timeout, it is marked unavailable and exclused from the pool until recovery. For the replicas of the shared region a replication protocol must be implemented to ensure consistency acrross servers.


# Exam 2022

## 4 de Julho

#### 1. The diagram bellow aims to depict in a hierarchical way the different layers of software/hardware modules required to run a distributed application in a parallel machine based on off-the-shelf computer systems. What is the role played by the middleware layers in this context? Explain why the Java Virtual Machine (JVM) may be thought of as part of the middleware.
![[image-2.png]]

**R:** The middleware provides an abstraction layer for communication between processes that do not share an address space. A communication device is provided which serves as an endpoint for inter-process communication. It ensures that applications can run transparently across different nodes, while it handles complex message passing, load balancing, fault tolerance, and data consistency without requiring the application developer to manage these.

The JVM is considered as middleware because it provides a platform-independent execution environment. Allowing applications written on Java to run on any  system compatible with JVM.

#### 3. ![[image-3.png]]

##### **I)**
a(1) e(1) j(1)| b(2) f(2) | g(3) c(3) | k(4) k(4) d(4)| l(5) | i(6) 
##### **II)**
$a \parallel j$
$e \prec d$

#### 4. The role of the distributor is to forward service requests to the primary server. The backup server is updated by the primary server so that their internal states are continuously synchronized. Assume that the reliability of the hardware platform that takes the role of distributor is much higher than the reliability of the hardware platforms that take the role of primary or backup servers. Which hardware platform is chosen as primary, is decided in a dynamical way: i) it is the first to register in the distributor after reboot; ii) it is the backup server, thereby changing its role, if primary fails. What kind of data the distributor and the primary have to keep internally in order for operations to be carried out as expected? How do the distributor and the backup server become aware of a primary failure? What operations have to be carried out to overcome this problem? Assume that no messages are lost and message transmission time is limited.

![[image-4.png | 600]]


**R:** The primary needs the shared resource, a communication channel to the distributor and a communication channel to the BackUp server.
The Distributor would need the service requests made by the clients, and the address/ID/port of the primary server and the backUp server.
The primary server may have a protocol to alert in case of failure, like heartbeat which may carry information about internal state, the response time of the primary server may hit the timeout value predefined aswell.
Change the primary server status and redirected the distributor to the backUp server until the previous primary server is stable and operational again.
