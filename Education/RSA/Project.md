---
tags:
  - "#Project"
  - "#RSA"
  - "#Networks"
  - "#RSU"
  - "#Vehicular"
  - "#Vanetza"
---
# **Título:** 
Sistema Integrado de Controlo de Tráfego com Robôs e Sinalização Inteligente 
# **Descrição:** 
O sistema propõe a integração de semáforos e veículos inteligentes numa rede de comunicação em tempo real. Através da monitorização contínua do fluxo de tráfego por sensores e do tempo de espera dos veículos, é possível ajustar dinamicamente os tempos dos semáforos. Esta abordagem visa uma gestão de tráfego mais eficiente, reduzindo congestionamentos e promovendo maior fluidez nas vias. 
# **Explicação – Simulação vs. Real: Simulação:** 

## **Simulação:**
Utilizam-se simuladores, modelos utilizados nas aulas para testar a integração entre veículos e semáforos. 

# **Real:** 
Realiza-se a implementação numa bancada de laboratório utilizando Raspberry Pis para simular a Roadside Unit (RSU) e os veículos inteligentes que possam interagir entre si, simulando apenas o GPS. 

## **Hardware Necessário (para Implementação Real):** 
1 Raspberry Pi para simular a RSU (opção de usar LEDs para simular semáforos) 
1 ou mais Raspberry Pi para simular veículos 
Laptop para visualização/controlo do sistema 

## **Diagrama da Proposta / Esquema Preliminar da Demo:** 
**Veículos:** recolhem dados de fluxo. 
**RSU:** Define os intervalos dos semáforos com base na análise dos dados obtidos pelos seus sensores/partilhados pelos veículos. 
**Comunicação:** Redes veiculares / ITS-G5. 
**Interface de Monitorização:** Aplicação para visualização da simulação de tráfego num laptop.


<img src="RSA_project.png" width="900" height="900" style="display: block; margin: auto;" />

## **Scenario:** 
Ambulance approaches an intersection. 
OBU broadcasts a Cooperative Awareness Message (CAM). 
RSU receives it, recognizes the vehicle as an emergency, and gives green light. 

## **Basic workflow:** 
OBU sends CAM or DENM (Decentralized Environmental Notification Message). 
RSU receives message. 
Traffic light controller dynamically adjusts signal timing.

## **To show next week (16/05):** 

Simulated Ambulance in the Virtual machine (or it can be the rasberry) with an OBU sending messages with Vaneza.

## **Future Work**
Testar com vários cruzamentos seguidos.

**MAPEM** para semáforos, código já feito Amanal.