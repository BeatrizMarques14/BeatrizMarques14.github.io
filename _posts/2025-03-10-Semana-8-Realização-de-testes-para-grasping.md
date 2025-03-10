---
title: "Semana 8 - Realização de testes para grasping"
date: "2025-03-10"
categories: Programação
tags: [Programação, ROS, Grasping]
math: true

---

Na semana passada, foi implementado o *Current-based Position Control Mode* com o objetivo de possibilitar a realização de *grasping* de objetos, dado que este modo de controlo permite definir tanto a posição desejada como a corrente máxima aplicada. Nesta semana, foram conduzidos vários testes para avaliar a viabilidade deste método na manipulação de objetos, analisando o seu desempenho e a sua adequação para funções de *grasping*.

## Movimento do dedo para velocidades diferentes

No *Current-based Position Control Mode*, não é possível definir diretamente uma velocidade desejada. No entanto, ao configurar o endereço do *Profile Velocity*, é possível estabelecer um limite máximo de velocidade, permitindo assim algum controlo sobre o movimento. Neste teste, o dedo foi fechado com duas velocidades distintas: a primeira, aproximadamente 1,2 rad/s, e a segunda, significativamente mais elevada, cerca de 23,98 rad/s. A corrente máxima definida foi de 1,8 A, uma vez que o objetivo é apenas analisar a influência da velocidade no movimento do dedo, bem como estudar o comportamento da corrente consumida e da posição ao longo do tempo. As experiências realizadas podem ser visualizadas no vídeo abaixo, e os gráficos de posição, velocidade e corrente são apresentados em seguida para uma análise detalhada.

<div style="text-align: center;">
  <a href="https://www.youtube.com/shorts/_3l9AzUPu9A">
    <img src="https://img.youtube.com/vi/_3l9AzUPu9A.jpg" alt="Texto alternativo">
  </a>
</div>

[Link do vídeo](https://www.youtube.com/shorts/_3l9AzUPu9A)

### Gráficos de posição, velocidade e corrente obtidos

Posições

<div style="text-align: center;">
  <table>
    <tr>
      <td>
        <figure>
          <img src="/assets/images/semana8/vel_test/finger_positions_vel50.png" alt="1 KOhm" width="200">
          <figcaption>Posições para a velocidade de 1.2 rad/s</figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana8/vel_test/finger_positions_vel1000.png" alt="10 KOhm" width="200">
          <figcaption>Posições para a velocidade de 23,98 rad/s</figcaption>
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
          <img src="/assets/images/semana8/vel_test/finger_velocities_vel50.png" alt="1 KOhm" width="200">
          <figcaption>Velocidades para a velocidade de 1.2 rad/s</figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana8/vel_test/finger_velocities_vel1000.png" alt="10 KOhm" width="200">
          <figcaption>Velocidades para a velocidade de 23,98 rad/s</figcaption>
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
          <img src="/assets/images/semana8/vel_test/finger_currents_vel50.png" alt="1 KOhm" width="200">
          <figcaption>Corrente para a velocidade de 1.2 rad/s</figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana8/vel_test/finger_currents_vel1000.png" alt="10 KOhm" width="200">
          <figcaption>Corrente para a velocidade de 23,98 rad/s</figcaption>
        </figure>
      </td>
    </tr>
  </table>
</div>

### Conclusões

A análise dos gráficos de posição obtidos revela que a taxa de variação da posição é significativamente maior para a velocidade de 23,98 rad/s, conforme esperado. Nesta condição, a transição de posição ocorre de forma quase instantânea, resultando numa curva que se assemelha a uma reta vertical. Em contraste, para a velocidade de 1,2 rad/s, a curva de variação da posição é mais suave e apresenta uma inclinação menor, indicando um movimento mais controlado e progressivo do dedo.

A análise dos gráficos de velocidade e corrente consumida demonstra que, para a velocidade de 23,98 rad/s, ocorre um pico acentuado no gráfico de velocidade, uma vez que o dedo atinge a posição desejada quase instantaneamente. Em contraste, para a velocidade de 1,2 rad/s, observa-se uma estabilização gradual na velocidade definida, evidenciando um movimento mais controlado. No que diz respeito à corrente consumida, verifica-se que, como esperado, para a velocidade de 1,2 rad/s, o consumo de corrente é significativamente inferior ao registado para a velocidade de 23,98 rad/s, refletindo o aumento da exigência energética em velocidades mais elevadas.



## Movimento do dedo interrompido por um obstáculo

Teoricamente, no *Current-based Position Control Mode*, quando os motores tentam atingir a posição desejada, mas encontram um obstáculo, eles cessam o movimento e aplicam um torque constante, consumindo a corrente máxima definida. Para esta experiência, foi estabelecida uma corrente máxima de 200 mA, e o teste consistiu em fechar o dedo em duas condições distintas: primeiro sem obstáculo e depois com obstáculo. A experiência foi repetida para duas velocidades diferentes, permitindo analisar o impacto da velocidade na interação do dedo com o obstáculo, bem como a variação da corrente consumida e da posição ao longo do tempo.


<div style="text-align: center;">
  <a href="https://www.youtube.com/shorts/t0O2q5OHMzo">
    <img src="https://img.youtube.com/vi/t0O2q5OHMzo.jpg" alt="Texto alternativo">
  </a>
</div>

[Link do vídeo](https://www.youtube.com/shorts/t0O2q5OHMzo)


### Gráficos de posição, velocidade e corrente obtidos

Posições

<div style="text-align: center;">
  <table>
    <tr>
      <td>
        <figure>
          <img src="/assets/images/semana8/obs_test/finger_pos_vel50_no_obs.png" alt="1 KOhm" width="200">
          <figcaption>Posições para a velocidade de 1.2 rad/s sem obstáculo</figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana8/obs_test/finger_pos_vel50_obs.png" alt="10 KOhm" width="200">
          <figcaption>Posições para a velocidade de 1,2 rad/s com obstáculo</figcaption>
        </figure>
      </td>
    </tr>
        <td>
            <figure>
            <img src="/assets/images/semana8/obs_test/finger_pos_vel500_no_obs.png" alt="1 KOhm" width="200">
            <figcaption>Posições para a velocidade de 23,98 rad/s sem obstáculo</figcaption>
            </figure>
        </td>
        <td>
            <figure>
            <img src="/assets/images/semana8/obs_test/finger_pos_vel500_obs.png" alt="10 KOhm" width="200">
            <figcaption>Posições para a velocidade de 23,98 rad/s com obstáculo</figcaption>
            </figure>
        </td>
  </table>
</div>

Velocidades

<div style="text-align: center;">
  <table>
    <tr>
      <td>
        <figure>
          <img src="/assets/images/semana8/obs_test/finger_vels_vel50_no_obs.png" alt="1 KOhm" width="200">
          <figcaption>Velocidades dos motores para a velocidade de 1.2 rad/s sem obstáculo</figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana8/obs_test/finger_vels_vel50_obs.png" alt="10 KOhm" width="200">
          <figcaption>Velocidades dos motores para a velocidade de 1,2 rad/s com obstáculo</figcaption>
        </figure>
      </td>
    </tr>
        <td>
            <figure>
            <img src="/assets/images/semana8/obs_test/finger_vels_vel500_no_obs.png" alt="1 KOhm" width="200">
            <figcaption>Velocidades dos motores para a velocidade de 23,98 rad/s sem obstáculo</figcaption>
            </figure>
        </td>
        <td>
            <figure>
            <img src="/assets/images/semana8/obs_test/finger_vels_vel500_obs.png" alt="10 KOhm" width="200">
            <figcaption>Velocidades dos motores para a velocidade de 23,98 rad/s com obstáculo</figcaption>
            </figure>
        </td>

  </table>
</div>

Correntes

<div style="text-align: center;">
  <table>
    <tr>
      <td>
        <figure>
          <img src="/assets/images/semana8/obs_test/finger_currs_vel50_no_obs.png" alt="1 KOhm" width="200">
          <figcaption>Correntes para a velocidade de 1.2 rad/s sem obstáculo</figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana8/obs_test/finger_currs_vel50_obs.png" alt="10 KOhm" width="200">
          <figcaption>Correntes para a velocidade de 1,2 rad/s com obstáculo</figcaption>
        </figure>
      </td>
    </tr>
        <td>
            <figure>
            <img src="/assets/images/semana8/obs_test/finger_currs_vel500_no_obs.png" alt="1 KOhm" width="200">
            <figcaption>Correntes para a velocidade de 23,98 rad/s sem obstáculo</figcaption>
            </figure>
        </td>
        <td>
            <figure>
            <img src="/assets/images/semana8/obs_test/finger_currs_vel500_obs.png" alt="10 KOhm" width="200">
            <figcaption>Correntes para a velocidade de 23,98 rad/s com obstáculo</figcaption>
            </figure>
        </td>

  </table>
</div>

### Conclusões

A análise dos gráficos de posição confirma que, para ambas as velocidades, ao encontrar um obstáculo, os motores estabilizam numa posição distinta da desejada e permanecem nela até receberem um novo comando. Nos gráficos de velocidade, observa-se que, na velocidade de 23,98 rad/s, os motores não atingem a mesma velocidade máxima da experiência anterior, pois encontram o obstáculo ainda em fase de aceleração, e a corrente máxima definida de 200 mA limita o seu desempenho. Enquanto que para a velocidade de 1,2 rad/s, os motores atingem aproximadamente a mesma velocidade da experiência anterior, uma vez que não ultrapassam a corrente máxima definida.

O aspeto mais relevante desta experiência é a análise dos gráficos de corrente consumida, que permitem identificar com precisão o momento em que o obstáculo é encontrado. Nestes gráficos, verifica-se que, ao colidir com um obstáculo, os motores aplicam um torque aproximadamente constante, evidenciado pela estabilização da corrente consumida no valor máximo definido. Além disso, independentemente da velocidade máxima estipulada, a corrente nunca excede o limite estabelecido, demonstrando a eficácia do Current-based Position Control Mode na regulação da força aplicada pelos motores.


## Movimento do dedo interrompido por um obstáculo em diferentes posições

Após analisar o comportamento do dedo quando interrompido por um obstáculo, é também relevante compreender o impacto de bloqueios em diferentes motores ao longo do movimento. Para este teste, foi definida uma corrente máxima de 200 mA e uma velocidade de aproximadamente 1,2 rad/s. A experiência foi repetida três vezes, variando a posição do bloqueio: na primeira, o obstáculo impediu o movimento do primeiro motor; na segunda, do segundo motor; e, por fim, na terceira repetição, o bloqueio ocorreu na ponta do dedo.

<div style="text-align: center;">
  <a href="https://www.youtube.com/shorts/Qy4lXXRUP4Q">
    <img src="https://img.youtube.com/vi/Qy4lXXRUP4Q.jpg" alt="Texto alternativo">
  </a>
</div>

[Link do vídeo](https://www.youtube.com/shorts/Qy4lXXRUP4Q)

### Gráficos de posição, velocidade e corrente obtidos

Posições
<div style="text-align: center;">
  <table>
    <tr>
      <td>
        <figure>
          <img src="/assets/images/semana8/obs_pos_test/finger_pos_vel50_obs_motor1.png" alt="1 KOhm" width="200">
          <figcaption>Posições quando o obstáculo bloqueia o motor 1</figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana8/obs_pos_test/finger_pos_vel50_obs_motor2.png" alt="10 KOhm" width="200">
          <figcaption>Posições quando o obstáculo bloqueia o motor 2</figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana8/obs_pos_test/finger_pos_vel50_obs_ponta.png" alt="10 KOhm" width="200">
          <figcaption>Posições quando o obstáculo bloqueia a ponta</figcaption>
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
          <img src="/assets/images/semana8/obs_pos_test/finger_vels_vel50_obs_motor1.png" alt="1 KOhm" width="200">
          <figcaption>Velocidades quando o obstáculo bloqueia o motor 1</figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana8/obs_pos_test/finger_vels_vel50_obs_motor2.png" alt="10 KOhm" width="200">
          <figcaption>Velocidades quando o obstáculo bloqueia o motor 2</figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana8/obs_pos_test/finger_vels_vel50_obs_ponta.png" alt="10 KOhm" width="200">
          <figcaption>Velocidades quando o obstáculo bloqueia a ponta</figcaption>
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
          <img src="/assets/images/semana8/obs_pos_test/finger_currs_vel50_obs_motor1.png" alt="1 KOhm" width="200">
          <figcaption>Correntes quando o obstáculo bloqueia o motor 1</figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana8/obs_pos_test/finger_currs_vel50_obs_motor2.png" alt="10 KOhm" width="200">
          <figcaption>Correntes quando o obstáculo bloqueia o motor 2</figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana8/obs_pos_test/finger_currs_vel50_obs_ponta.png" alt="10 KOhm" width="200">
          <figcaption>Correntes quando o obstáculo bloqueia a ponta</figcaption>
        </figure>
      </td>
    </tr>
  </table>
</div>

### Conclusões

Esta experiência confirma os resultados obtidos anteriormente, demonstrando que a corrente consumida nunca excede o valor máximo definido. No entanto, acrescenta informações importantes sobre o comportamento dos motores que não são diretamente bloqueados pelo obstáculo.

A partir dos gráficos, verifica-se que, quando o obstáculo interseta primeiro o motor localizado mais abaixo no dedo, os restantes motores continuam a movimentar-se para atingir as suas posições definidas, resultando apenas no esforço do motor bloqueado. Por outro lado, quando o obstáculo bloqueia a ponta do dedo, todos os motores são afetados, ficando em esforço e impossibilitados de alcançar as posições desejadas.

## Estrutura do código desenvolvido até ao momento

Na figura seguinte, apresenta-se uma imagem gerada pelo ROS Graph, que ilustra de forma clara a organização do código desenvolvido até ao momento, assim como a estrutura de comunicação entre os nós, destacando os tópicos que estão a ser publicados e subscritos.

<figure style="text-align: center;">
    <img src="/assets/images/semana8/rosgraph.png" alt="Organização do código desenvolvido" style="height: 100px;">
    <figcaption style="margin-bottom: 30px;">Organização do código desenvolvido</figcaption>
</figure>

## Código desenvolvido durante a semana

Durante esta semana, foi desenvolvido o nó `grasping_node.py`, responsável por detetar a interseção de objetos e ajustar a corrente e a velocidade para evitar que sejam esmagados. Este nó apresenta uma lógica semelhante ao `manager_node.py`, mas diferencia-se na forma como processa novos valores. Sempre que há uma leitura de corrente e velocidade, estes são comparados com os valores anteriormente definidos.

Quando ocorre um aumento significativo da corrente e uma redução da velocidade, infere-se que um obstáculo está a ser intersetado pelo dedo, levando à diminuição da `GOAL_CURRENT`. Por outro lado, na ausência de obstáculos, a `GOAL_CURRENT` é aumentada, permitindo atingir a velocidade máxima previamente definida.