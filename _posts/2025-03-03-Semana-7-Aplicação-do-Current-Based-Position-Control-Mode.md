---
title: "Semana 7 - Aplicação do Current based Position Control Mode"
date: "2025-03-03"
categories: Programação
tags: [Programação, ROS, Grasping]
math: true

---

A função de *grasping* de objetos é fundamental no desenvolvimento de uma mão robótica, exigindo um controlo preciso da posição, velocidade e força aplicada. Ao longo desta semana, foi analisada a possibilidade de controlar a mão utilizando um modo de controlo de velocidade com corrente limite. Além disso, foi implementado o *Current-based Position Control Mode*, que permite definir uma posição desejada para os motores Dynamixel, ao mesmo tempo que se estabelece um limite de corrente, tal como foi abordado na semana anterior.

## É possível implementar um modo de controlo com velocidade e corrente limite?

Não. Os motores Dynamixel oferecem vários modos de controlo, mas nenhum deles permite definir simultaneamente uma velocidade e uma corrente desejada. Durante esta semana, foi analisada a possibilidade de combinar dois modos de controlo: o *Velocity Control Mode* e o *Current Control Mode*. No entanto, essa abordagem exigiria a alteração do modo de controlo dos motores Dynamixel durante a execução do programa, o que implica a desativação breve do torque de todos os motores para efetuar a mudança. Esse processo poderia causar variações indesejadas nas posições dos motores, comprometendo a estabilidade e precisão do sistema.

## Aplicação do *Current-based Position Control Mode*

Dado que não foi possível implementar um modo de controlo que permitisse definir simultaneamente a velocidade e a corrente desejada, optou-se pela utilização do *Current-based Position Control Mode*. Este modo permite comandar os motores para uma posição específica, respeitando um limite de corrente previamente definido. Caso exista um obstáculo no percurso, o motor interrompe o movimento e aplica torque até atingir a corrente limite, sem a ultrapassar.

Embora este modo não permita um controlo direto da velocidade, é possível definir uma velocidade máxima através do endereço `Profile Velocity`. Assim, os motores deslocam-se até à posição desejada com a velocidade máxima estipulada, mas garantindo sempre que a corrente nunca excede o valor definido.

### Gráficos de posição, velocidade e corrente sem obstáculo

Nas figuras seguintes, apresentam-se os gráficos da posição, velocidade e corrente ao longo do tempo para os quatro motores de um dedo. Durante a experiência, foi enviado o comando para fechar o dedo e, em seguida, reabri-lo. Os valores definidos para a corrente limite e a velocidade máxima foram os seguintes:

```python

#Endereços
ADDR_GOAL_POSITION = 116
ADDR_GOAL_CURRENT = 102
ADDR_PROFILE_VELOCITY = 112

#Valores definidos
GOAL_CURRENT_VALUE = 100 # mA
PROFILE_VELOCITY_VALUE = 50 # aproximadamente 1.2 rad/s

```


<figure style="text-align: center;">
    <img src="/assets/images/semana7/finger_positions.png" alt="Posições dos motores ao longo do tempo" style="height: 300px;">
    <figcaption style="margin-bottom: 30px;">Posições dos motores ao longo do tempo</figcaption>
</figure>

<figure style="text-align: center;">
    <img src="/assets/images/semana7/finger_velocities.png" alt="Velocidades dos motores ao longo do tempo" style="height: 300px;">
    <figcaption style="margin-bottom: 30px;">Velocidades dos motores ao longo do tempo</figcaption>
</figure>

<figure style="text-align: center;">
    <img src="/assets/images/semana7/finger_currents.png" alt="Correntes consumidas pelos motores ao longo do tempo" style="height: 300px;">
    <figcaption style="margin-bottom: 30px;">Correntes consumidas pelos motores ao longo do tempo</figcaption>
</figure>

Como é possível observar, durante todo o movimento, a corrente desejada nunca foi ultrapassada, assim como a velocidade. No entanto, no motor 1, verificou-se uma pequena variação, onde a velocidade excedeu em apenas 0.07 rad/s o valor máximo definido. Essa oscilação é esperada, pois pequenas variações na velocidade são normais no funcionamento dos motores.

Além disso, ao analisar a curva de movimento dos motores, foi possível recolher um total de 62 pontos desde o instante em que o dedo se começou a mover até atingir a posição desejada. No entanto, este valor deverá variar consoante a velocidade máxima definida.

### Gráficos de posição, velocidade e corrente com obstáculo

Após a análise dos gráficos de posição, velocidade e corrente consumida pelos motores de um dedo, repetiu-se a experiência anterior, desta vez com um objeto a impedir o dedo de atingir a posição desejada. De seguida, voltou-se a enviar o comando de abrir e fechar o dedo sem obstáculo com o objetivo de comparar o comportamento dos motores nas duas situações.

Nas figuras seguintes, apresentam-se os gráficos de posição, velocidade e corrente consumida para esta nova experiência, mantendo-se todos os valores definidos anteriormente.

<figure style="text-align: center;">
    <img src="/assets/images/semana7/finger_positions_1.png" alt="Posições dos motores ao longo do tempo" style="height: 300px;">
    <figcaption style="margin-bottom: 30px;">Posições dos motores ao longo do tempo</figcaption>
</figure>

<figure style="text-align: center;">
    <img src="/assets/images/semana7/finger_velocities_1.png" alt="Velocidades dos motores ao longo do tempo" style="height: 300px;">
    <figcaption style="margin-bottom: 30px;">Velocidades dos motores ao longo do tempo</figcaption>
</figure>

<figure style="text-align: center;">
    <img src="/assets/images/semana7/finger_currents_1.png" alt="Correntes consumidas pelos motores ao longo do tempo" style="height: 300px;">
    <figcaption style="margin-bottom: 30px;">Correntes consumidas pelos motores ao longo do tempo</figcaption>
</figure>



A análise dos gráficos revela diferenças significativas no comportamento do motor entre as condições com e sem obstáculo. Durante a primeira tentativa de fecho do dedo, a presença do obstáculo impede a conclusão do movimento, resultando na estabilização prematura da posição e na anulação da velocidade. Paralelamente, observa-se um aumento acentuado da corrente, refletindo o esforço do motor para superar a resistência imposta. Após a remoção do obstáculo, o movimento ocorre sem restrições, permitindo que a posição desejada seja alcançada com uma trajetória contínua, uma velocidade mais estável e um consumo de corrente inferior. 

Estes resultados demonstram a eficácia do *Current-based Position Control Mode*, uma vez que permitem a definição de um limite de corrente, garantindo que o motor não exerça forças excessivas durante a interação com objetos. Esta capacidade é particularmente relevante para aplicações que envolvem a manipulação de objetos delicados, pois possibilita a adaptação dinâmica da corrente limite conforme a rigidez e a sensibilidade dos materiais, evitando deformações ou danos.


## Código desenvolvido durante a semana

Durante esta semana foi senvolvido o seguinte código:
- `manager_node`: foi modificado  de forma a utilizar o *Current-based Position Control Mode*
- `read_positions_node`, `read_velocities_node` e `read_currents_node`: modificados de forma a guardar os valores recebidos para posterior análise.
- `pos_graph.py`,`pos_vel.py` e `pos_curr.py`: scripts python desenvolvidos para ler os dados guardados, efetuar as conversões e gerar os gráficos

### Trabalho ainda em desenvolvimento

De momento os nós `read_positions_node`, `read_velocities_node` e `read_currents_node` ainda se encontram a ser alterados para gerarem gráficos em tempo real.