---
title: "Semana 2 - Unidade de Controlo"
date: "2024-10-14"
categories: [Materiais]
tags: [Materiais]

---

Durante esta semana do projeto, o foco é selecionar a unidade de controlo mais adequada para integrar os sensores de contacto apresentados no post anterior. 

Após definir os locais de posicionamento dos sensores (nas pontas dos dedos e na palma da mão), o próximo passo é identificar qual microcontrolador poderá garantir uma leitura precisa dos sinais adquiridos e também adequar-se ao espaço limitado no interior da mão robótica. 

<figure style="text-align: center;">
    <img src="/assets/images/palma.png" alt="Peça da palma da mão onde será introduzida a unidade de controlo" style="height: 200px;">
    <figcaption style="margin-bottom: 30px;">Peça da palma da mão com o espaço reservado para o microcontrolador destacado a vermelho.</figcaption>
</figure>

É de realçar que os locais de posicionamento dos sensores ainda poderão ser ajustados, assim como o número total de sensores utilizados. Por isso, é essencial considerar o número de entradas e saídas analógicas disponíveis no microcontrolador escolhido, ou a possível necessidade de incorporar uma PCB (Placa de Circuito Impresso) ao circuito.


## Dimensões Unidade de Controlo

Com o auxílio da ferramenta FreeCad, foi possível realizar medições na peça da palma da mão de forma a apurar as dimensões máximas possíveis para o microcontrolador a ser implementado.

Tal como é visível na figura imediatamente abaixo, as dimensões máximas permitidas para a unidade de controlo são de 23.40 x 52.63 mm.

<figure style="text-align: center;">
    <img src="/assets/images/espaço_controlador.png" alt="Peça da palma da mão onde será introduzida a unidade de controlo" style="height: 300px;">
    <figcaption >Medições efetuadas com a ferramenta FreeCad.</figcaption>
</figure>


## Seleção do Microcontrolador

Após efetuar uma pesquisa sobre unidades de controlo, apresenta-se uma tabela com vários modelos encontrados dentro das dimensões apresentadas:

| Componente | Número de Pinos Analógicos | Frequência de Operação | Dimensões | Tensão de Alimentação | Preço |
|:-------------|:-------------:|:-------------:|:-------------:| :-------------:|
| [Teensy 4.0](https://mauser.pt/catalog/product_info.php?products_id=096-9107) | 14 | 600 MHz | 19 x 36 mm | 5 V | 30.80 € |
| [Arduino Nano](https://mauser.pt/catalog/product_info.php?products_id=096-2425) | 8 | 16 MHz |  19 x 43 mm | 5 V | 22.90 € |
| [Seeeduino XIAO](https://mauser.pt/catalog/product_info.php?products_id=096-8528) | 11 | 48 MHz | 18 x 20 mm | 5 V | 7.50 € |


### Teensy 4.0 

O Teensy 4.0 é um microcontrolador equipado com um processador ARM Cortex-M7 que opera a 600 MHz, proporcionando alto desempenho e eficiência, o que permite a leitura rápida e precisa das variações de resistência dos sensores. Com 14 pinos analógicos e uma resolução de 12 bits, o Teensy pode obter dados detalhados sobre as forças aplicadas nos sensores.

Uma das grandes vantagens do Teensy 4.0 é a sua flexibilidade em termos de conectividade e compatibilidade com diversas bibliotecas, o que inclui a capacidade de operar em ambientes de programação como a IDE do Arduino, PlatformIO e até mesmo integração com sistemas como o ROS (Robot Operating System) através da utilização de bibliotecas como o ROSSerial.

<figure style="text-align: center;">
    <img src="/assets/images/teensy4.0.jpg" alt="Teensy 4.0" style="height: 200px;">
    <figcaption >Teensy 4.0.</figcaption>
</figure>

### Arduino Nano

O Arduino Nano é um microcontrolador compacto e acessível, baseado no ATmega328 que opera a 16 MHz. Este microcontrolador é bastante utilizado em projetos de robótica que exigem um design compacto, uma vez que possui 8 entradas analógicas e 14 pinos digitais, permitindo controlar diversos sensores e/ou motores.

Embora seja um microcontrolador com menos recursos comparado com o Teensy, o Arduino Nano também pode ser integrado no ROS através da biblioteca ROSSerial. Para além disso, existe uma vasta quantidade de documentação e suporte na comunidade sobre o uso do Arduino acoplado ao ROS, o que facilita a implementação e resolução de problemas.

<figure style="text-align: center;">
    <img src="/assets/images/arduino_nano.jpg" alt="Arduino Nano" style="height: 200px;">
    <figcaption >Arduino Nano.</figcaption>
</figure>


### Seeeduino XIAO

O Seeeduino XIAO é um dos menores microcontroladores disponíveis no mercado, equipado com um processador ARM Cortex-M0+ que opera a 48 MHz. Este microcontrolador possui 14 PINs GPIO (General Purpose Input/Output), dos quais 11 podem ser utilizados para PINs analógicos.

Tal como ambos os microcontroladores apresentado anteriormente, também o Seeeduino XIAO pode ser integrado ao ROS através da biblioteca ROSSerial. No entanto, de todas as opções é o microcontrolador com documentação e suporte mais pobre sobre a sua utilização acoplada ao ROS.

<figure style="text-align: center;">
    <img src="/assets/images/seeeduino.jpg" alt="Seeeduino XIAO" style="height: 200px;">
    <figcaption >Seeeduino XIAO.</figcaption>
</figure>

## Conclusão

Após analisar todas as opções de microcontroladores apresentadas acima e tendo em consideração tanto as suas características como a documentação e suporte disponíveis, considera-se que a melhor escolha será o microcontrolador Teensy 4.0.

Esta decisão baseia-se no facto do Teensy 4.0 oferecer um número adequado de pinos analógicos, com sobra para possíveis expansões no número de sensores a serem incorporados. Além disso, a sua alta frequência de operação (a mais elevada de todas as opções) e a facilidade de integração com o ROS, tornam este microcontrolador uma ótima escolha para este projeto, uma vez que a documentação da LEAP[^1] Hand se encontra bastante relacionada com a utilização do ROS.


## Referências

[^1]: SHAW, Kenneth; AGARWAL, Ananye; PATHAK, Deepak. Leap hand: Low-cost, efficient, and anthropomorphic hand for robot learning. arXiv preprint arXiv:2309.06440, 2023.