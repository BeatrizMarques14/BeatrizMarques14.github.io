---
title: "Semana 12 - Implementação de Atrasos Programáveis entre Dedos"
date: "2025-04-07"
categories: [Programação,Materiais,Montagem]
tags: [Programação, ROS, Grasping, Montagem]
math: true

---

A mão humana é extremamente complexa, permitindo uma grande variedade de movimentos, incluindo ações em que os dedos se movimentam de forma independente. É possível, por exemplo, mover apenas um dedo ou iniciar o movimento de vários dedos em momentos distintos, de forma coordenada mas não simultânea.

Ao longo desta semana, o principal foco de trabalho foi a adaptação do código existente de controlo da mão robótica, de modo a permitir a introdução de atrasos temporais (*offsets*) entre os movimentos dos diferentes dedos. Com esta funcionalidade, torna-se possível, por exemplo, iniciar o fecho do dedo médio e, apenas após um intervalo de tempo definido, iniciar o movimento do polegar, aproximando-se assim de comportamentos mais naturais e realistas.

Paralelamente, foi concluída a construção dos dois últimos dedos da Leap Hand, permitindo que todos os quatro dedos estejam agora operacionais para testes e desenvolvimento futuros.


## Implementação de offsets programaveis entre dedos

Tal como referido anteriormente, a mão humana permite realizar movimentos coordenados entre os dedos, mas que não ocorrem necessariamente de forma simultânea. Esta capacidade foi replicada na Leap Hand através da implementação de atrasos temporais entre os movimentos dos dedos.

Nos vídeos seguintes, é demonstrada a ação de fechar e abrir a mão com diferentes configurações de offsets entre os dois dedos atualmente construídos: o dedo médio (*middle finger*) e o polegar.

No primeiro vídeo, o dedo médio inicia o movimento de fecho e, apenas 0.5 segundos depois, o polegar começa a mover-se. No movimento de abertura, a ordem é invertida: o polegar inicia primeiro e o dedo médio começa o seu movimento 0.5 segundos mais tarde.

Na segunda experiência, a sequência é contrária à anterior. O polegar fecha primeiro e o dedo médio inicia o movimento 0.5 segundos depois. Na abertura, o dedo médio começa o seu movimento antes e o polegar espera 0.5 segundos para iniciar a sua trajetória.


### Vídeos

<div style="text-align: center;">
  <a href="https://www.youtube.com/watch?v=dnG1arEnE_w">
    <img src="https://img.youtube.com/vi/dnG1arEnE_w/0.jpg" alt="Texto alternativo">
  </a>
</div>

[Link do vídeo](https://www.youtube.com/watch?v=dnG1arEnE_w)


<div style="text-align: center;">
  <a href="https://www.youtube.com/watch?v=LxN-bPqYuO0">
    <img src="https://img.youtube.com/vi/LxN-bPqYuO0/0.jpg" alt="Texto alternativo">
  </a>
</div>

[Link do vídeo](https://www.youtube.com/watch?v=LxN-bPqYuO0)


## Montagem dos dedos restantes

As figuras seguintes ilustram as diferentes fases do processo de construção do dedo indicador, seguindo integralmente as instruções disponibilizadas pela LEAP Hand. A montagem decorreu conforme previsto, resultando na construção bem-sucedida de um dedo totalmente funcional.

Adicionalmente, foi também construído o dedo polegar. Uma vez que o seu processo de montagem é idêntico ao do dedo indicador, não serão apresentadas imagens detalhadas da sua construção.

<div style="text-align: center;">
  <table style="margin: auto;">
    <tr>
      <td>
        <figure>
          <img src="/assets/images/semana12/1.jpg" alt="" width="200">
          <figcaption></figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana12/2.jpg" alt="" width="200">
          <figcaption></figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana12/3.jpg" alt="" width="200">
          <figcaption></figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana12/4.jpg" alt="" width="200">
          <figcaption></figcaption>
        </figure>
      </td>
    </tr>
    <tr>
      <td>
        <figure>
          <img src="/assets/images/semana12/5.jpg" alt="" width="200">
          <figcaption></figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana12/6.jpg" alt="" width="200">
          <figcaption></figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana12/7.jpg" alt="" width="200">
          <figcaption></figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana12/8.jpg" alt="" width="200">
          <figcaption></figcaption>
        </figure>
      </td>
    </tr>
    <tr>
      <td>
        <figure>
          <img src="/assets/images/semana12/9.jpg" alt="" width="200">
          <figcaption></figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana12/10.jpg" alt="" width="200">
          <figcaption></figcaption>
        </figure>
      </td>
      <td>
        <figure>
          <img src="/assets/images/semana12/11.jpg" alt="" width="200">
          <figcaption></figcaption>
        </figure>
      </td>
    </tr>
  </table>
</div>

Para além das imagens apresentadas, também foi gravado um vídeo ao longo das diferentes fases de construção. No entanto, esse vídeo ainda está a ser editado.

### Mão completa

Após a montagem dos dedos anelar e indicador, procedeu-se à sua fixação na estrutura da LEAP Hand, ficando assim concluída a montagem completa da mão.


<div style="text-align: center;">
    <img src="/assets/images/semana12/leap.jpg" alt="Texto alternativo" width="400">
</div>


## Código desenvolvido durante a semana

Durante esta semana, foram desenvolvidas e implementadas várias melhorias no código, nomeadamente:

 - Desenvolvimento da função `publish_ordered_positions` no nó `set_positions.py` para permitir o envio de posições distintas para diferentes dedos em instantes separados no tempo, através da definição de *offsets temporais*. Esta função funciona da seguinte forma:

    1. As entradas são ordenadas por ordem crescente de offset;
    2. Inicialmente, são enviadas todas as posições dos dedos cujo offset é zero;
    3. O sistema aguarda (utilizando `time.sleep`) o intervalo de tempo correspondente à diferença entre o offset do último grupo de dedos enviado e o offset do próximo grupo;
    4. Após esse intervalo, as posições dos dedos seguintes são enviadas;
    5. Este processo repete-se até que todas as posições tenham sido publicadas. 

```python
    def publish_ordered_positions(self, data):
        # Ordenar os dados com base no offset
        data.sort(key=lambda x: x[-1])

        msg = data[0][:-1] 
        previous_offset = data[0][-1] / 1000  # primeiro offset
        for i in range(1, len(data)):  #
        
            offset = data[i][-1] / 1000
            
            if offset != previous_offset:  
                self.publish_positions(msg)  # Publica as posições acumuladas
                self.get_logger().info(f"Aguardando {offset - previous_offset} segundos antes de enviar a próxima posição...")
                time.sleep(offset - previous_offset) 
                msg = []  

            # Acumula as posições para o mesmo offset
            msg.extend(data[i][:-1])  
            previous_offset = offset  


        self.publish_positions(msg)
```
