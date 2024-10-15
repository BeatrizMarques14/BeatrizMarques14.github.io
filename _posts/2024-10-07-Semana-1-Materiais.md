---
title: "Semana 1 - Materiais"
date: "2024-10-07"
categories: [Materiais]
tags: [Materiais]

---

Este é o primeiro post de uma série semanal na qual irei relatar o progresso da minha dissertação de mestrado, cujo tema é o desenvolvimento de uma mão robótica antropomórfica que irá baseada no projeto LEAP Hand[^1]. Este projeto tem como objetivo a construção de uma prótese robótica funcional, com foco na precisão dos movimentos e na resposta sensorial eficiente para posteriormente ser utilizada em projetos para a utilização de algoritmos de *reinforcement learning* na tarefa de "*grasping*" de objetos.

![Imagem com a LEAP Hand](/assets/images/leap_hand.png)


Nesta fase inicial, o trabalho está concentrado na elaboração de uma lista de materiais e na pesquisa de sensores apropriados para a integração ao sistema uma vez que estes sensores irão ser cruciais no desenvolvimento do "*tato*" desta mão.

## Materiais necessários para a montagem dos motores 

| Componente | Quantidade |
|:-------------|:-------------:|
| [Dynamixel XC330-M288-T](https://www.robotis.us/dynamixel-xc330-m288-t/) | 16 |
| [Dynamixel FPX330-H101](https://www.robotis.us/fpx330-h101-4pcs-set/) | 11 |
| [Dynamixel FPX330-S101](https://www.robotis.us/fpx330-s101-4pcs-set/) | 12 |
| [Dynamixel FPX330-S102](https://www.robotis.us/fpx330-s102-4pcs-set/) | 6 |
| [Dynamixel U2D2](https://www.robotis.us/u2d2/) | 1 |
| [XT30 Pigtail](https://www.amazon.com/dp/B09LYHHS9J?_encoding=UTF8&psc=1&ref_=cm_sw_r_cp_ud_dp_9XKNAYTYBS6N4M4F43AR) | 1 |
| [JST EH ASEHSEH22K305](https://www.digikey.pt/pt/products/detail/jst-sales-america-inc/ASEHSEH22K305/6009480) | 15 |
| [JST EHR 3](https://www.digikey.com/en/products/detail/jst-sales-america-inc/EHR-3/527225) | 5 |

#### Dynamixel XC330-M288-T

Os motores DYNAMIXEL XC330-M288-T são motores que nos permitem realizar movimentos suaves e controlados tendo sempre feedback da sua posição, o que os torna muito adequados esta tarefa.

<figure style="text-align: center;">
    <img src="/assets/images/motor.jpg" alt="Descrição da Imagem" style="height: 200px;">
    <figcaption>Dynamixel XC330-M288-T.</figcaption>
</figure>

#### Dynamixel FPX330-H101, Dynamixel FPX330-S101 e Dynamixel FPX330-S102

Estas peças são as molduras dos motores que os protegem no caso de algum acidente. Na lista de materiais apresentada, consideram-se 2 peças extra do DYNAMIXEL FPX330-S101 uma vez que em caso de acidente, esta peça poderá partir de forma a proteger o resto da mão.

<div style="display: flex; justify-content: space-around;text-align: center;">
    <figure style="margin: 0;">
        <img src="/assets/images/h101.jpg" alt="Dynamixel FPX330-H101" style="height: 200px; object-fit: cover;">
        <figcaption style="margin-bottom: 20px;">Dynamixel FPX330-H101</figcaption>
    </figure>

    <figure style="margin: 0;">
        <img src="/assets/images/s101.jpg" alt="Dynamixel FPX330-S101" style="height: 200px; object-fit: cover;">
        <figcaption style="margin-bottom: 20px;">Dynamixel FPX330-S101</figcaption>
    </figure>

     <figure style="margin: 0;">
        <img src="/assets/images/s102.jpg" alt="Dynamixel FPX330-S102" style="height: 200px; object-fit: cover;">
        <figcaption style="margin-bottom: 20px;">Dynamixel FPX330-S102</figcaption>
    </figure>
</div>


#### Dynamixel U2D2

O U2D2 é um dispositivo de interface USB para comunicação com os motores DYNAMIXEL. Ele atua como um conversor USB para TTL, permitindo que o computador ou outro dispositivo controlador se conecte diretamente aos motores DYNAMIXEL para enviar comandos e receber feedback.

<figure style="text-align: center;">
    <img src="/assets/images/u2d2.jpg" alt="Descrição da Imagem" style="height: 200px;">
    <figcaption>Dynamixel U2D2.</figcaption>
</figure>


#### XT30 Pigtail, JST EH ASEHSEH22K305 e JST EHR 3

Estas peças combinadas serão utilizadas para conectar a fonte de alimentação ao U2D2 e a cada um dos dedos da mão robótica. Utilizam-se estas peças em substituição do U2D2 Power Hub de forma a ter uma montagem mais simples e organizada.

<div style="display: flex; justify-content: space-around;text-align: center;">
    <figure style="margin: 0;">
        <img src="/assets/images/xt30.jpg" alt="XT30 Pigtail" style="height: 200px; object-fit: cover;">
        <figcaption style="margin-bottom: 20px;">XT30 Pigtail</figcaption>
    </figure>

    <figure style="margin: 0;">
        <img src="/assets/images/jst_cable.jpg" alt="JST EH ASEHSEH22K305" style="height: 200px; object-fit: cover;">
        <figcaption style="margin-bottom: 20px;">JST EH ASEHSEH22K305</figcaption>
    </figure>

     <figure style="margin: 0;">
        <img src="/assets/images/jst_ehr3.jpg" alt="JST EHR 3" style="height: 200px; object-fit: cover;">
        <figcaption style="margin-bottom: 20px;">JST EHR 3</figcaption>
    </figure>
</div>


## Incorporação de sensores

Após realizar uma pesquisa sobre sensores utilizados em mãos robóticas antropomórficas, observou-se a existência de diversas soluções comerciais (como o *BioTac fingertip*) e também foram desenvolvidas várias abordagens por diferentes autores, que utilizam técnicas variadas, como, por exemplo, sensores baseados no efeito Hall[^2]. No entanto, estas soluções tornam-se dispendiosas e muito complexas de integrar noutros projetos. 

<div style="display: flex; justify-content: space-around;text-align: center;">
    <figure style="margin: 0;">
        <img src="/assets/images/biotac.jpeg" alt="Biotac fingertip" style="height: 200px; object-fit: cover;">
        <figcaption style="margin-bottom: 20px;"><i>Biotac fingertip</i> desenvolvida por <i>Syntouch</i>.</figcaption>
    </figure>

    <figure style="margin: 0;">
        <img src="/assets/images/hall_sensor.png" alt="Sensores desenvolvidos com base no efeito de Hall" style="height: 200px; object-fit: cover;">
        <figcaption style="margin-bottom: 20px;">Sensores desenvolvidos com base no efeito de Hall [2].</figcaption>
    </figure>
</div>



Uma alternativa de baixo custo e relativamente fácil de incorporar é o sensor FSR (*Force Sensing Resistor*) que permite a deteção de força ou pressão aplicada numa superfície. O princípio de funcionamento destes sensores é baseado na variação da resistência elétrica de materiais poliméricos quando são deformados ou comprimidos e uma das suas grandes vantagens é o facto de serem relativamente maleáveis, permitindo uma aplicação simples.

<figure style="text-align: center;">
    <img src="/assets/images/fsr.jpg" alt="Descrição da Imagem" style="height: 300px;">
    <figcaption>Diferentes sensores FSR.</figcaption>
</figure>

Após analisar as pontas dos dedos da mão robótica, conclui-se que os sensores FSR redondos são a melhor opção, já que estes se adaptam melhor à geometria irregular das pontas dos dedos. Os sensores quadrados, por sua vez, apresentam dimensões maiores, o que pode dificultar o encaixe e o desempenho adequado.

 Entre os modelos redondos disponíveis, destacam-se o FSR 400, com diâmetro de 7.62 mm, e o FSR 402, com 18.28 mm. Ambos são muito semelhantes, sendo a única diferença o tamanho. Em relação aos sensores quadrados, apenas existe um modelo, o FSR 406, que possui dimensões 43.69x43.69 mm. Todos estes sensores possuem uma sensibildade de força entre 0.2 N e 20 N, o que corresponde a uma massa entre 20 g e 2 Kg.



Nesta fase inicial, considera-se que a melhor abordagem para a incorporação dos sensores será aplicar sensores FSR 400 em cada ponta dos dedos da mão robótica e um sensor FSR 406 na palma da mão.

#### Lista de sensores

| Componente | Quantidade |
|:-------------|:-------------:|
| Sensor FSR 400 | 4 |
| Sensor FSR 406 | 1 |


## Outros acessórios

Nesta secção apresenta-se uma lista de alguns acessórios não listados anteriormente mas que são cruciais para o desenvolvimento das restantes peças, nomeadamente o material para produzir as peças impressas em 3D.

| Componente | Quantidade |
|:-------------|:-------------:|
| [M3 6mm 3D printer inserts](https://www.amazon.com/dp/B08Z86Z85R?ref_=cm_sw_r_cp_ud_dp_D2B4NNVZK6B5H3RCRM6X) | 5 |
| [M2 6mm 3D printer Inserts](https://www.amazon.com/dp/B08Z86Z85R?ref_=cm_sw_r_cp_ud_dp_D2B4NNVZK6B5H3RCRM6X) | 24 |
| [M2 8mm Screws](https://www.amazon.com/dp/B07W5J19Y5?ref_=cm_sw_r_cp_ud_dp_YSED9J2Q9JHAZJE2TE6S&peakEvent=4&dealEvent=1) | 16 |
| [M2 10mm Screws](https://www.amazon.com/dp/B07W5J19Y5?ref_=cm_sw_r_cp_ud_dp_YSED9J2Q9JHAZJE2TE6S&peakEvent=4&dealEvent=1) | 24 |
| [M2 6mm Screws](https://www.amazon.com/dp/B07W5J19Y5?ref_=cm_sw_r_cp_ud_dp_YSED9J2Q9JHAZJE2TE6S&peakEvent=4&dealEvent=1) | 4 |
| [M3 6mm Screws](https://www.amazon.com/dp/B07S1R8NL7?_encoding=UTF8&psc=1&ref_=cm_sw_r_cp_ud_dp_C66CFKPG8A5B9GW1KH9T) | 5 |
| [Polymaker PLA Pro](https://mauser.pt/catalog/product_info.php?products_id=096-8892) | ~200g |
| [Filamento TPU](https://mauser.pt/catalog/product_info.php?products_id=096-8175) | 100g |
| [Power Supply ( 5V 30A)](https://www.electrofun.pt/transformadores-e-conversores/fonte-de-alimentacao-industrial-5v-150w-30a-prok) | 1|
| Power Cable AC | 1 |
| [14 AWG 5V Cable](https://www.amazon.com/dp/B088RDY284?ref_=cm_sw_r_cp_ud_dp_4ESS07CK5QSXK185XQEP) | 1 |








## Referências

[^1]: SHAW, Kenneth; AGARWAL, Ananye; PATHAK, Deepak. Leap hand: Low-cost, efficient, and anthropomorphic hand for robot learning. arXiv preprint arXiv:2309.06440, 2023.
[^2]: JAMONE, Lorenzo, et al. Highly sensitive soft tactile sensors for an anthropomorphic robotic hand. IEEE sensors Journal, 2015, 15.8: 4226-4233