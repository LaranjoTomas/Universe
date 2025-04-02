---
tags:
  - "#RSA"
  - "#Test"
---

# De acordo com os conceitos e funcionamento de CDNs e redes peer-to-peer, explique quais são as maiores semelhanças e diferenças entre estas redes.

- **Diferenças**:
  - CDN tem estrutura com servidor e clientes, é baseado em servidores dedicados distribuídos em vários locais estratégicos. Enquanto P2P basease na comunicação direta entre utilizadores.
  - CDN é controlado por uma empresa, P2P é por utilizadores sem um controle central.
  - CDN é usado para streaming videos, sites, games. Enquanto P2P é para partilhar ficheiros entre users.
- **Semelhanças**:
  - Ambas as redes tornam-se melhor quantos mais pontos (servidores e nós) estiverem disponíveis.
  - Ambas os modelos reduzem a carga sobre um único servidor/nó, distribuindo os dados por vários pontos da rede.

# Num protocolo peer-to-peer não estruturado, que mecanismos podem ser usados para procurar um conteúdo raro?

**Flooding** é o mecanismo mais comum usado para procurar um conteúdo numa rede P2P mas quando se trata de conteúdo raro este tem mais desvantagens que vantagens. 
**K-Random walkers** em vez de enviar a consulta para todos os vizinhos, o nó escolhe aleatoriamente alguns vizinhos para enviar o pedido com um **TTL** alto. 
Por último, o **Expanding Ring Search** aonde a pesquisa começa com um raio pequeno limitado aos primeiros vizinhos, se não encontrar o conteúdo o tamanho do raio vai aumentar a cada tentativa. até encontrar o conteúdo.

# No protocolo de distribuição de conteúdos peer-to-peer BitTorrent, como é que este consegue incentivar os peer nodes a participar na rede e partilhar os conteúdos?

O BitTorrent tem o principio de **tit-for-tat** aonde um utilizador terá de partilhar ficheiro na rede para poder receber ficheiros da mesma rede. Isto inabilita leeches the aproveitarem-se na rede.
# Na manutenção da informação de conteúdos utilizando uma DHT, como se tem informação do local para incluir a localização de um conteúdo?
A manutenção da informação sobre a localização de conteúdos é feita através de um mecanismo de **hashing distribuído**. Um ficheiro, por exemplo, video.mp4 vai ser aplicado uma função hash que pode ser ao seu nome ou ao seu conteúdo que irá resultar num identificador. Este identificador será utilizado para determinar em que nó da DHT a informação fica armazenada. 
# Numa rede de veículos numa cidade, com percursos distintos, utilizaria um protocolo de encaminhamento Location Aware Routing ou Batman? Justifique

#### **BATMAN**
Se os percursos mencionados fossem semelhantes este ganhava.
Em redes veiculares numa cidade, batman seria uma escolha devido ao facto de se adaptar melhor a topologias que constantemente. Não depende de GPS o que pode ser pouco confiável em areas rurais, garante roteamento descentralizado e resiliente. Enquanto LAR pode ser util em ambientes previsíveis ele pode ter problemas com mobilidade rápida e as limitações do GPS.
## **LAR**
Os percursos mencionados referem-se aos **percursos dos veículos**. Numa rede de veículos (VANET), a mobilidade dos nós (veículos) é a principal característica que influencia a topologia da rede e, por conseguinte, os caminhos que os pacotes de dados podem tomar. Quando se refere a "percursos semelhantes" dos veículos numa cidade, implica que, apesar de poderem estar em locais diferentes, os veículos podem seguir padrões de movimento comuns (por exemplo, rotas dentro da cidade, seguir certas avenidas, etc.). Esta semelhança nos percursos dos veículos tem várias implicações para a rede e para a escolha do protocolo de encaminhamento: 
* **Topologia da Rede Dinâmica, mas com Padrões:** Embora a topologia da rede esteja em constante mudança devido ao movimento dos veículos, a semelhança nos seus percursos pode levar a padrões de conectividade e desconectividade que podem ser explorados ou que precisam ser robustamente geridos pelos protocolos de encaminhamento.
* **Qualidade das Ligações Variavelmente Previsível:** Se os veículos seguem percursos semelhantes, a qualidade das ligações entre eles pode ter alguma previsibilidade baseada na distância e na presença de obstáculos comuns ao longo desses percursos. No contexto da escolha entre AODV e Batman, esta informação reforça a justificação dada anteriormente para a preferência pelo Batman: 
* O **Batman** beneficia de avaliar continuamente a qualidade das ligações (baseado na receção de OGMs) e adaptar os caminhos de encaminhamento de forma proativa. Se os percursos dos veículos são semelhantes, as mudanças na qualidade das ligações podem seguir padrões que o Batman pode aprender e adaptar-se mais rapidamente do que o AODV, que reage principalmente a falhas de ligação detetadas. 
* Embora o AODV seja eficiente em situações de menor mobilidade, a mobilidade inerente aos veículos, mesmo com percursos semelhantes em locais diferentes, ainda exige uma adaptação rápida às mudanças de topologia, algo em que o Batman se destaca pela sua abordagem proativa e pela manutenção de informação de encaminhamento mais atualizada. Portanto, os "percursos semelhantes" referem-se aos trajetos físicos dos veículos, e esta informação é crucial para entender a dinâmica da rede e justificar a escolha de um protocolo de encaminhamento robusto à mobilidade como o Batman. Os percursos dos pacotes são uma consequência da topologia da rede criada pelo movimento dos veículos e das decisões tomadas pelo protocolo de encaminhamento para entregar os dados ao destino.

**Protocolos Reactivos** são melhor para percursos distintos enquanto **Protocolos Proativos** são melhor para percursos semelhantes.

# De que forma o Batman consegue manter informação da qualidade das ligações atualizada? Justifique.

Através de messagens OGM (Originator Message) que cada nó da rede manda periódicamente para informar o resto dos nós da sua presença. O batman utiliza a presença ou ausencia destes pacotes de controlo para determinar a qualidade das ligações. 

# Nos mecanismos de aprendizagem é possível chegar a uma decisão ótima se estes forem distribuídos na rede? Justifique.

Nos macanismos de aprendizagem distribuída, é possível chegar a uma decisão ótima, mas depende da eficiência na comunicação e no processo de combinação das informações de cada nó da rede. Maior parte dos casos a solução ótima pode ser aproximada, dependendo da arquitetura e das técnicas de consenso implementadas. Também depende de outros fatores, como o modelo de aprendizagem utilizado.

# Num mecanismo de federated learning, os modelos locais podem ser diferentes? De que forma permitem ter uma convergência na aprendizagem? Justifique.

No mecanismo de federated learning, os modelos locais podem ser diferentes. A convergência na aprendizagem depende de vários fatores:
- **Agregação eficiente**: Algoritmos ajudam a encontrar um modelo global que minimiza a perda nos dispositivos locais.
- **Distribuição de Dados**: Se os dados locais forem muito diferentes, a convergência pode ser mais lenta.
- **Nº de rondas de aprendizagem**: Um número maior de iterações melhora a convergência, mas aumenta o custo de comunicação.
Mesmo que os modelos locais possam ser diferentes em alguns cenários, é possível alcançar convergência através de métodos avançados de agregação e transferência de conhecimento.

--- 
---

# Na pesquisa da informação de conteúdos utilizando uma DHT, como é que um nó consegue localizar qual o nó da DHT que contém informação do conteúdo?

A pesquisa da informação de conteúdos numa DHT é feita através de mecanismos de Hashing, identificadores únicos e routing. Um nó a pesquisar por certo conteúdo necessita de dar hash do nome ou conteudo do ficheiro para depois com a key k, resultado da hash, conseguir usar routing (finger table) para procurar pela network que se encontra. Se o conteúdo foi guardado corretamente, ele estara num nó em que o seu identificador único é parecido com o do conteúdo que o nó inicial está à procura.

# Qual a importância dos rendezvous peers e relay peers em sistemas peer-to-peer? Como se distinguem?

Os **relay peers** oferecem a possibilidade de outros peers conseguirem comunicarem através de equipamento de firewall ou NAT. 
Os **Rendezvous** são tipo pontos de encontro ou sitios para outros peers na rede conseguirem descobrir uns aos outros e os conteúdos. 
Distinguem-se estes dois tipos de Nodes pois um faz parte da data stream e o outro da discovery path, isto é, um é utilizado para troca de dados e outro é utilizado na descoberta de nós.

# Num protocolo peer-to-peer estruturado, qual a diferença na pesquisa de um conteúdo raro ou com muitas réplicas disponíveis na rede peer-to-peer?

No protocologo P2P estruturado é usado DHT para a pesquisa de rare files. A diferença na pesquisa entre as duas raridades de conteúdos está na eficiência e na probabilidade de encontrar o conteúdo rapidamente. Isto deve-se à forma como o conteúdo é armazenado e como se localizam os dados na rede. Se o conteúdo tiver muitas réplicas ele pode estar armazenando em vários nós diferentes o que aumenta a probabilidade de encontrá-lo rápidamente. Se o conteúdo for raro e a rede estiver bem organizada ele estará em poucos nós da rede o que torna a sua pesquisa mais dificil, graças ao mapeamento do DHT a sua busdca ainda pode ser eficiente e só poderá acontecer algum problema se os poucos nós que conttêm este conteúdos estiveres offline ou falharem.
A eficiência da DHT minimiza esse problema, desde que os nós que armazenam o conteúdo estejam disponiveís.

# 