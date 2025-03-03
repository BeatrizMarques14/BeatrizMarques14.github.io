---
title: "Semana 6 - Controlo de um dedo"
date: "2025-02-24"
categories: Programação
tags: [Programação, ROS]
math: true

---

Nesta semana, foram aprofundadas algumas questões teóricas que não ficaram bem clarificadas na semana anterior. Além disso, foi desenvolvido código em ROS 2 para controlar um dedo da mão robótica, enquanto se realizavam, simultaneamente, leituras de posição, velocidade e corrente de todos os motores.

## O que é o PWM e como controla o motor?

PWM significa *Pulse Width Modulation* e refere-se ao sinal de modulação de largura de pulso utilizado para controlar a potência fornecida ao motor ao variar o tempo em que a tensão fica ativa (*duty cycle*) dentro de um período fixo.

Nos motores dynamixel, o motor não recebe uma tensão contínua pois recebe pulsos rápidos de tensão que ligam e desligam repetidamente. No controlo a partir do PWM, determina-se a fração do tempo em que a tensão está ativa dentro de um ciclo podendo-se assim controlar a potência média fornceda ao motor.

A duração e a frequência do *duty cycle* nos motores dynamixel XC330-M288-T não é especificada na documentação da Dynamixel. No entanto, em modelos anteriores a frequência típica do PWM rondava os 20 KHz. Desta forma, para controlar o motor a partir do PWM, apenas se define a a fração do tempo em que a tensão está ativa dentro de um ciclo. Como o PWM varia entre 885 e -885, se definirmos o valor do PWM de 442, significa que o motor receve 5V apenas 50% do tempo, resultando em metade da potência aplicada.

### Existe relação entre o PWM e a corrente consumida?

Sim, existe uma relação entre o PWM aplicado e a corrente consumida pelo motor. No entanto, essa relação não é linear.

Uma vez que o PWM controla a tensão aplicada ao motor e a tensão afeta a corrente consumida, então o PWM também afeta a corrente consumida. Quanto maior for o PWM, maior a fração do tempo em que o motor recebe 5V e por isso, mais corrente irá consumir.

## É possível calcular o torque a partir da corrente consumida?

A partir da documentação do motor Dynamixel XC330-M288-T é possível observar o gráfico de performance do motor onde se observa a relação entre a corrente consumida (em A) e o torque (em N*m).

<figure style="text-align: center;">
    <img src="/assets/images/performance_graph.png" alt="Gráfico de performance do motor Dynamixel XC330-M288-T" style="height: 300px;">
    <figcaption style="margin-bottom: 30px;">Gráfico de performance do motor Dynamixel XC330-M288-T</figcaption>
</figure>

Através da análise do gráfico é possível observar que a relação entre a corrente consumida e o torque não é completamente linear. No entanto, se considerarmos o *stall Torque* para 5V e 1.8A fornecido também na documentação da Dynamixel, é possível calcular o declive da reta. Neste caso, como o *stall Torque* para 5V e 1.8A é de 0.93 N.m, o declive da reta será dado por: 

$$
m = \frac{0.93 \text{N.m}}{1.8 \text{A}}
$$

No entanto, se compararmos alguns pontos da reta com os valores do gráfico de performance, verifica-se que existe uma diferença significativa nos valores obtidos.

## É possível ler vários parâmetros de vários motores ao mesmo tempo?

Sim, a dynamixel fornece duas funções que permitem operar vários motores simultaneamente:

- `sync_read`: lê o mesmo parâmetro de vários motores ao mesmo tempo (por exemplo, ler a velocidade de 4 motores simultaneamente)
- `bulk_read`: lê diferentes parâmetros de vários motores ao mesmo tempo (por exemplo, ler a velocidade e posição de 2 motores simultaneamente)

No nosso projeto, utilizaremos a função `bulk_read` para obter a posição, a velocidade e a corrente consumida de todos os motores. Além dessas funções de leitura, a Dynamixel também disponibiliza `sync_write` e `bulk_write`, que permitem a escrita de parâmetros em múltiplos motores.

## Modos de operação dos motores XC330-M288-T

Os motores Dynamixel XC330-M288-T oferecem diferentes modos de operação, permitindo adaptá-los a diversas aplicações robóticas. Cada modo define como o motor responde aos comandos de controlo, seja regulando posição, velocidade ou torque. Nesta secção, serão explorados os principais modos de operação disponíveis, destacando as suas características e possíveis utilizações no projeto.

### *Position Control Mode*

- Permite definir uma posição exata para o motor se mover, dentro dos limites definidos.
- No caso de existir um obstáculo, o motor continua a tentar alcançar a posição desejada, aplicando torque até atingir o seu limite máximo.
- Se o motor não conseguir superar o obstáculo, pode sobreaquecer se não existir um limite de cirrente bem ajustado.

### *Extended Position Control Mode*

- Semelhante ao *Position Control Mode*, mas permite rotações superiores a 360º.
- Para o nosso projeto náo é um modo relevante, pois as juntas de um dedo nunca podem realizar rotações superiores a 360º.

### *Velocity Control Mode*

- Define uma velocidade angular constante para o motor girar, sem definir uma posição específica.
- O motor acelera até atingir a velocidade desejada e quando a atinge, mantém-se a rodar.
- No caso de existir um obstáculo, o motor continua a tentar girar, aplicando o torque máximo para tentar manter a velocidade definida.
- Se o motor não conseguir superar o obstáculo, pode sobreaquecer e parar de girar.

### *PWM Control Mode*

- Permite controlar diretamente a potência do motor.
- Não define uma posição ou velocidade específica.
- No caso de existir um obstáculo, o motor vai sempre tentar continuar a ultrapassar o obstáculo. O que pode causar sobreaquecimento.

### *Current Control Mode*

- Controla diretamente o torque aplicado pelo motor, definindo um limite máximo de corrente e um valor de corrente desejado.
- Se não existir resistência, o motor gira continuamente.
- Se existir um obstáculo:
    * O motor não tentar ultrapassar o obstáculo, pois a corrente máxima limita a força aplicada.
    * Se o torque necessário para continuar o movimento for maior do que o permitido, o motor para de se mover e mantém um torque constante, equivalente ao valor de corrente desejado.

### *Current-Based Position Control Mode*

- Semelhante ao *Position Control Mode*, mas permite definir um limite de corrente para limitar o torque aplicado.
- No caso de existir um obstáculo:
    * O motor tenta alcançar a posição desejada, mas apenas até ao limite de corrente definido.
    * Se o torque necessário, for maior do que o permitido, o motor para e mantém um torque constante, sem tentar forçar o movimento.

### Comparação dos modos de operação

| **Modo**                         | **Controla**        | **Ideal para**                              | **Reação a Obstáculos**                           |
|----------------------------------|--------------------|--------------------------------------------|--------------------------------------------------|
| ***Position Control Mode***                      | Ângulo            | Movimentos precisos                        | Continua a tentar alcançar a posição, aplicando torque máximo |
| ***Extended Position Control Mode***            | Ângulo (>360°)    | Movimentos que exigem múltiplas rotações  | Mesmo comportamento do modo de posição |
| ***Velocity Control Mode***                   | Velocidade        | Movimentos contínuos (rodas, esteiras)     | Continua a tentar girar, aplicando torque máximo até parar |
| ***Current Control Mode***                     | Corrente    | Controlo de força (*grasping*)         | Para de se mover e mantém um torque constante contra o obstáculo |
| ***Current-Based Position Control Mode***   | Ângulo + Corrente   | Controlo de posição com limitação de força | Tenta alcançar a posição, mas para se o torque for insuficiente |
| ***PWM Control Mode***                          |Potência     | Controlo manual avançado                  | Depende do controlo externo, pode continuar a tentar girar indefinidamente |

## Código da LEAP Hand

Nesta secção, analisámos o código fornecido pela LEAP Hand para identificar funções que possam ser úteis para o nosso projeto. A partir dessa análise, destacam-se as seguintes observações: 

- Os limites das juntas para evitar colisões encontram-se no script `leap_hand_utils.py`
- `leap_hand_utils.py`: possui os limites das juntas e conversões do simulador para real
- `leap_hand_node.py`: nó que comunica com a porta serial
    * recebe comandos (de posição)
    * envia comandos
    * efetua a leitura
- `dynamixel_client.py`: efetua as configurações iniciais, converte os valores lidos e possui métodos para ler e escrever no motor

## Código desenvolvido durante a semana

Durante esta semana, foram desenvolvidos nós em ROS2 para realizar múltiplas leituras de um motor simultaneamente, tal como já tinha sido feito em ROS1. Além disso, foram adicionados nós para ler a posição, velocidade e corrente de todos os motores de um dedo e controlá-lo com base na posição de todos os motores. Para otimizar a comunicação, foram utilizadas as funções `bulk_read` e `bulk_write`, permitindo a leitura e escrita simultânea de todos os parâmetros em múltiplos motores.

Até ao momento, o `bulk_write` é utilizado apenas para enviar as posições dos motores. No entanto, espera-se implementar o *Current-Based Position Control Mode*, permitindo assim o envio simultâneo das posições e das correntes desejadas para todos os motores.

Nós desenvolvidos para o controlo de um dedo:
- `manager_node`: lê e publica a posição, velocidade e corrente de múltiplos motores em 3 tópicos e permite o envio de novas posições para controlo dos motores, utilizando comunicação `bulk_read` e `bulk_write`
- `read_positions_node`: subscreve o tópico `/dynamixel_finger_positions` que recebe as posições dos motores
- `read_velocities_node`: subscreve o tópico `/dynamixel_finger_velocities` que recebe as velocidades dos motores
- `read_currents_node`: subscreve o tópico `/dynamixel_finger_currents` que recebe as correntes consumidas pelos motores
- `set_positions`: publica as posições dos motores no tópico `/dynamixel_set_finger_positions`, permitindo ao utilizador inserir manualmente os valores desejados pelo terminal ou controlar os motores através de comandos como "*close*" e "*open*"

Todo o código desenvolvido pode ser consultado no [repositório do GitHub](https://github.com/BeatrizMarques14/leap_control)