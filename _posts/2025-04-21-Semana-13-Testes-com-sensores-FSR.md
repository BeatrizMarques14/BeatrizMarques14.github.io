---
title: "Semana 13 - Testes com sensores FSR"
date: "2025-04-21"
categories: [Programação,Análise de Dados]
tags: [Programação, ROS, Análise de Dados]
math: true

---

Até ao momento, toda a mão já foi construída e programada, sendo possível detetar a realização da tarefa de grasping através da leitura das correntes consumidas pelos motores e do cálculo da sua aceleração. No entanto, este método ainda não permite uma deteção precisa do contacto com os objetos, pelo que serão adicionados sensores de contacto FSR (Force Sensitive Resistor) para apoiar esta tarefa.

Neste post, serão apresentados os primeiros testes realizados com os sensores, com o objetivo de compreender o seu funcionamento, avaliar a necessidade de aplicar filtros para melhorar o sinal e garantir uma leitura mais fiável. Para além disso, será também mostrado um movimento completo da mão com os respetivos atrasos temporais entre os dedos, tal como referido na semana passada.

## Testes com os sensores

Para a realização dos testes com os sensores, foi feita uma montagem muito semelhante à apresentada no post da semana 3, desta vez com quatro sensores a operar em simultâneo: três sensores FSR 400 e um sensor FSR 406.

### Aplicação de força em apenas um dos sensores

Na primeira experiência realizada, foi aplicada força apenas sobre um dos sensores, com o objetivo de observar a sua resposta individual e avaliar se os restantes sensores eram de alguma forma influenciados ou afetados pela força aplicada no sensor em teste.

<div style="text-align: center;">
  <table>
    <tr>
      <td>
        <figure>
          <img src="/assets/images/semana13/sensor1.png" alt="1 KOhm" width="200">
          <figcaption>Aplicação de força no 1º sensor</figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana13/sensor2.png" alt="10 KOhm" width="200">
          <figcaption>Aplicação de força no 2º sensor</figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana13/sensor3.png" alt="10 KOhm" width="200">
          <figcaption>Aplicação de força no 3º sensor</figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana13/sensor4.png" alt="10 KOhm" width="200">
          <figcaption>Aplicação de força no 4º sensor</figcaption>
        </figure>
      </td>
    </tr>
  </table>
</div>

Através das figuras apresentadas, é possível observar que os sensores respondem de forma adequada à aplicação de carga, enquanto os restantes sensores permanecem estáveis durante todo o processo. Para além disso, verifica-se uma ligeira variação de aproximadamente 0.01 V na ausência de força aplicada. No entanto, dado que esta oscilação é bastante residual, não se justifica, para já, a aplicação de qualquer tipo de filtragem no sinal.


### Aplicação de força em vários sensores em simultâneo


Na segunda experiência realizada, foram aplicadas forças em vários sensores em simultâneo, com o objetivo de estudar o comportamento conjunto dos mesmos. Os gráficos correspondentes a esta experiência apresentam-se de seguida.

<div style="text-align: center;">
  <table>
    <tr>
      <td>
        <figure>
          <img src="/assets/images/semana13/2sensora.png" alt="1 KOhm" width="200">
          <figcaption>Aplicação de força em 2 sensores</figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana13/3sensors.png" alt="10 KOhm" width="200">
          <figcaption>Aplicação de força em 3 sensores</figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana13/4sensors.png" alt="10 KOhm" width="200">
          <figcaption>Aplicação de força em 4 sensores</figcaption>
        </figure>
      </td>
    </tr>
  </table>
</div>

Através das figuras apresentadas, é possível confirmar as conclusões retiradas da primeira experiência, uma vez que se verifica que os sensores se comportam de forma independente, sem interferência mútua. Verifica-se ainda que os sensores apresentam uma resposta bastante satisfatória quando submetidos à aplicação de carga.

### Comportamento do sensor FSR 406

A terceira experiência teve como objetivo avaliar o comportamento do sensor FSR 406, uma vez que possui uma área de deteção maior do que os sensores FSR 400. Nesta experiência, foi inicialmente aplicada uma carga colocando um objeto sobre o sensor e, posteriormente, aplicou-se uma segunda carga num ponto distinto do mesmo sensor, de forma a observar a sua resposta a múltiplos pontos de pressão.



<div style="text-align: center;">
    <img src="/assets/images/semana13/sensor_406.png" alt="Texto alternativo">
    <figcaption>Teste realizado com o sensor FSR 406</figcaption>
</div>


Através do gráfico obtido, é possível observar que, aquando da aplicação da primeira carga sobre o sensor, existe alguma oscilação no sinal. Este comportamento poderá estar relacionado com o facto de o objeto utilizado não ser muito pesado e de o sensor não se encontrar devidamente fixado à superfície. Posteriormente, com a aplicação da segunda carga noutro ponto do sensor, verifica-se que o sinal resultante corresponde à soma das duas cargas aplicadas, o que demonstra a capacidade do sensor FSR 406 para detetar pressões em múltiplas zonas da sua superfície.

### Aplicação de luva na ponta do dedo

Na fase de incorporação dos sensores na mão robótica, uma das principais preocupações prende-se com a fixação dos sensores nas pontas dos dedos, bem como com a sua proteção, de forma a evitar o contacto direto com os objetos manipulados. Como solução, considerou-se a utilização de um dedo de uma luva de látex envolta na extremidade de cada dedo, posicionando o sensor entre a ponta do dedo e a luva. Desta forma, a luva não só contribui para a fixação do sensor, como também atua como uma camada protetora, reduzindo o risco de danos causados pelo atrito ou impacto com os objetos.

Adicionalmente, ao contrário das experiências anteriores, nas quais a amostragem dos valores era realizada a uma frequência de 20 Hz, nesta experiência a frequência de amostragem foi aumentada para cerca de 1000 Hz, permitindo uma análise mais detalhada do comportamento dinâmico do sensor.

<div style="text-align: center;">
    <img src="/assets/images/semana13/glove.jpg" alt="Texto alternativo" width="200">
    <figcaption>Sensor colocado na ponta do dedo com uma luva</figcaption>
</div>

<div style="text-align: center;">
    <img src="/assets/images/semana13/glove.png" alt="Texto alternativo" width="200">
    <figcaption>Experiencia realizada com o sensor na ponta do dedo</figcaption>
</div>


Através do gráfico obtido, é possível verificar uma maior variação no sinal, o que poderá estar relacionado com o aumento da frequência de amostragem e com as vibrações dos motores do dedo. No entanto, estas oscilações mantêm-se na ordem dos 0.01 V, sendo por isso bastante residuais. Ainda assim, observa-se que o sensor continua a apresentar uma boa capacidade de resposta à aplicação de carga. No entanto, o sinal revela-se menos estável em comparação com os testes anteriores, pelo que poderá ser necessário considerar a aplicação de algum tipo de filtragem com o objetivo de suavizar o sinal e garantir uma leitura mais fiável.

## Movimento da mão com *offsets* temporais entre dedos

Para exemplificar o movimento da mão com *offsets* temporais entre os dedos, foi desenvolvido o comando *wave*. Neste movimento, a mão fecha de forma sequencial, iniciando pelo polegar, seguido pelos restantes dedos. O atraso temporal entre o polegar e o dedo seguinte foi definido como 0.5 segundos, enquanto os offsets entre os restantes dedos foram definidos como 0.2 segundos.

Na fase de abertura da mão, optou-se por inverter a ordem dos *offsets* de forma a manter uma simetria no movimento, permitindo que os dedos que fecharam por último sejam os primeiros a abrir. Para isso, os offsets de abertura foram calculados com a seguinte expressão:

```python
offsets_abertura = [max_offset - (o - min_offset) for o in offsets_fecho]
```

<div style="text-align: center;">
  <a href="https://www.youtube.com/watch?v=BEH4XPx-fHk">
    <img src="https://img.youtube.com/vi/BEH4XPx-fHk/0.jpg" alt="Texto alternativo">
  </a>
</div>

[Link do vídeo](https://www.youtube.com/watch?v=BEH4XPx-fHk)
