---
title: "Semana 10 - Grasping com dois dedos"
date: "2025-03-24"
categories: [Programação]
tags: [Programação, ROS, Grasping]
math: true

---

Durante a semana passada, foi concluída a construção do polegar da LEAP Hand. Assim, o foco desta semana centrou-se na programação da mão para realizar *grasping* com dois dedos, permitindo, pela primeira vez, segurar objetos sem qualquer auxílio externo.

## Teste de *grasping* com um dedo

No post da semana 8, abordou-se o desenvolvimento do nó `grasping_node`, responsável por detetar a interseção de objetos e ajustar a corrente para evitar que sejam esmagados. No entanto, não foram apresentados testes experimentais que comprovassem esse comportamento. Para colmatar essa lacuna, as figuras abaixo mostram os gráficos de posição, velocidade e corrente para duas condições distintas: primeiro sem obstáculo e depois com obstáculo. Definiu-se uma corrente máxima de 200 mA quando não há obstáculo, permitindo que o dedo se mova livremente com a velocidade desejada, e uma corrente máxima de 100 mA para quando o dedo deteta um obstáculo, garantindo uma interação mais segura e controlada.

### Gráficos de posição, velocidade e corrente obtidos

Posições

<div style="text-align: center;">
  <table>
    <tr>
      <td>
        <figure>
          <img src="/assets/images/semana10/finger_pos_vel50_no_obs.png" alt="1 KOhm" width="200">
          <figcaption>Posições sem obstáculo</figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana10/finger_pos_vel50_obs.png" alt="10 KOhm" width="200">
          <figcaption>Posições com obstáculo</figcaption>
        </figure>
      </td>
    </tr>
  </table>
</div>

Velocidades

<div style="text-align: center;">
  <table>
    <tr>
      <td>
        <figure>
          <img src="/assets/images/semana10/finger_vels_vel50_no_obs.png" alt="1 KOhm" width="200">
          <figcaption>Velocidades sem obstáculo</figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana10/finger_vels_vel50_obs.png" alt="10 KOhm" width="200">
          <figcaption>Velocidades com obstáculo</figcaption>
        </figure>
      </td>
    </tr>
  </table>
</div>

Correntes

<div style="text-align: center;">
  <table>
    <tr>
      <td>
        <figure>
          <img src="/assets/images/semana10/finger_currs_vel50_no_obs.png" alt="1 KOhm" width="200">
          <figcaption>Correntes consumidas sem obstáculo</figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana10/finger_currs_vel50_obs.png" alt="10 KOhm" width="200">
          <figcaption>Correntes consumidas com obstáculo</figcaption>
        </figure>
      </td>
    </tr>
  </table>
</div>

### Conclusões

A análise dos gráficos apresentados revela a ocorrência de picos de corrente superiores a 100 mA enquanto o dedo se movimenta livremente, sem encontrar obstáculos. No entanto, a certo momento, observa-se que a corrente se estabiliza nos 100 mA, indicando que o dedo detetou a interseção com um objeto e ajustou automaticamente a corrente limite para evitar exercer força excessiva sobre ele. Após a libertação do objeto, registam-se novamente picos de corrente superiores a 100 mA, permitindo que o dedo retome o seu movimento sem restrições.

## Teste de *grasping* com dois dedos

Após as alterações implementadas no código, foram realizados testes de grasping de objetos utilizando dois dedos. No vídeo seguinte, demonstra-se que a manipulação com dois dedos permite segurar um objeto de forma estável, sem que este caia. No entanto, ainda não é possível apresentar os gráficos de posição, velocidade e corrente, uma vez que o código de análise desses dados ainda não foi adaptado para múltiplos dedos.

<div style="text-align: center;">
  <a href="https://www.youtube.com/watch?v=Q50j53qd_OI">
    <img src="https://img.youtube.com/vi/Q50j53qd_OI/0.jpg" alt="Texto alternativo">
  </a>
</div>

[Link do vídeo](https://www.youtube.com/watch?v=Q50j53qd_OI)

## Código desenvolvido durante a semana


 Durante esta semana, foram desenvolvidas e implementadas várias melhorias no código, nomeadamente:

- Desenvolvimento do nó `thumb_manager`, responsável por receber as informações de posição, velocidade e corrente dos motores associados ao polegar, além de enviar os comandos de corrente limite para o `manager_node`, permitindo o controlo do torque exercido durante o *grasping de objetos*;
- Modificação do nó `manager_node` para detetar automaticamente quais os dedos ativos;
- Atualização dos nós `middle_manager` e `thumb_manager` para publicarem no tópico `set_currents`, de forma a transmitirem as correntes desejadas.
- Alteração do nó `manager_node` para subscrever o tópico `set_currents` e enviar as correntes definidas para os motores do dedo correspondente.
- Alteração do nó `set_positions`, para permitir o controlo do polegar e a realização de comandos globais para a mão, como *hand close* e *hand open*.

### Esquema do código desenvolvido

<figure style="text-align: center;">
    <img src="/assets/images/semana10/ros_graph_2.png" alt="Organização do código desenvolvido" style="height: 100px;">
    <figcaption style="margin-bottom: 30px;">Organização do código desenvolvido</figcaption>
</figure>