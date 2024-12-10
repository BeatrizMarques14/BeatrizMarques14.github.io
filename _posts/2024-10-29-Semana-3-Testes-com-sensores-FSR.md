---
title: "Semana 3 - Testes com sensores FSR"
date: "2024-12-09"
categories: [Materiais, Análise de Dados]
tags: [Materiais, Análise de Dados]

---

Os sensores FSR são dispositivos resistivos sensíveis à força que reduzem a sua resistência conforme a pressão aumenta. Quando não existe pressão, estes sensores apresentam uma resistência muito alta ( >10MΩ) e à medida que a força aumenta, a resistência diminui. Para ler os valores do sensor FSR, normalmente é necessário montar um divisor de tensão com uma resistência fixa pois este converte a mudança de resistência do FSR numa variação de tensão que pode ser medida por uma entrada analógica do microcontrolador. Para além disso, a escolha da resistência fixa no divisor de tensão pode melhorar a sensibilidade e a precisão do circuito.

Neste post irão ser aplicados vários valores de resistências no divisor de tensão de modo a obter um equilíbrio de sensibilidade e estabilidade nos dados recolhidos. Para tal irá utilizar-se um circuito semelhante ao apresentado na figura 1, a única diferença é que iremos utilizar o microcontrolador Teensy 4.0 e o pino de alimentação será o de 3.3V.

<figure style="text-align: center;">
    <img src="/assets/images/circuito.jpg" alt="Fig.1 Circuito utilizado para testar os sensores FSR" style="height: 200px;">
    <figcaption style="margin-bottom: 30px;">Circuito utilizado para testar os sensores FSR</figcaption>
</figure>

Para converter o valor lido pelo ADC em volts, utilizamos a seguinte fórmula:

<figure style="text-align: center;">
    <img src="/assets/images/testes_sensores/equation_white_bg.png" alt="">
</figure>

Onde fsrValue é o valor lido pelo ADC, 3.3 é o valor de Vin uma vez que o nosso sensor está conectado ao pino de 3.3V e 1023 corresponde à resolução de 10 bits do Teensy.

## Sensibilidade do Sensor com um leve toque

As figuras apresentadas abaixo mostram os valores obtidos para diferentes valores de resistências no circuito ao efetuar-se um leve toque com o dedo no sensor de forma a testar a sensibilidade do sensor.

<div style="text-align: center;">
  <table>
    <tr>
      <td>
        <figure>
          <img src="/assets/images/testes_sensores/1KOhm_leve_toque_max3.3.V.png" alt="1 KOhm" width="200">
          <figcaption>1 kΩ</figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/testes_sensores/10KOhm_leve_toque_max3.3V.png" alt="10 KOhm" width="200">
          <figcaption>10 kΩ</figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/testes_sensores/18KOhm_leve_toque_max3.3V.png" alt="18 KOhm" width="200">
          <figcaption>18 kΩ</figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/testes_sensores/20.2KOhm_leve_toque_max3.3V.png" alt="20.2 KOhm" width="200">
          <figcaption>20.2 kΩ</figcaption>
        </figure>
      </td>
    </tr>
    <tr>
      <td>
        <figure>
          <img src="/assets/images/testes_sensores/22.7KOhm_leve_toque_max3.3V.png" alt="22.7 KOhm" width="200">
          <figcaption>22.7 kΩ</figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/testes_sensores/28KOhm_leve_toque_max3.3V.png" alt="28 KOhm KOhm" width="200">
          <figcaption>28 kΩ</figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/testes_sensores/32.7KOhm_leve_toque_max3.3V.png" alt="32.7 KOhm" width="200">
          <figcaption>32.7 kΩ</figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/testes_sensores/39KOhm_leve_toque_max3.3V.png" alt="39 KOhm" width="200">
          <figcaption>39 kΩ</figcaption>
        </figure>
      </td>
    </tr>
  </table>
</div>

Através da análise das figuras é possível verificar que para resistências com menores valores, o sensor apresenta muito pouca sensibilidade (no caso de 1kΩ nem é possível verificar quando é que o sensor foi tocado pelo dedo). Para valores muito altos de resistências, verifica-se que a sensibilidade aumenta, mas também aumenta a instabilidade do sinal (no caso de 39 kΩ também não é possível verificar quando é que o sensor foi tocado devido à falta de estabilidade do sinal).

## Sensibilidade do Sensor com aplicação de uma força

Nas figuras apresentadas abaixo, apresentam-se os dados recolhidos ao aplicar a máxima força possível com um dedo.

<div style="text-align: center;">
  <table>
    <tr>
      <td>
        <figure>
          <img src="/assets/images/testes_sensores/teste_força/1KOhm_teste_força_max3.3.V.png" alt="1 kΩ" width="200">
          <figcaption>1 kΩ</figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/testes_sensores/teste_força/10KOhm_teste_força_max3.3V.png" alt="10 kΩ" width="200">
          <figcaption>10 kΩ</figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/testes_sensores/teste_força/18KOhm_teste_força_max3.3V.png" alt="18 kΩ" width="200">
          <figcaption>18 kΩ</figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/testes_sensores/teste_força/20.2KOhm_teste_força_max3.3V.png" alt="20.2 kΩ" width="200">
          <figcaption>20.2 kΩ</figcaption>
        </figure>
      </td>
    </tr>
    <tr>
      <td>
        <figure>
          <img src="/assets/images/testes_sensores/teste_força/22.7KOhm_teste_força_max3.3V.png" alt="22.7 kΩ" width="200">
          <figcaption>22.7 kΩ</figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/testes_sensores/teste_força/28KOhm_teste_força_max3.3V.png" alt="28 KOhm kΩ" width="200">
          <figcaption>28 kΩ</figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/testes_sensores/teste_força/32.7KOhm_teste_força_max3.3V.png" alt="32.7 kΩ" width="200">
          <figcaption>32.7 kΩ</figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/testes_sensores/teste_força/39KOhm_teste_força_max3.3V.png" alt="39 kΩ" width="200">
          <figcaption>39 kΩ</figcaption>
        </figure>
      </td>
    </tr>
  </table>
</div>

Tal como analisado anteriormente, um aumento da resistência resulta numa maior sensibilidade, mas também amplifica o ruído. Isso é evidente na figura correspondente à resistência de 39 kΩ, onde a voltagem atingiu o valor máximo de 3.3 V, mas apresentou instabilidade. Por outro lado, com a resistência de 1 kΩ, observou-se uma subida menos acentuada na voltagem quando aplicada força, indicando uma resposta mais lenta do sensor.

## Conclusões

Os dados recolhidos indicam que o aumento da resistência no divisor de tensão do circuito provoca o aumento de sensibilidade mas também o aumento da instabilidade do sinal. No entanto, as experiências realizadas não permitem obter conclusões definitivas, uma vez que a aplicação manual de força com o dedo dificulta a replicação exata da mesma força em testes diferentes.

No futuro, irão ser realizadas experiências utilizando massas pré-definidas, permitindo uma avaliação mais precisa do valor ideal de resistência a ser aplicado no circuito. Com base nos dados obtidos até agora, os valores de resistência que melhor equilibram estabilidade e sensibilidade são 20.2 kΩ e 32.7 kΩ.

## Referências

[^1]: SHAW, Kenneth; AGARWAL, Ananye; PATHAK, Deepak. Leap hand: Low-cost, efficient, and anthropomorphic hand for robot learning. arXiv preprint arXiv:2309.06440, 2023.