---
title: "Semana 1 - Materiais"
date: "2024-10-07"
categories: [Materiais]
tags: [Materiais]

---

Este é o primeiro post de uma série semanal na qual irei relatar o progresso da minha dissertação de mestrado, cujo tema é o desenvolvimento de uma mão robótica antropomórfica que irá baseada no projeto LEAP Hand. Este projeto tem como objetivo a construção de uma prótese robótica funcional, com foco na precisão dos movimentos e na resposta sensorial eficiente para posteriormente ser utilizada em projetos para a utilização de algoritmos de *reinforcement learning* na tarefa de "*grasping*" de objetos.

![Imagem com a LEAP Hand](/assets/images/leap_hand.png)


Nesta fase inicial, o trabalho está concentrado na elaboração de uma lista de materiais e na pesquisa de sensores apropriados para a integração ao sistema uma vez que estes sensores irão ser cruciais no desenvolvimento do "*tato*" desta mão.

## Materiais necessários para a montagem dos motores 

| Componente | Quantidade |
|:-------------|:-------------:|
| Dynamixel XC330-M288-T | 16 |
| Dynamixel FPX330-H101 | 11 |
| Dynamixel FPX330-101 | 12 |
| Dynamixel FPX330-S103 | 6 |
| Dynamixel U2D2 | 1 |
| Dynamixel Power Hub| 1 |
| XT30 Pigtail | 1 |

## Incorporação de sensores

Após realizar uma pesquisa sobre sensores utilizados em mãos robóticas antropomórficas, observou-se a existência de diversas soluções comerciais (como o *BioTac fingertip*) e também foram desenvolvidas várias abordagens por diferentes autores, que utilizam técnicas variadas, como, por exemplo, sensores baseados no efeito Hall[^1]. No entanto, estas soluções tornam-se dispendiosas e muito complexas de integrar noutros projetos. 

<div style="display: flex; justify-content: space-around;text-align: center;">
    <figure style="margin: 0;">
        <img src="/assets/images/biotac.jpeg" alt="Biotac fingertip" style="height: 200px; object-fit: cover;">
        <figcaption style="margin-bottom: 20px;"><i>Biotac fingertip</i> desenvolvida por <i>Syntouch</i>.</figcaption>
    </figure>

    <figure style="margin: 0;">
        <img src="/assets/images/hall_sensor.png" alt="Sensores desenvolvidos com base no efeito de Hall" style="height: 200px; object-fit: cover;">
        <figcaption style="margin-bottom: 20px;">Sensores desenvolvidos com base no efeito de Hall [1].</figcaption>
    </figure>
</div>



Uma alternativa de baixo custo e relativamente fácil de incorporar é o sensor FSR (*Force Sensing Resistor*) que permite a deteção de força ou pressão aplicada numa superfície. O princípio de funcionamento destes sensores é baseado na variação da resistência elétrica de materiais poliméricos quando são deformados ou comprimidos.

<figure style="text-align: center;">
    <img src="/assets/images/fsr.jpg" alt="Descrição da Imagem" style="height: 300px;">
    <figcaption>Diferentes sensores FSR.</figcaption>
</figure>






## Referências

[^1]: JAMONE, Lorenzo, et al. Highly sensitive soft tactile sensors for an anthropomorphic robotic hand. IEEE sensors Journal, 2015, 15.8: 4226-4233