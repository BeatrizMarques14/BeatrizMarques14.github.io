---
title: "Semana 4 - Primeiros Testes com motores"
date: "2025-01-24"
categories: [Materiais, Análise de Dados]
tags: [Materiais, Análise de Dados]

---

Os motores Dynamixel XC330-M288-T são atuadores inteligentes reconhecidos pela sua
elevada precisão, eficiência energética e design compacto. Estes motores oferecem controlo preciso de posição,
velocidade e corrente, características essenciais para sistemas que requerem movimentos suaves e altamente controlados.

Uma das grandes vantagens dos motores Dynamixel é o facto de serem compatíveis com diversas plataformas tais como ROS, Python e C++. No entanto, antes de iniciar qualquer tipo de controlo destes equipamentos, é necessáriorealizar as configurações necessárias através da aplicação oficial da Dynamixel, Dynamixel Wizard 2.0.

## Interface Dynamixel Wizard 2.0

Após conectar um dos motores ao U2D2 corretamente, juntamente com a fonte de alimentação adequada, foi iniciado o programa Dynamixel Wizard 2.0.
Para localizar o motor, utilizou-se a opção *Scan DYNAMIXEL*, que na primeira utilização do programa apresenta uma janela de configuração com diversas opções de forma a optimizar o processo de localização:

1. Protocolo de Comunicação: O utilizador deve selecionar o protocolo compatível com o motor. No caso do Dynamixel XC330-M288-T, utiliza-se o Protocol 2.0.
2. Porta de Comunicação: Deve especificar-se a porta em que o U2D2 está conectado. Por exemplo, na imagem abaixo, a porta selecionada é COM3 (USB Serial Port). 
3. *Baud Rate*: O *baud rate* deve ser configurado para corresponder ao valor utilizado pelo motor. Neste caso sabemos que o Dynamixel XC330-M288-T vem de fábrica com um *baud rate* de 57600 bps.
4. Intervalo de IDs: O programa também permite especificar o intervalo de IDs dos motores a serem procurados. Por padrão, ele procura todos os IDs entre 0 e 252.


<figure style="text-align: center;">
    <img src="/assets/images/motores_1/scan.png" alt="Janela de configurações para realizar a localização do motor" style="height: 300px;">
    <figcaption style="margin-bottom: 30px;">Janela de configurações para realizar a localização do motor</figcaption>
</figure>

Após reconhecer o motor, já é possível verificar as características do mesmo, alterar configurações e até colocar o motor em movimento.

<figure style="text-align: center;">
    <img src="/assets/images/motores_1/tela_1.png" alt="Interface do Dynamixel Wizard 2.0" style="height: 300px;">
    <figcaption style="margin-bottom: 30px;">Interface do Dynamixel Wizard 2.0</figcaption>
</figure>

## Colocar o motor em movimento

Os motores Dynamixel vêm configurados de fábrica na posição de 180º, também conhecida como home position. Para ajustar a posição do motor, basta ativar a opção Torque e definir a nova goal position, como ilustrado na figura abaixo.

<figure style="text-align: center;">
    <img src="/assets/images/motores_1/alterar_posição.png" alt="Interface para alterar a posição do motor" style="height: 300px;">
    <figcaption style="margin-bottom: 30px;">Interface para alterar a posição do motor</figcaption>
</figure>

Para controlar o motor no Dynamixel Wizard 2.0, existem 3 alternativas possíveis: controlar com o dedo ao arrastar a seta vermelha da figura do motor, enviar comandos de +/- 90º ou enviar a posição correta desejada.

Os motores Dynamixel operam com uma resolução de 12 bits, ou seja, 4096 passos por rotação completa. Logo, cada passo equivale a *360 / 4096* que equivale a aproximadamente 0.09º. Para descobrir o valor que se tem de colocar para atingir a posição desejada, basta multiplicar o valor que queremos em º por 4096 e dividir por 360. Por exemplo, para ter a posição 180º, tem de se enviar o valor 2048. 

Para além de controlar o motor, o Dynamixel Wizard 2.0 também permite realizar gráficos de modo a verificar o desempenho o motor. Na figura abaixo, mostra-se um gráfico com a *goal position*, a *present position* e também a velocidade do motor após enviar comandos de +90º, -90º, -90º e +90º respetivamente.

<figure style="text-align: center;">
    <img src="/assets/images/motores_1/grafico_com velocidade.png" alt="Gráfico com a *goal position*, a *present position* e também a velocidade do motor" style="height: 300px;">
    <figcaption style="margin-bottom: 30px;">Gráfico com a *goal position*, a *present position* e também a velocidade do motor</figcaption>
</figure>

O gráfico demonstra um sistema de controlo eficiente, no qual a posição atual do motor (linha vermelha) segue com precisão a posição objetivo (linha azul) em diferentes momentos. As variações na velocidade (linha verde) indicam ajustes rápidos e controlados durante as mudanças de objetivo, evidenciando uma resposta dinâmica adequada. A estabilidade alcançada após cada transição reflete um bom ajuste no controlo, que evidencia precisão e eficiência no movimento do motor.

## Motor em movimento

Nesta secção deixa-se um pequeno vídeo onde se controlou o motor através do Dynamixel Wizard 2.0 com as 3 alternativas descritas anteriormente. No futuro, o controle dos motores será realizado com o auxílio do ROS, utilizando o Dynamixel SDK, em vez do Dynamixel Wizard 2.0. No entanto, o Dynamixel Wizard 2.0 desempenha um papel fundamental na atribuição dos IDs corretos a cada motor, garantindo o controlo correto e preciso da mão que será construída.





<div style="text-align: center;">
  <a href="https://www.youtube.com/shorts/JcM6Dt9DKEE">
    <img src="https://img.youtube.com/vi/JcM6Dt9DKEE/0.jpg" alt="Texto alternativo">
  </a>
</div>

[Link do vídeo](https://www.youtube.com/shorts/JcM6Dt9DKEE)

