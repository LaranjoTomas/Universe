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