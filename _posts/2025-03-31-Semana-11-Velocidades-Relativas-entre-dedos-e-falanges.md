---
title: "Semana 11 - Velocidades Relativas entre os dedos e falanges"
date: "2025-03-31"
categories: [Programação]
tags: [Programação, ROS, Grasping]
math: true

---

O ser humano possui uma notável capacidade de executar uma ampla variedade de movimentos com a mão, aplicando diferentes velocidades tanto entre os dedos como entre as falanges de um mesmo dedo. Esta habilidade permite a realização de gestos que, embora pareçam semelhantes, são significativamente distintos e adaptáveis a diferentes situações. Um exemplo disso é o ato de fechar a mão, que pode ocorrer de maneiras diversas, como iniciando o movimento pelo polegar ou, alternativamente, pelos restantes dedos.

Nesta semana, o foco do trabalho esteve no desenvolvimento de um mecanismo capaz de aplicar fatores de velocidade relativa, tanto entre os diferentes dedos quanto entre as falanges de cada dedo. Esta abordagem permite a execução de distintos padrões de movimento, como o exemplo mencionado anteriormente, onde a mão pode ser fechada de diferentes formas, dependendo da sequência e da velocidade aplicada a cada segmento. Para além disso, está em desenvolvimento um mecanismo para detetar colisões entre os dedos com base na cinemática de cada um. Etse sistema permitirá identificar interseções entre os movimentos dos dedos.

## Velocidades relativas entre dedos e falanges

Neste post, serão apresentadas duas experiências para analisar o impacto da velocidade relativa tanto entre os dedos quanto entre as falanges de um mesmo dedo.

Na primeira experiência, será aplicada ao *middle finger* uma velocidade duas vezes superior à do polegar, permitindo avaliar o efeito da diferença de velocidade entre os dedos.

Na segunda experiência, o foco será exclusivamente a variação da velocidade entre as falanges de um único dedo. Para isso, a primeira falange terá o dobro da velocidade das restantes, possibilitando observar como essa diferença influencia o movimento global do dedo.

### Velocidade relativa entre dedos

Nas figuras seguintes, são apresentados os gráficos de posição, velocidade e corrente para ambos os dedos na condição em que o *middle finger* opera com o dobro da velocidade do polegar.

Além disso, também são disponibilizados os links para os vídeos no YouTube que documentam esta experiência, permitindo uma visualização mais detalhada do comportamento da mão robótica em tempo real.

#### Gráficos

Posições do *middle finger*

<div style="text-align: center;">
  <table>
    <tr>
      <td>
        <figure>
          <img src="/assets/images/semana11/finger_pos_finger_1.png" alt="1 KOhm" width="200">
          <figcaption>Posições dos motores do *middle finger* com velocidade igual do polegar </figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana11/finger_pos2_finger_1.png" alt="10 KOhm" width="200">
          <figcaption>Posições dos motores do *middle finger* com o dobro da velocidade do polegar </figcaption>
        </figure>
      </td>
    </tr>
  </table>
</div>

Posições do polegar

<div style="text-align: center;">
  <table>
    <tr>
      <td>
        <figure>
          <img src="/assets/images/semana11/finger_pos_finger_3.png" alt="1 KOhm" width="200">
          <figcaption>Posições dos motores do polegar com velocidade igual do *middle finger* </figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana11/finger_pos2_finger_3.png" alt="10 KOhm" width="200">
          <figcaption>Posições dos motores do polegar com metade da velocidade do *middle finger </figcaption>
        </figure>
      </td>
    </tr>
  </table>
</div>

Velocidades do *middle finger*

<div style="text-align: center;">
  <table>
    <tr>
      <td>
        <figure>
          <img src="/assets/images/semana11/finger_vel_finger_1.png" alt="1 KOhm" width="200">
          <figcaption>Velocidades dos motores do *middle finger* com velocidade igual do polegar </figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana11/finger_vel2_finger_1.png" alt="10 KOhm" width="200">
          <figcaption>Velocidades dos motores do *middle finger* com o dobro da velocidade do polegar </figcaption>
        </figure>
      </td>
    </tr>
  </table>
</div>

Velocidades do polegar

<div style="text-align: center;">
  <table>
    <tr>
      <td>
        <figure>
          <img src="/assets/images/semana11/finger_vel_finger_3.png" alt="1 KOhm" width="200">
          <figcaption>Velocidades dos motores do polegar com velocidade igual do *middle finger* </figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana11/finger_vel2_finger_3.png" alt="10 KOhm" width="200">
          <figcaption>Velocidades dos motores do polegar com metade da velocidade do *middle finger </figcaption>
        </figure>
      </td>
    </tr>
  </table>
</div>

Correntes do *middle finger*

<div style="text-align: center;">
  <table>
    <tr>
      <td>
        <figure>
          <img src="/assets/images/semana11/finger_curr_finger_1.png" alt="1 KOhm" width="200">
          <figcaption>Corrente consumidas pelos motores do *middle finger* com velocidade igual do polegar </figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana11/finger_curr2_finger_1.png" alt="10 KOhm" width="200">
          <figcaption>Correntes consumidas pelos motores do *middle finger* com o dobro da velocidade do polegar </figcaption>
        </figure>
      </td>
    </tr>
  </table>
</div>

Correntes do polegar

<div style="text-align: center;">
  <table>
    <tr>
      <td>
        <figure>
          <img src="/assets/images/semana11/finger_curr_finger_3.png" alt="1 KOhm" width="200">
          <figcaption>Correntes consumidas pelos motores do polegar com velocidade igual do *middle finger* </figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana11/finger_curr2_finger_3.png" alt="10 KOhm" width="200">
          <figcaption>Correntes consumidas pelos motores do polegar com metade da velocidade do *middle finger </figcaption>
        </figure>
      </td>
    </tr>
  </table>
</div>


#### Vídeos 

<div style="text-align: center;">
  <a href="https://www.youtube.com/watch?v=40anKBzSLEo">
    <img src="https://img.youtube.com/vi/40anKBzSLEo/0.jpg" alt="Texto alternativo">
  </a>
</div>

[Link do vídeo](https://www.youtube.com/watch?v=40anKBzSLEo)


<div style="text-align: center;">
  <a href="https://www.youtube.com/watch?v=y8ddleaoB3U">
    <img src="https://img.youtube.com/vi/y8ddleaoB3U/0.jpg" alt="Texto alternativo">
  </a>
</div>

[Link do vídeo](https://www.youtube.com/watch?v=y8ddleaoB3U)


#### Conclusões

Através da análise dos gráficos e dos vídeos apresentados, é possível observar que, quando o middle finger opera com o dobro da velocidade do polegar, este último acaba por posicionar-se sobre o middle finger. 


### Velocidades relativas entre falanges

Nas figuras a seguir, são apresentados os gráficos de posição, velocidade e corrente do middle finger quando a primeira falange opera com o dobro da velocidade da ponta.

Adicionalmente, são disponibilizados os links para os vídeos no YouTube que documentam esta experiência, proporcionando uma análise mais detalhada e uma visualização em tempo real do comportamento da mão robótica.

#### Gráficos

Posições

<div style="text-align: center;">
  <table>
    <tr>
      <td>
        <figure>
          <img src="/assets/images/semana11/finger_pos3_finger_1.png" alt="1 KOhm" width="200">
          <figcaption>Posições com velocidades dos motores iguais </figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana11/finger_pos4_finger_1.png" alt="10 KOhm" width="200">
          <figcaption>Posições com velocidade do primeiro motor superior </figcaption>
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
          <img src="/assets/images/semana11/finger_vel3_finger_1.png" alt="1 KOhm" width="200">
          <figcaption>Velocidades com velocidades dos motores iguais </figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana11/finger_vel4_finger_1.png" alt="10 KOhm" width="200">
          <figcaption>Velocidades com velocidade do primeiro motor superior </figcaption>
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
          <img src="/assets/images/semana11/finger_curr3_finger_1.png" alt="1 KOhm" width="200">
          <figcaption>Correntes consumidas com velocidades dos motores iguais </figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana11/finger_curr4_finger_1.png" alt="10 KOhm" width="200">
          <figcaption>Correntes consumidas com velocidade do primeiro motor superior </figcaption>
        </figure>
      </td>
    </tr>
  </table>
</div>


#### Vídeos 

<div style="text-align: center;">
  <a href="https://www.youtube.com/watch?v=8vk7IcXj0z4">
    <img src="https://img.youtube.com/vi/8vk7IcXj0z4/0.jpg" alt="Texto alternativo">
  </a>
</div>

[Link do vídeo](https://www.youtube.com/watch?v=8vk7IcXj0z4)


<div style="text-align: center;">
  <a href="https://www.youtube.com/watch?v=QenLMoWv7jk">
    <img src="https://www.youtube.com/watch?QenLMoWv7jk/0.jpg" alt="Texto alternativo">
  </a>
</div>

[Link do vídeo](https://www.youtube.com/watch?v=QenLMoWv7jk)


## Código desenvolvido durante a semana


 Durante esta semana, foram desenvolvidas e implementadas várias melhorias no código, nomeadamente:

 - Desenvolvimento da classe Finger com o nome do dedo, fator de velocidade relativa do dedo e fatores de velocidades relativas entre juntas:

 ```python
class Finger:
    def __init__(self, name, factor, motor_factors):
        self.name = name
        self.factor = factor  # Velocidade relativa do dedo
        self.motor_factors = motor_factors  # Velocidades relativas dos motores do dedo

    def get_motor_speed(self, motor_name):
        return PROFILE_VELOCITY_VALUE * self.factor * self.motor_factors.get(motor_name, 1.0)
    
    def get_finger_speed(self):
        return PROFILE_VELOCITY_VALUE * self.factor
```

- Desenvolvimento de um dicionário de dedos com todos os fatores de velocidades relativas:

 ```python
    self.fingers = [
                Finger("index", 1, {"0": 1, "1": 1, "2": 1, "3": 1}),   # finger_0
                Finger("middle", 1, {"4": 2, "5": 1, "6": 1, "7": 1}),  # finger_1
                Finger("ring", 1, {"8": 1, "9": 1, "10": 1, "11": 1}), # finger_2
                Finger("thumb", 1, {"12": 1, "13": 1, "14": 1, "15": 1}) # finger_3
            ]
```

- Desenvolvimento de uma função para a velocidade de um motor de um dedo através do índice do motor:

 ```python
    def get_motor_speed(self,motor_id):
        finger_index = motor_id // 4
    return self.fingers[finger_index].get_motor_speed(str(motor_id))
```

- Desenvolvimento do nó `save_data_node.py` para guardar os dados de todas as experiências com as posições, velocidades e correntes de todos os dedos

- Desenvolvimento do script `check_colisions.py`que através do ficheiro URDF do robot permite obter as posições de cada junta relativamente à base da mão. Este script ainda se encontra em desenvolvimento e será adaptado para verificar a distância relativa entre os dedos e também verificar se existe alguma colisão.

