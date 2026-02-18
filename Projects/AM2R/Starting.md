#AM2R

Ler wiki, do edgar, gonçalo silva, tiago rodrigues. claudio sencio

# TO-DO

Apartir da informação que está a ir para o broker local, fazer uma base de dados para depois poder trabalhar nesta. Esta base de dados deve recolher os dados que está a ser enviado para o broker local. (Done)

## Data Logger

To start the data logger:

```bash
python3 mqtt_data_logger.py &
```

It is possible to check if it's running with 

```bash
python3 sensor_query.py sensors
```

To inspect the data the following commands are possible:

```bash
python3 sensor_query.py latest # Last 20 readings

python3 sensor_query.py latest 50 heart # Last 50 heart readings

python3 sensor_query.py sensors # List all sensors

python3 sensor_query.py stats lux # Min/max/avg for lux

python3 sensor_query.py time 2 temperature # Last 2 hours of data
```

## Possiveis trabalhos / paths

### Fusão de sensor LiDAR e ultra sons com os que vem de trás.
Isto deveria ser feito na parte de trás antes de ir para o CAN para evitar congestionamento. Ver os possiveis beneficios da fusão destes sensores.
Só possível depois do márcio mostrar o prótipo e ser colocado com o lidar que o enrico baixou o tamanho por causa do Bus.
### haptic feedback / alertas de som/vibração para o ciclistas


### Possivel planeamento de Rotas com o IOT na bicicleta


## Sensor Fusion:
### LiDAR & Ultra som, rear-facing

Combinando estes dois sensores é bastante eficaz porque compensam mutuamente as suas limitações físicas. O LiDAR (baseado em luz) tem elevada precisão, mas pode ter dificuldades com vidro ou superfícies refletoras; o sensor ultrassônico (baseado em som) é mais lento, mas destaca-se na detecção de objetos sólidos independentemente da sua transparência ou cor.
- **Redução de Carga (Pré-filtragem de Dados):** Em vez de enviar pontos de distância brutos de ambos os sensores através do CAN Bus (o que saturaria a largura de banda), o microcontrolador local executa a validação de objetos. Isto é, apenas transmite uma única mensagem CAN se ambos os sensores concordarem na detecção de um alvo ou se o LiDAR detectar uma aproximação a alta velocidade.
- **Índice de "Confiança" no Ângulo Morto:** Ao fundir os dados, é possível criar um nível de confiança. Se o ultrassônico detectar algo a 1 metro, mas o LiDAR estiver cego(e.g., encadeamento), o sistema atribui um alerta de confiança média. Se ambos detectarem o objeto, o alerta é crítico.
- **Filtragem Ambiental:** Sensores ultrassônicos captam frequentemente ruído do solo ou vegetação alta. É possível usar a nuvem de pontos precisa do LiDAR para "mascarar" o solo, instruindo o sensor ultrassônico a ignorar ecos provenientes de um determinado ângulo vertical, reduzindo drasticamente os falsos positivos.
- **Escalonamento Dinâmico de Alcance:** O LiDAR é usado para detecção de longo alcance e o ultrassônico para distâncias mais curtas. O CAN Bus recebe apenas uma mensagem baseada nos dados mais relevantes.

### Outros sensores e serviços físicos
- **Assistência preditiva do Motor (Frequência Cardíaca + Peso):** Ao fundir dados de **frequência cardíaca** com a **célula de carga (peso)**, a bicicleta consegue detectar "Esforço do Condutor". Se a frequência cardíaca ultrapassar um determinado limiar enquanto o peso da carga for >100kg, o Raspberry Pi envia um comando via CAN Bus para o controlador do motor, aumentando automaticamente o perfil.
- **Encaminhamento V2X Sensível ao Cansaço:** A bicicleta envia uma mensagem para a infraestrutura da cidade indicando "Baixa Energia / Cansaço elevado". A RSU da cidade responde com um percurso que privilegia ciclovias com menor inclinação ou semáforos sincronizados.
### Serviços de infraestrutura & Segurança
- **Mapeamento de Buracos e Qualidade da Estrada (IMU + GPS):** Utilizando a IMU (acelerómetros), é possível detectar impactos verticais bruscos. Estes dados são marcados temporalmente via GPS e enviados para a cloud **LightMobie**. Isto permite criar um "Mapa de Calor" da qualidade das estradas, visível por outros ciclistas nos seus capacetes de AR como um ícone de "Cuidado".
- **Vigilância Anti-Roubo (Câmara + LiDAR):** Quando a bicicleta está estacionada, o LiDAR passa a funcionar como um "Perímetro Virtual". Se o LiDAR detectar movimento dentro de 30 cm e a câmara identificar um rosto humano que não seja reconhecido como o do proprietário, é ativado um alerta celular e é gravado um clip de X segundos.
- **VAM (Mensagem de Consciência de Utilizadores Vulneráveis da Via):** A bicicleta utiliza a **câmara** para detectar o olhar do ciclista. Se a câmara identificar que o ciclista está a olhar para a esquerda, mas o **LiDAR traseiro** detectar um autimóvel a ultrapassar pela direita, o sistema V2X envia um alerta para o veículo para reduzir a velocidade, enquanto o capacete de AR faz piscar um aviso vermelho no lado direito da viseira.










## Intelligent Mobility Services: Data-Driven E-Bike Ecosystem

The fusion of on-board telemetry with V2X connectivity transforms the bicycle into a "Mobile Sensing Node." Below are the architectural possibilities for your thesis.

### 1. Sensor Fusion: Rear-Facing LiDAR & Ultrasonic (High-Fidelity Proximity)

Combining these two sensors is highly effective because they compensate for each other's physical weaknesses. LiDAR (Light-based) has high precision but can struggle with glass or highly reflective surfaces; Ultrasonic (Sound-based) is slower but excels at detecting solid objects regardless of their transparency or color.

- **Load Reduction (Data Pre-Filtering):** Instead of sending raw distance points from both sensors over the CAN Bus (which would flood the 500kbps or 1Mbps bandwidth), the local microcontroller performs **Object Validation**. It only broadcasts a single CAN Message (`0x300 - REAR_OBSTACLE`) if _both_ sensors agree on a target or if the LiDAR detects a high-speed approach.
    
- **Blind Spot "Confidence" Index:** By fusing the two, you can create a "Confidence Level." If the Ultrasonic detects something at 1 meter but the LiDAR is blind (perhaps due to glare), the system assigns a "Medium Confidence" alert. If both detect the object, it's "Critical."
    
- **Environmental Filtering:** Ultrasonic sensors often pick up "ground noise" or tall grass. You can use the LiDAR’s precise point cloud to "mask" the ground, telling the Ultrasonic sensor to ignore any echoes coming from a specific vertical angle, drastically reducing false positives.
    
- **Dynamic Range Scaling:** Use the LiDAR for long-range detection (e.g., a car 20 meters away) and the Ultrasonic for ultra-close range (e.g., parking or filtering through traffic at <2 meters). The CAN Bus only receives one "Zone Alert" message based on the most relevant data.
    

---

### 2. Physical & Physiological Services (The Human Element)

- **Predictive Engine Assistance (Heart Rate + Weight):** By fusing **Heart Rate** with the **Load Cell (Weight)** data, the bike can detect "Rider Strain." If the heart rate exceeds a threshold while the cargo weight is >100kg, the Raspberry Pi 5 sends a command via CAN Bus to the motor controller to increase the torque profile automatically.
    
- **Fatigue-Aware V2X Routing:** The bike sends a message to the Smart City infrastructure (V2X) indicating "Low Energy/High Fatigue." The city's RSU (Road Side Unit) responds with a route that prioritizes cycling lanes with lower inclines or synchronized green lights (Green Wave).
    

### 3. Infrastructure & Safety Services (The Environmental Element)

- **Pothole/Road Quality Mapping (IMU + GPS):** Using the IMU (Accelerometers), you can detect sharp vertical impacts. This data is timestamped via GPS and uploaded to the **LightMobie** cloud. This creates a "Heatmap" of road quality that other cyclists can see in their AR helmets as a "Caution" icon.
    
- **Anti-Theft Surveillance (Cam + LiDAR):** When the bike is parked (detected via CAN Bus power state), the LiDAR becomes a "Virtual Perimeter." If the LiDAR detects movement within 30cm and the Camera identifies a human face not recognized as the owner, it triggers a cellular alert and records a 10-second clip.
    
- **VAM (Vulnerable Road User Awareness Message):** The bike uses the **Camera** to detect the rider's gaze. If the camera sees the rider is looking left, but the **Rear LiDAR** detects a car passing on the right, the V2X system sends a "High Priority" alert to the car to slow down, while the AR helmet flashes a red warning on the right side of the visor.