---
tags:
  - "#ASE"
  - "#Project"
---
# Sensores

2 x ESP32-C3
1 x TC74
1 x BME280
1 x Buzzer 3.3V
# Descrição do Projeto

Obter todos os dados possiveis através dos sensores TC74, BME280 (SPI) e com os módulos de WiFi enviar em real time os dados para a segunda placa de ESP32-C3. Estes dados serão então atualizados na interface visual na segunda placa.
Utilização dos registo local SPIFFS para guardar logs do mesmo e posteriormente ser possivel guardar as alterações climatéricas mesmo com queda de network WiFi.

# Improvements

Utilizar um data logger no caso de perda ou queda da rede WiFi. SPIFFS é um registo local que é uma forma de armazenar dados em arquivos diretamente na memória flash interna do ESP32-C3.

- 1 : A **placa sensor** coleta dados
- 2 : Esses dados são formatados em texto (ex: CSV ou JSON ou TXT).
- 3 : A cada intervalo de tempo (ex: 5 segundos), os dados são gravados em um arquivo no SPIFFS.
- 4 : **Opcionalmente**: a outra placa pode pedir por WiFi os dados arquivados ou os  dados atuais.
  
  
# Em terreno

Um ESP32-C3 nunca terá problemas de consumo energetico. este seria o que recebe os dados via wifi e mostra-os em tempo real na interface.

A outra esp32-c3 que está no terreno seria a que pode ter problemas com energia, por isso é necessário usar FREERTOS e Gestão do consumo energético no Software desta esp32.


# Ideais

Usar apps e clients ou ESPNOW para o WiFi API, espnow em principio fornece mais benefícios.


Brokers, grafana (obter os outputs do esp32 e depois mandar para a grafana). 