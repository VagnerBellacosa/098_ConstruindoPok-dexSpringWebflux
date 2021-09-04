# Spring Webflux – Aplicações reativas em Java – Parte 1

### [Spring Webflux – Aplicações reativas em Java – Parte 1](https://imasters.com.br/java/spring-webflux-aplicacoes-reativas-em-java-parte-1)

27 NOV, 2019

### [Você tem um minuto ou dois, para falar de Code Smells? — Parte 1](https://imasters.com.br/banco-de-dados/code-smells)

20 SET, 2019

### [Vamos falar sobre o Graphql?](https://imasters.com.br/apis-microsservicos/vamos-falar-sobre-o-graphql)

Nos últimos tempos a programação reativa tem se expandido cada vez mais,já está sendo abordada por diversas linguagens, mas em Java é algo novo para você? Então essa série de artigos é para você!

Nesta série de artigos irei abordar assuntos como programação reativa, Spring Webflux, Projeto Reactor e Netty.

Neste primeiro vou abordar alguns conceitos gerais antes de me aprofundar em cada um deles, vamos lá 🙂

### O problema:

Esperamos que nossas aplicações sejam escaláveis, precisamos usar os recursos de forma cada vez mais eficiente, o tempo de latência deve ser mínimo, precisamos de respostas rápidas à nossas solicitações, lidamos com solicitações concorrentes (cada thread consome um pouco de memória), APIs tradicionais funcionam de modo síncrono e bloqueante.

Para solucionar isso, tivemos o surgimento dos Callbacks e dos Futures:
Callbacks não tem uma leitura fácil e manutenção dificil, são complexos (podem formar um callback hell), por meio deles não é possível retornar nenhum valor.

Future: retorna uma instância do tipo future e é difícil de escalar para múltiplas operações assíncronas,como alternativa, a partir do Java 8 surgiu o Completable Future que suporta a programação funcional e facílita o uso de múltiplas operações assíncronas, porém, como nada é perfeito, não é uma boa opção para chamadas assíncronas com multiplos items.

### Forma alternativa de desenvolver APIs

- Assíncrona e não bloqueante
- Fora do modelo de thread por solicitação
- Diminui o número de threads criadas

E ai entra a programação reativa, ok, mas o que ela tem de diferente?

- assíncrona e não bloqueante
- Fluxo de dados como um fluxo orientado a eventos/mensagens
- Código mantém estilo da programação funcional.
- BackPressure nos Streams de dados

Voltemos uma atenção maior para a parte de fluxo de dados:
Os dados podem vir de banco de dados, arquivos externos, serviços integrados, outras aplicações, etc, para cada item dessa fonte de dados, temos um evento ou mensagem que é disparado, após a execução desse evento/mensagem, temos uma mensagem de erro ou de que foi completa.
Para um requisição para salvar dados por exemplo,temos um evento de onNext(Product):

```
List <Product> products= productRepository.getAllProducts();
```

Quando chamamos os dados de uma banco de dados por exemplo,a chamada retorna e para cada item é lançado um onNext(Product) e ao terminarmos a listagem temos um evento de onComplete() para informar que nossa requisição terminou.

E se a nossa requisição tiver algum erro?
Em vez de retornar o evento onComplete() teremos o evento de OnError() e nao teremos o conteúdo esperado.

### Reactive Stream Specification

Consiste de um conjunto de especificações para utilização de streams, criada por empresas como Pivotal e Netflix, dentre essas especificações, temos:

- Publishers: A interface Publisher,é um provedor de um número ilimitado de elementos de forma sequencial e os publica de acordo com a demanda que recebe dos seus Subscribers (explicarei o que é a seguir). Um mesmo publisher pode atender a diversos Subscribers dinamicamente em diversos momentos. Os publishers são as nossas fontes de dados.

```
public interface Publisher<T>{public void subscribe (Subscriber<? super T> s);}
```

- Subscriber: Recebe somente uma chamada para o OnSubscribe, depois que passar uma instância do Subscriber para Publisher.subscribe(Subscriber). Nenhuma notificação será recebida até que Subscription.request(long) seja chamado. Depois que a chamada for iniciada: Uma ou mais chamadas do onNext(Object) são lançadas até que o número máximo definido por Subscription.request (long) seja atingido. Uma única chamada de erro onError(Throwable) ou de que foi completa a requisição onComplete(). Essa demanda pode ser sinalizada através da Subscription.request (long) sempre que a instância do Subscriber puder lidar com mais.

```
public interface Subscriber<T>{public void onSubscribe(Subscription s);public void onNext (T t);public void onError(Throwable t);public void onComplete();}
```

E a Subscription:

```
public interface Subscription{public void request (long n);public void cancel ();}
```

- Processor: Representa uma etapa de processamento, que é tanto um Subscriber quanto um Publisher que aceita as normas de ambos.

```
public interface Processor <T,R> extends Subscriber <T>,Publisher <R>{}
```

sendo que :
T- tipo de elemento que é sinalizado para o Subscriber.
R- tipo de elemento que é sinalizado para o Publisher.

Esta foi a primeira parte da série de artigos, na qual falamos sobre alguns pontos e vantagens da programação reativa, no pŕoximo irei abordar algumas Reactive Libraries.

Dúvidas ou feedbacks?

Segue abaixo algumas referências e conteúdos interessantes sobre esse assunto:

[Manifesto Reativo](https://www.reactivemanifesto.org/)

[Streams Reativos](https://www.reactive-streams.org/reactive-streams-1.0.0-javadoc/)

[Streams Reativos](https://www.baeldung.com/java-9-reactive-streams)

[Curso de WebFlux](https://www.udemy.com/course/build-reactive-restful-apis-using-spring-boot-webflux/)

Até a próxima!