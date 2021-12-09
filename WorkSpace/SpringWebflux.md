[Michelli Brito](https://medium.com/@michellibrito?source=post_page-----f611c8256c53-----------------------------------)

[252 Followers](https://medium.com/@michellibrito/followers?source=post_page-----f611c8256c53-----------------------------------)[About](https://medium.com/@michellibrito/about?source=post_page-----f611c8256c53-----------------------------------)Follow





[Upgrade](https://medium.com/plans?source=upgrade_membership---nav_full-------------------------------------)











# Spring Webflux

[![Michelli Brito](https://miro.medium.com/fit/c/56/56/1*OJMKZNMDAQI7CJPfkB-p3g.png)](https://medium.com/@michellibrito?source=post_page-----f611c8256c53-----------------------------------)

[Michelli Brito](https://medium.com/@michellibrito?source=post_page-----f611c8256c53-----------------------------------) - [Sep 27, 2019·5 min read](https://medium.com/@michellibrito/spring-webflux-f611c8256c53?source=post_page-----f611c8256c53-----------------------------------)



*Este artigo aborda um pouco sobre o módulo Spring Webflux que permite trabalhar com programação reativa em aplicações Java com Spring*

Considerando uma aplicação que utiliza programação síncrona e bloqueante, quando um determinado cliente envia uma requisição para o servidor, supondo que seja uma requisição que solicita um grande volume de dados por exemplo, o processamento levará mais tempo até que o cliente receba de volta essa resposta. E se o cliente, em seguida, enviar mais duas requisições posteriores ao servidor, Requisição 2 e Requisição 3, essas ficarão **bloqueadas** até que a Requisição 1 seja finalizada. Ou seja, gera-se um gargalo na aplicação em casos de processamentos longos. Ao término do processamento da Requisição 1, a Requisição 2 será processada e da mesma maneira, ao término do processamento da Requisição 2, a Requisição 3 será processada. Quando o módulo Spring MVC é utilizado para construir aplicações web, o processamento é dessa maneira, **síncrono e bloqueante**.

![img](https://miro.medium.com/max/1030/1*VGRKa7-0alM7UXI0rBKeTA.png)

Exemplo do processamento de uma aplicação bloqueante

Agora, considerando uma aplicação assíncrona e não bloqueante, ou seja, dados podem chegar em tempo real e serem processados, se o mesmo cliente do exemplo anterior enviar ao servidor a mesma Requisição 1, solicitando um grande volume de dados e em seguida, enviar mais duas Requisições, 2 e 3, a Requisição 1 não mais bloqueará as requisições que forem chegando posteriores a ela. Isso porque como a aplicação é não bloqueante, as requisições podem ser processadas de forma paralela, não criando bloqueios e nem gargalos na aplicação. Quando o módulo Spring Webflux é utilizado para construir aplicações web, o processamento neste caso é **assíncrono e não bloqueante**.

![img](https://miro.medium.com/max/60/1*JfLl8zgDrR7Q2yGeN_to3g.png?q=20)

![img](https://miro.medium.com/max/475/1*JfLl8zgDrR7Q2yGeN_to3g.png)

Exemplo do processamento de uma aplicação não bloqueante

## Spring Webflux

O Spring Webflux é um módulo que foi inserido no Spring Framework 5 e possibilita aplicações web com Spring do lado servidor trabalhar de forma reativa. Para adicioná-lo em uma aplicação Spring Boot, por exemplo, basta inserir sua dependência no arquivo pom.xml e, assim, utilizar todos os recursos que vieram com esse novo módulo.

![img](https://miro.medium.com/max/60/0*XP5oxxzrGvREK-pC?q=20)

![img]()

Dependência do Spring Webflux no arquivo pom.xml

O **Spring** **Webflux** é baseado no projeto **Reactor**, que é uma biblioteca de programação reativa sem bloqueio para a JVM. Assim como o Reactor, ele possui dois tipos: **Flux** e **Mono**. Lembrando que na programação reativa trabalha-se com fluxos e não com dados. Sendo assim, o tipo Flux consiste em um fluxo (stream) de 0 a N elementos e o tipo Mono consiste em um fluxo (stream) de 0 ou 1 elemento apenas.

## Componentes Web Server-Side

Quando se utiliza o Spring Webflux, há duas maneiras de se criar componentes Web Server-Side. A primeira é a forma tradicional, utilizando anotações, assim como utiliza-se com o Spring MVC na criação de Controllers.

![img](https://miro.medium.com/max/60/1*0CrX_mFsLftgKTuyyo-JCg.png?q=20)

![img]()

Componente web criado com anotações (forma tradicional)

A segunda maneira de se criar componentes Web Server-Side é utilizando o novo estilo funcional (Serviço reativo funcional), onde os componentes são declarados via funções para rotear e manipular solicitações. Para isso é preciso criar dois componentes, Handler e Router. O Handler é onde manipula-se as requisições e defini-se os retornos como retorno HTTP, Content-Type e Body da resposta. Já o Router é o componente onde defini-se as rotas através de funções, recebendo como argumento do método o Handler já pré-definido para especificar o retorno para cada rota criada.

![img](https://miro.medium.com/max/60/1*H92-TOVylJ2p1H8QsAiUeg.png?q=20)

![img]()

Exemplo de um Handler

![img](https://miro.medium.com/max/60/1*14qr23lbBFq4d6QJuwKpIA.png?q=20)

![img]()

Exemplo de um Router

## Servlet Container no Spring Webflux

Quando utiliza-se o Spring Webflux para construir uma aplicação web com Spring Boot, um servidor Netty vem embutido para iniciar a aplicação, diferente de quando utilizamos o Spring MVC, que trás embutido um servidor Tomcat.

O **Netty** é uma estrutura de servidor que oferece a infraestrutura não bloqueante e trabalha com tempo de execução assíncrono. Por isso é o mais coerente de ser utilizado quando se trabalha com programação reativa.

## Spring Data com Spring Webflux

O Spring Data é outro módulo do Spring Framework 5 que obteve suporte a programação reativa. Assim, ele permite utilizar os tipos Flux e Mono para serem mapeados como resultado. Na programação reativa é preciso utilizar os bancos de dados com suporte para a programação reativa, para que todo o fluxo seja assíncrono e não bloqueante. Alguns exemplos de bancos de dados NoSQL são: MongoDB, Redis, Apache Cassandra, etc.

Quando utiliza-se esses tipos de banco, ao se fazer uma persistência de um dado, por exemplo, o driver inicia a transação e ao término, cria uma nova instancia desse objeto para ser retornado. Utilizando um banco de dados SQL tradicional, a transação de uma persistência fica bloqueada até que seja finalizada para a mesma referencia do objeto ser retornada, gerando assim, um ponto de bloqueio na aplicação. Por isso, para garantir que a aplicação seja não bloqueante em todos os pontos de processamento, é preciso utilizar drivers já adaptados para esse tipo de programação reativa.

Para utilizar o Spring Data com MongoDB, por exemplo, basta adicionar sua dependência no arquivo pom.xml.

![img](https://miro.medium.com/max/60/0*YlVuAJg1f8Rxs9zR?q=20)

![img](https://miro.medium.com/max/630/0*YlVuAJg1f8Rxs9zR)

Dependência do Spring Data MongoDB

E assim como no JpaRepository, é possivel criar um repository e estender o ReactiveMongoRepository para desfrutar de vários métodos prontos para transações com banco de dados, como por exemplo findAll, findById, entre outros. E também, é possível criar métodos customizados e obter como retorno um tipo Flux ou Mono.

![img](https://miro.medium.com/max/60/0*LKOVJmuBV1j39yTZ?q=20)

![img](https://miro.medium.com/max/630/0*LKOVJmuBV1j39yTZ)

Exemplo de repository com ReactiveMongoRepository

**Quando utilizar o modelo síncrono com Spring MVC ou assíncrono com Spring Webflux**

O Spring Webflux não veio para substituir o Spring MVC. São maneiras diferentes de se construir aplicações web e cada uma tem seu propósito. Por isso, ao escolher entre uma ou outra, é preciso conhecer e definir o objetivo e necessidades da aplicação, para que assim, o melhor módulo seja utilizado.

## Para quem se interessar…

Para quem gostou deste tema e gostaria de aprender mais, segue o link de um curso no Youtube onde há o passo a passo de como construir uma **API REST reativa com Spring Webflux e MongoDB**: https://www.youtube.com/watch?v=jW1YdAb3GZo&list=PL8iIphQOyG-CyD9uuRTMiqxEut5QAKHgahttps://policy.medium.com/medium-terms-of-service-9db0094a1e0f?source=post_page-----f611c8256c53-----------------------------------)