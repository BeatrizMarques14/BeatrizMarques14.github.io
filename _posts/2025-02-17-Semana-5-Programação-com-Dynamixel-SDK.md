---
title: "Semana 5 - Programação com Dynamixel SDK"
date: "2025-02-17"
categories: [Materiais, Análise de Dados]
tags: [Materiais, Análise de Dados]
math: true

---

O Dynamixel SDK é uma biblioteca multiplataforma que permite a comunicação e o controlo eficiente dos motores Dynamixel em várias linguagens de programação, incluindo Python, C, C++ e Java. Projetado para oferecer uma interface unificada, o SDK simplifica a leitura e escrita de dados nos motores, suportando diferentes protocolos de comunicação (neste caso utiza-se o protocolo 2.0 para os motores XC330-M288-T) e facilitando a configuração de parâmetros como velocidade, posição e torque. A sua versatilidade torna-o uma ferramenta essencial para aplicações em robótica, automação e pesquisa, permitindo um controlo preciso e personalizado dos atuadores em projetos complexos.

Neste post irá ser explicado como é que é possível ler a posição, velocidade e PWM de um motor através da utilização da biblioteca Dynamixel SDK integrada com o ROS1 e também com o ROS2.


## Parâmetros

Para enviar comandos e ler os parâmetros desejados dos motores Dynamixel, é essencial configurar corretamente algumas variáveis que estabelecem a comunicação. Isso inclui definir a porta serial utilizada (normalmente é *'/dev/ttyUSB0'*), a taxa de transmissão (baud rate), o protocolo de comunicação (1.0 ou 2.0), a série do motor (neste caso será a série X) e o ID do motor, que identifica cada atuador na rede.

```python
from dynamixel_sdk import *  # Biblioteca Dynamixel SDK

# Configurações do Dynamixel
MY_DXL = "X_SERIES"
BAUDRATE = 57600
PROTOCOL_VERSION = 2.0
DXL_ID = 3
DEVICENAME = '/dev/ttyUSB0'
```
Os motores Dynamixel armazenam e gerem os seus parâmetros através de uma tabela de controlo de memória, onde cada parâmetro, como posição, velocidade e torque, está associado a um endereço específico. Esses endereços permitem a leitura e escrita de dados no motor através de comandos enviados pelo controlador. A tabela é dividida em duas áreas principais: EEPROM, que contém configurações permanentes (como ID do motor e baud rate) e só pode ser modificada após reinicialização, e RAM, onde estão os parâmetros dinâmicos (como posição atual e temperatura) e que são atualizados em tempo real. Para interagir com esses endereços, o Dynamixel SDK utiliza funções de leitura e escrita, permitindo configurar e monitorizar o estado do motor de forma precisa e eficiente. 

A tabela seguinte apresenta alguns dos endereços utilizados para enviar comandos de controlo e ler parâmetros no código desenvolvido. Esses endereços correspondem a posições específicas na memória do motor, permitindo a configuração e monitorização de variáveis essenciais, como posição, velocidade e torque.

| **Endereço** | **Parâmetro**            | **Tamanho (bytes)** | **Intervalo de valores**  |  
|-------------|-------------------------|--------------------|-----------------|  
| 36          | PWM Limit              | 2                  | 0-885 |  
| 44           | Velocity Limit                | 4                  | 0-2047 |  
| 48          | Max Position Limit            | 4                  | 0-4095  |  
| 52          | Min Position Limit         | 4             | 0-4095  |  
| 64          | Torque Enable        | 1                  | 0/1  |  
| 100          | Goal PWM            | 2                  | -PWM Limit - PWM Limit  |  
| 104          | Goal Velocity         | 4                  | -Velocity Limit - Velocity Limit  |  
| 116          | Goal Position          | 4                  | Min Position Limit - Max Position Limit  |  
| 124          | Present PWM        | 2                  | -  |  
| 128         | PresentVelocity            | 4                  | -  |
| 132           | Present Position          | 4                 | - |  

Para além destes endereços, existem muitos outros parâmetros que podem ser configurados e lidos para um controlo mais detalhado do motor. No entanto, até agora, estes foram os principais utilizados no desenvolvimento. A lista completa de parâmetros disponíveis pode ser consultada na [tabela de controlo do Dynamixel XC330-M288-T](https://emanual.robotis.com/docs/en/dxl/x/xc330-m288/).

### Posição

Os motores Dynamixel utilizam um sistema de posicionamento baseado em ticks, onde a posição do motor é representada por um número inteiro dentro de um intervalo definido pelo encoder interno.

No caso do Dynamixel XC330-M288-T, a posição é representada com 12 bits, permitindo um total de $2^{12} = 4096 $ valores distintos. Assim, o motor usa um intervalo de 0 a 4095 ticks para representar uma rotação completa de 360°.

#### Conversão de posição

Como o intervalo 0 a 4095 representa 0° a 360°, é bastante simples de realizar a conversão de ticks para º. Se tivermos um valor de ticks, a posição em graus pode ser calculada por:

$$
\theta = \frac{\text{ticks} \times 360}{4096}
$$

Onde $\theta$ é a posição do motor em graus e *ticks* é o valor lido do motor (entre 0 e 4095)

### Velocidade

Nos motores Dynamixel XC330-M288-T, a velocidade de rotação é representada por um valor numérico discreto, denominado ticks. A relação entre os ticks e a velocidade real do motor é determinada por um fator de conversão fixo, estabelecido pelo fabricante.

A velocidade real do motor é expressa em rotações por minuto (RPM), e a conversão entre ticks e RPM pode ser realizada utilizando a equação:

$$
RPM=ticks×0.229
$$

Nesta equação, o fator 0.229 representa a velocidade angular correspondente a um único tick, fornecido pelo fabricante do motor. Assim, ao atribuir um valor de 500 ticks, a velocidade resultante será de aproximadamente 114.5 RPM.

Além disso, os motores Dynamixel permitem definir o sentido de rotação a partir do sinal do valor atribuído à velocidade. Valores positivos correspondem a uma rotação no sentido horário, enquanto valores negativos resultam em uma rotação no sentido anti-horário. 

### PWM

Nos motores Dynamixel XC330-M288-T, o valor de PWM (Pulse Width Modulation) representa diretamente a potência elétrica aplicada ao motor, controlando a força gerada para o movimento.
O motor aceita valores de PWM expressos em ticks, variando entre -885 e 885, onde:

- Valores positivos aplicam potência para rotação no sentido horário.
- Valores negativos aplicam potência para rotação no sentido anti-horário.
- O valor 0 significa que não há força sendo aplicada ao motor.

A relação entre o valor de PWM e a potência aplicada pode ser expressa como uma fração da potência máxima do motor, usando a seguinte equação:

$$
\text{Potência Aplicada (%)} = \left(\frac{\text{PWM (ticks)}}{885}\right) \times 100
$$

Por exemplo, ao definir PWM = 442, o motor recebe aproximadamente 50% da potência máxima disponível. Se for atribuído PWM = -885, o motor operará com 100% da potência no sentido oposto.

## Código desenvolvido em ROS

Nesta secção do post, serão apresentados os procedimentos essenciais para estabelecer comunicação com os motores através do ROS. Dado que o código foi desenvolvido tanto para ROS 1 como para ROS 2, esta secção será dividida em duas partes. Todo o código-fonte pode ser consultado no [repositório do GitHub](https://github.com/BeatrizMarques14/leap_control), onde foram criadas duas branches distintas: uma para ROS 1 e outra para ROS 2.

Embora o código tenha sido desenvolvido para ROS 1 e ROS 2, existem passos fundamentais comuns a ambas as versões: a importação das bibliotecas do Dynamixel SDK e a inicialização da porta de comunicação através das seguintes funções:

```python
from dynamixel_sdk import *  # Biblioteca Dynamixel SDK

portHandler = PortHandler(DEVICENAME)  
packetHandler = PacketHandler(PROTOCOL_VERSION)

```

Após a inicialização, é essencial ativar o torque do motor para que este possa receber comandos. A partir desse momento, podem ser enviados comandos de controlo, como a definição da posição, velocidade ou PWM, dependendo do modo de operação configurado. Esses valores são escritos nos registos internos do motor utilizando as seguintes funções do SDK:

- `write1ByteTxRx()` para valores de 1 byte
- `write2ByteTxRx()` para valores de 2 bytes
- `write4ByteTxRx()` para valores de 4 bytes

Além do envio de comandos, é igualmente importante ler o estado do motor, obtendo parâmetros como posição atual, velocidade e PWM. Para isso, utilizam-se as funções `read2ByteTxRx()` e `read4ByteTxRx()`, garantindo que os valores lidos são interpretados corretamente.

### ROS1

Para ROS 1, foram desenvolvidos dois nós individuais: um para ler a posição e outro para ler a velocidade. No entanto, como apenas um nó pode comunicar diretamente com a porta serial, não é possível utilizar esses dois nós simultaneamente para obter ambas as informações ao mesmo tempo.

Para resolver essa limitação, foi criada a pasta `multiple_reading_from_one_motor`, onde foram desenvolvidos cinco nós que permitem uma leitura eficiente e estruturada:

- `manager.py`: Nó responsável por comunicar diretamente com a porta serial. Este nó lê os dados do motor e envia comandos (como definição da posição) através da publicação e subscrição de tópicos.
- `set_position.py`: Publica no tópico set_position a posição desejada para o motor.
- Três nós de leitura:
  * Um subscreve o tópico read_position para obter a posição atual do motor.
  * Outro subscreve o tópico read_velocity para obter a velocidade.
  * O terceiro subscreve o tópico read_pwm para obter o valor de PWM aplicado.

Com esta estrutura, o nó manager.py centraliza a comunicação com o motor, evitando conflitos na porta serial, enquanto os outros nós interagem com ele por meio de tópicos ROS, garantindo uma implementação modular e eficiente.

### ROS2

Para ROS 2, até o momento, foram desenvolvidos três nós individuais:

- Um nó para ler a posição do motor.
- Um nó para ler a velocidade.
- Um nó para ler o PWM aplicado.

Embora esta abordagem funcione para leituras isoladas, tal como em ROS 1, existe a limitação de que apenas um nó pode comunicar diretamente com a porta serial. Dessa forma, ainda não foi implementada uma solução que permita a leitura simultânea de múltiplos parâmetros sem conflitos na comunicação.

### Diferença na implementação entre ROS1 e ROS2

A principal diferença entre o desenvolvimento dos nós em ROS1 e ROS2 está na forma como os nós Python são estruturados.

Em ROS1, os nós são implementados utilizando a biblioteca `rospy`, e a estrutura básica de um nó inclui:
- Inicialização do nó com `rospy.init_node()`.
- Utilização de `rospy.Subscriber()` e `rospy.Publisher()` para comunicação entre nós.
- Um loop baseado na taxa definida por `rospy.Rate()`.

Já em ROS2, os nós são implementados utilizando `rclpy`, que apresenta algumas diferenças importantes:
- A criação de um nó requer a definição de uma classe que herda de Node.
- Os publishers e subscribers são criados como atributos da classe e inicializados dentro do construtor.
- Em vez de um loop manual, utilizam-se executores (`rclpy.spin()`), que gerem automaticamente os callbacks dos tópicos.

Adicionalmente, o ROS 2 tem um sistema de comunicação mais eficiente, suportando DDS (Data Distribution Service), o que permite maior flexibilidade e desempenho na transmissão de mensagens. No entanto, essas mudanças exigem ajustes na implementação dos nós para garantir compatibilidade com a nova estrutura do ROS 2.

