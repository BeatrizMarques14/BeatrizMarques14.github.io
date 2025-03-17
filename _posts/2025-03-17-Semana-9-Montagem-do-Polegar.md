---
title: "Semana 9 - Montagem do polegar"
date: "2025-03-17"
categories: [Programação,Montagem, Materiais]
tags: [Programação, ROS, Montagem]
math: true

---

Uma vez que, ao longo das últimas semanas, foram realizados vários testes utilizando um único dedo, o foco desta semana centrou-se na montagem de um segundo dedo, o polegar. Adicionalmente, foi necessário adaptar o código previamente desenvolvido para permitir o controlo simultâneo de múltiplos dedos, de forma a preparar a implementação para o funcionamento completo da mão robótica. 

## Montagem do polegar

As figuras seguintes ilustram as diferentes fases da construção do dedo, seguindo integralmente as instruções fornecidas pela LEAP Hand. O processo resultou na montagem bem-sucedida de um dedo funcional.

Relativamente à ponta do dedo, foi testada uma redução do *infill* com o objetivo de torná-la ligeiramente mais flexível e *compliant*. No entanto, durante a inserção dos *brass inserts*, verificou-se que a densidade de *infill* era insuficiente para garantir uma fixação adequada, comprometendo a aderência dos *inserts* à peça. Por isso, optou-se por continuar com o *infill* recomendado, assegurando a resistência necessária para a montagem adequada da ponta do dedo.

<div style="text-align: center;">
  <table style="margin: auto;">
    <tr>
      <td>
        <figure>
          <img src="/assets/images/semana9/1.jpg" alt="" width="200">
          <figcaption></figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana9/2.jpg" alt="" width="200">
          <figcaption></figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana9/3.jpg" alt="" width="200">
          <figcaption></figcaption>
        </figure>
      </td>
    </tr>
    <tr>
      <td>
        <figure>
          <img src="/assets/images/semana9/4.jpg" alt="" width="200">
          <figcaption></figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana9/5.jpg" alt="" width="200">
          <figcaption></figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana9/6.jpg" alt="" width="200">
          <figcaption></figcaption>
        </figure>
      </td>
    </tr>
    <tr>
      <td>
        <figure>
          <img src="/assets/images/semana9/7.jpg" alt="" width="200">
          <figcaption></figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana9/8.jpg" alt="" width="200">
          <figcaption></figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana9/9.jpg" alt="" width="200">
          <figcaption></figcaption>
        </figure>
      </td>
    </tr>
  </table>
</div>

Pra além das imagens apresentadas, também foi gravado um vídeo ao longo das diferentes fases de construção. No entanto, esse vídeo ainda está a ser editado.

## Código desenvolvido durante a semana

Durante as semanas anteriores, foi desenvolvido código para controlar um único dedo. No entanto, com a adição de mais dedos, tornou-se necessário reestruturar o código para permitir o controlo simultâneo de vários dedos.

Na nova estrutura, foi implementado um nó `/manager_node`, responsável pela comunicação com a porta serial, pela leitura dos dados dos motores e pela publicação dessas informações nos tópicos correspondentes a cada dedo: `/middle_data`, `/index_data`, `/thumb_data` e `/ring_data`. Além disso, este nó também subscreve o tópico `/set_positions`, que contém as posições alvo dos motores de cada dedo.

Para cada dedo, existe um nó manager dedicado, responsável por processar as informações lidas dos motores e tomar decisões específicas para o seu funcionamento. Essas decisões são então enviadas ao `/manager_node`, que as comunica aos motores.

Esta nova reestruturação melhora significativamente o funcionamento do sistema, uma vez que cada dedo é programado de forma independente. Dessa forma, caso ocorra uma falha em um dos dedos, os restantes continuam a operar corretamente, garantindo maior robustez ao controle da mão robótica.

A figura seguinte apresenta um esquema ilustrativo da nova estrutura, considerando apenas um dedo. Na próxima semana, será disponibilizado um esquema atualizado, abrangendo pelo menos dois dedos

<figure style="text-align: center;">
    <img src="/assets/images/semana9/rosgraph_1.png" alt="Organização do código reestruturado" style="height: 50px;">
    <figcaption style="margin-bottom: 30px;">Organização do código reestruturado</figcaption>
</figure>


### Melhorias no código implementado da semana passada

Na semana passada, observou-se que um aumento significativo da corrente aliado a uma redução da velocidade indicava a interseção de um obstáculo pelo dedo. No entanto, devido à variação temporal na leitura das velocidades ao longo do tempo, concluiu-se que a aceleração dos motores seria um parâmetro mais adequado para essa análise. Assim, estabeleceu-se que a deteção de um obstáculo ocorrerá quando houver um aumento significativo da corrente e uma redução expressiva da aceleração, tendendo a zero, garantindo uma identificação mais precisa do momento de contato com o obstáculo.