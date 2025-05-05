---
title: "Semana 14 e 15 - Implementação dos sensores de conntacto"
date: "2025-05-05"
categories: [Programação,Montagem,Materiais]
tags: [Programação, Montagem]
math: true

---

Nas últimas duas semanas, o trabalho no projeto centrou-se na integração dos sensores de contacto nas pontas dos dedos da mão robótica. Este processo exigiu várias etapas, incluindo o desenho de uma peça em 3D para alojar o microcontrolador e o PCB, a colagem dessa peça à estrutura da palma da mão, a modificação da própria palma para permitir a passagem do cabo de ligação entre o microcontrolador e o computador, e, por fim, o desenho do circuito do PCB, de modo a obter um esquema de ligações que otimize o espaço disponível.

## Desenho da peça 3D

Após a avaliação do espaço disponível na estrutura da palma da mão, decidiu-se posicionar tanto o microcontrolador como o PCB na vertical, ou seja, perpendiculares à superfície da peça. Para viabilizar esta configuração, foi desenhada uma peça de suporte com duas ranhuras, capazes de manter ambos os componentes estáveis durante os movimentos da mão. Simultaneamente, esta solução permite a remoção fácil de qualquer um dos elementos, sem necessidade de ferramentas ou esforço excessivo. O desenho desta peça foi realizado com o auxílio da ferramenta Tinkercad, sendo apresentado na figura seguinte:

<div style="text-align: center;">
    <img src="/assets/images/semana14/peca3d.jpg" alt="Texto alternativo" width="300">
    <figcaption>Peça desenhada com a ferramenta Tinkercad</figcaption>
</div>

## Aplicação da peça na palma da mão

Para a aplicação da peça na palma da mão, foi necessário fixá-la de forma segura. Para tal, utilizou-se cola epóxi, garantindo a aderência necessária. Além disso, para permitir a passagem do cabo que conecta o microcontrolador ao computador, foi necessário realizar uma ampliação na furação existente na peça. A furação original não era suficientemente grande para acomodar o cabo adicional, o que exigiu a modificação para garantir o seu correto posicionamento.


<div style="text-align: center;">
    <img src="/assets/images/semana14/colagem.jpg" alt="Texto alternativo" width="300">
    <figcaption>Peça já colada na palma da mão</figcaption>
</div>

<div style="text-align: center;">
    <img src="/assets/images/semana14/furacao.jpg" alt="Texto alternativo" width="300">
    <figcaption>Furação efetuada na peça da palma da mão</figcaption>
</div>


## Desenho do PCB

Para realizar as ligações necessárias ao circuito, foram utilizados dois DIP sockets de 9 pinos, 5 resistências de 22 kΩ e um pente de conectores jumper fêmea, também com 9 pinos. No entanto, o desenho das ligações ainda não é apresentado, uma vez que ainda não foi encontrada uma ferramenta apropriada para desenhar as ligações na placa.