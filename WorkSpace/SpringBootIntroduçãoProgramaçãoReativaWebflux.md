## Spring Boot: Introdução à Programação Reativa com Webflux

08/12/2020[Rodrigo Martins](https://atitudereflexiva.wordpress.com/author/rdomartins/)[Deixe um comentário](https://atitudereflexiva.wordpress.com/2020/12/08/spring-boot-introducao-a-programacao-reativa-com-webflux/#respond)[Go to comments](https://atitudereflexiva.wordpress.com/2020/12/08/spring-boot-introducao-a-programacao-reativa-com-webflux/#comments)

A primeira coisa que a maioria dos desenvolvedores pensa quando enfrenta problemas de performance é aumentar a quantidade de recursos de uma máquina – escalonamento vertical – ou aumentar a quantidade de máquinas – escalonamento horizontal. Estou supondo que se está utilizando um modelo *on premise* (os servidores estão na infraestrutura da organização) e não *cloud* (modelos IaaS, PaaS, SaaS). Essas estratégias não são ruins se não há perspectivas de migrar nossas aplicações para a nuvem ou se nosso objetivo é mitigar um problema de performance que já está impactando o usuário final e onerando o suporte de primeiro nível. Ainda que estivéssemos utilizando um modelo cloud, poderíamos, do lado do software, encontrar uma solução que minimize a latência, aumente o throughput e utilize melhor os recursos disponíveis em cada VM provisionada

Algo a se pensar quando estamos analisando uma API tradicional é a sincronização de requisições, o que causa bloqueio de recursos. Se um usuário solicitar a criação de um pedido, por exemplo, via API Rest, os recursos necessários para cumprir essa requisição ficarão bloqueados pelo tempo que durar a requisição, o que impede aquele usuário de fazer novas requisições. Se fosse possível paralelizar essas requisições, impediríamos o bloqueio de recursos e atenderíamos mais rapidamente as várias requisições subsequentes de um usuário utilizando de forma mais eficiente o mesmo hardware.

O [Manifesto Reativo](https://www.reactivemanifesto.org/), que foi publicado em 2014, é uma resposta à necessidade de utilizar melhor os recursos de hardware. Esse manifesto prevê quatro elementos que definem um sistema reativo:

**1. Responsivo:** o sistema responde em tempo hábil se possível;

**2. Resiliente:** o sistema permanece responsivo quando ocorre uma falha;

**3. Elástico:** o sistema permanece responsivo sob carga variável;

**4. Orientado a mensagens:** sistemas reativos passam mensagens assíncronas para estabelecer os limites entre os componentes, o que garante baixo acoplamento, isolamento e localização transparente.

Mas o que é reatividade? Fazendo uma analogia, você poderia pensar em reatividade como células de uma planilha eletrônica. Dadas as células A1 e A2, queremos que a célula A3 exiba o resultado da soma A1+A2. Se você alterar o valor presente em A1, a célula A3 reagirá à mudança sem que você precise agir manualmente. Reatividade, de acordo com a [documentação do Webflux](https://docs.spring.io/spring/docs/current/spring-framework-reference/web-reactive.html), se refere a modelos de programação que são construídos para reagir a mudanças – componentes de rede reagindo a eventos de I/O, controladores de UI reagindo a eventos de mouse, etc. Desta forma, ser não bloqueante é ser reativo, pois, ao invés de ser bloqueado, nós agora podemos reagir às notificações de operações finalizadas ou disponibilidade de dados por exemplo.

Quando se fala em reatividade do ponto de vista de uma aplicação, entende-se que a aplicação trabalha com fluxos de dados contínuos (data streams). Esses fluxos recebem duas classificações:

**Could streams:** fluxos de dados lazy (pull based), ou seja, são conjuntos de dados que ficam aguardando por subscribers.



Anúncios

DENUNCIAR ESTE ANÚNCIO



**Hot streams:** fluxos de dados ativos (push based), ou seja, estão aguardando a chegada de novos dados para encaminhar aos subscribers registrados

Um problema que pode ocorrer ao se inscrever para lidar com data streams muito grandes é o backpressure. Backpressure é uma espécie de controle de vazão realizado sobre os data streams para evitar estouro de memória, pois pode ser que o fluxo de dados seja muito maior do que a capacidade de processamento do subscriber.

**Programação Reativa com Webflux**

A **Programação Reativa** é um paradigma funcional, orientado a eventos, não bloqueante, assíncrono e centrado no conceito de processamento de data streams (fluxos de dados) e é um estilo de programação cujas bases foram estabelecidas pelo **Manifesto Reativo**. A reatividade é possível através da implementação do padrão [Observer](https://www.dofactory.com/net/observer-design-pattern) que, grosso modo, diz algo como: “não precisa me perguntar se tenho trabalho para você; quando aparecer alguma coisa, eu aviso você e todos os seus colegas sem que vocês precisem ficar me perguntando!”. Por esse padrão, os dados são entregues ao consumidor, que é chamado de subscriber, pois ele se inscreve em um publisher, que publica streams de dados de forma assíncrona. Dessa forma, aplicações reativas escalam melhor e são mais eficientes quando estão lidando com muitos streams de dados.

A reatividade está presente no Java através do projeto [Reactor](https://projectreactor.io/), que é uma implementação da especificação [Reactive Streams](https://www.reactive-streams.org/). Esse projeto oferece dois tipos de publicadores: **Mono** e **Flux**:

**Flux:** é um publisher que produz de 0 à N valores e deve ser utilizado em operações que retornam múltiplos elementos

**Mono:** é um publisher que produz de 0 à um valor e é utilizado para operações que retornam um elemento.

O Spring Framework também implementa um conceito de reatividade através do **Spring Reactor**. O Spring Reactor é uma biblioteca para construir aplicações não bloqueantes na JVM. À partir do Spring 5, foi introduzido o framework Spring Webflux, que oferece suporte à programação reativa:

[![img](https://atitudereflexiva.files.wordpress.com/2020/12/webflux.jpg?w=150&h=84)](https://atitudereflexiva.files.wordpress.com/2020/12/webflux.jpg)

O Spring MVC utiliza o Tomcat como servlet container, mas como ele ainda não dá suporte à especificação servlet 3.0, o Webflux utiliza o Netty, que é performático e não bloqueante. Para escalar, o Spring MVC abre muitas threads, porém, isso pode sobrecarregar o servidor. O Webflux utiliza umas poucas Event Loop Threads que abrem muitas conexões, o que utiliza melhor os recursos disponíveis.

[![img](https://atitudereflexiva.files.wordpress.com/2020/12/webflux_arquitetura.png?w=300&h=110)](https://atitudereflexiva.files.wordpress.com/2020/12/webflux_arquitetura.png)



Anúncios

DENUNCIAR ESTE ANÚNCIO



Uma Event Loop Thread faz uma requisição e libera o recurso – não bloqueante. Por isso, cabe aqui uma observação importante: não basta que o framework seja reativo. Precisamos que o processo seja reativo de ponta a ponta.

**Exemplo**

Nesse exemplo, construiremos duas versões de API Rest: a primeira, bloqueante, utiliza Spring MVC; a segunda, não bloqueante, utiliza Webflux. E para testar, vamos utilizar um plano de teste do [JMeter](https://atitudereflexiva.wordpress.com/2020/03/07/teste-carga-com-apache-jmeter/). Simularemos algumas centenas de usuários realizando algumas centenas de requisições às nossas APIs e compararemos os resultados. Além disso, utilizaremos a imagem do MongoDB para Docker que configuramos em[ outro artigo](https://atitudereflexiva.wordpress.com/2020/09/11/docker-configurando-uma-imagem-do-mongodb/). Para as duas versões da API, utilizaremos o documento Pessoa:

```
@Data``@Document``(collection = ``"pessoas"``)``public` `class` `Pessoa {``  ``@Id``  ``private` `BigInteger _id;``  ` `  ``@Field``(``"codigo"``)``  ``private` `Long codigo;` `  ``@Indexed``  ``@Field``(``"nome"``)``  ``private` `String nome;``}
```

E as seguintes configurações:

```
server.port=``8082``server.servlet.context-path=/pessoa``#mongodb``spring.data.mongodb.host=localhost``spring.data.mongodb.port=``27017``spring.data.mongodb.database=pessoa``spring.data.mongodb.username=rodrigo``spring.data.mongodb.password=``123456``#logging``logging.level.org.springframework.data=debug``logging.level=error
```

**Aplicação Spring MVC**

Aqui, não há qualquer diferença em relação a uma aplicação tradicional. É uma aplicação Spring Boot que utiliza Spring MVC e um repositório para acessar o MongoDB. O POM fica assim:

```
<``dependency``>``  ``<``groupId``>org.springframework.boot</``groupId``>``  ``<``artifactId``>spring-boot-starter-data-mongodb</``artifactId``>``</``dependency``>``<``dependency``>``  ``<``groupId``>org.springframework.boot</``groupId``>``  ``<``artifactId``>spring-boot-starter-web</``artifactId``>``</``dependency``>``<``dependency``>``  ``<``groupId``>org.springframework.boot</``groupId``>``  ``<``artifactId``>spring-boot-devtools</``artifactId``>``  ``<``scope``>runtime</``scope``>``  ``<``optional``>true</``optional``>`` ``</``dependency``>`` ``<``dependency``>``  ``<``groupId``>org.projectlombok</``groupId``>``  ``<``artifactId``>lombok</``artifactId``>``</``dependency``>
```



Definimos o seguinte repositório:

```
public` `interface` `PessoaRepository ``extends` `MongoRepository<Pessoa, Long> {``}
```

E o controller:

```
@RestController``public` `class` `PessoaController {``  ``@Autowired``  ``private` `PessoaRepository repository;`` ` `  ``@PostMapping``(value = ``"/salvar"``)``  ``public` `ResponseEntity<?> save(``@RequestBody` `Pessoa pessoa) {``   ``return` `ResponseEntity.ok(repository.save(pessoa));``  ``}` `  ``@GetMapping``(value = ``"/"``)``  ``public` `List<Pessoa> findAll() {``   ``return` `repository.findAll();``  ``}``}
```

**Aplicação Reativa com Webflux**

Também é uma aplicação Spring Boot, mas usa um repositório diferente:

```
public` `interface` `PessoaRepository ``extends` `ReactiveMongoRepository<Pessoa, Long> {``}
```

Observe o uso de Mono e Flux no retorno dos métodos:

```
@RestController``public` `class` `PessoaController {``  ``@Autowired``  ``private` `PessoaRepository repository;`` ` `  ``@PostMapping``(value = ``"/salvar"``)``  ``@ResponseStatus``(code = HttpStatus.CREATED)``  ``public` `@ResponseBody` `Mono<String> save(``@RequestBody` `Pessoa pessoa) {``   ``return` `repository.save(pessoa).map(g -> ``"Salvo: "` `+ pessoa.getNome());``  ``}` `  ``@GetMapping``(value = ``"/"``, produces = MediaType.TEXT_EVENT_STREAM_VALUE)``  ``@ResponseStatus``(HttpStatus.OK)``  ``public` `Flux<Pessoa> findAll() {``   ``return` `repository.findAll();``  ``}``}
```



Anúncios

DENUNCIAR ESTE ANÚNCIO



**Análise Comparativa dos Resultados Através do Summary Report do JMeter**

Partimos de uma pequena massa de dados de 50000 pessoas inseridas no banco de dados. Vamos observar o resultado da execução do endpoint **pessoa**. Esse endpoint retorna aquelas 50000 pessoas disponíveis no banco de dados. Para simular as requisições bloqueantes, definimos no JMeter 200 usuários executando, cada um, 1000 requisições àquela API. O resultado da versão tradicional com Spring MVC foi:

[![img](https://atitudereflexiva.files.wordpress.com/2020/12/jmeter_springmvc.png?w=595&h=178)](https://atitudereflexiva.files.wordpress.com/2020/12/jmeter_springmvc.png)

O resultado da versão reativa com Webflux foi:

[![img](https://atitudereflexiva.files.wordpress.com/2020/12/jmeter_webflux.png?w=595&h=179)](https://atitudereflexiva.files.wordpress.com/2020/12/jmeter_webflux.png)

Antes de comparar, vamos destacar e definir alguns elementos do [Summary Report](http://return service.findall()./) do JMeter:

**Max:** o tempo máximo que uma resposta demorou para ser retornada (ml). Quanto menor o tempo, menor a latência.

**Std. Dev.:** desvio padrão em relação à média dos tempos de resposta de todas as requests. Quanto menor esse valor, mais consistente é o dado

**Throughput:** quantidade de requests processadas por unidade de tempo.

Como você pode observar, a versão reativa se saiu melhor nos três itens acima: apresentou menor latência, maior consistência da resposta e mais requisições processadas por segundo.

**Pontos a Considerar antes de Adotar a Programação Reativa**

A programação reativa não veio para substituir a programação tradicional. Se você precisa apenas realizar um Put Rest para atualizar um dado de um cliente, o Spring MVC já tem tudo o que você precisará. Esse modelo é indicado para cenários com grande tráfego de dados em que ocorra pouco processamento do lado da aplicação. Sua decisão pela adoção do modelo reativo deve ser orientada pelos seguintes pontos:



\1. Processamento de dados: como processamento de dados é bloqueante, não utilize programação reativa, pois ela só faz sentido se sua aplicação é não bloqueante de ponta a ponta

\2. Bibliotecas e drivers reativos: não há uma grande disponibilidade de bibliotecas e drivers de banco de dados para programação reativa quando comparado ao modelo tradicional

\3. Curva de aprendizado: embora frameworks como o Webflux utilize características já bem conhecidas do Spring MVC e a facilidade de construir uma aplicação com SpringBoot, a API reativa e a programação funcional, que estão intimamente relacionadas, devem ser bem compreendidas

\4. Debug: como estamos observando um fluxo de dados aberto em tempo de execução, não faz sentido colocar um break point e esperar que as coisas simplesmente congelem para que observemos o conteúdo das variáveis. Você precisará fazer bom uso de logs e dos métodos da API reativa, como doOnSuccess e doOnError.

**Referências**

\1. ZETCODE. **Spring Boot MongoDB Reactive tutorial**. ZetCode. Disponível em: [http://zetcode.com/springboot/mongodbreactive/]. Acesso em 18 out. 2020.

\2. SANTOS, Deivid. **Programação Reativa e Webflux**. Deivid Santos. Disponível em: [https://devinnovation.wordpress.com/2020/01/15/programacao-reativa-e-webflux/]. Acesso em 18 out. 2020.

\3. MAGALHÃES, Eder. **Programação Reativa com Spring Boot, WebFlux e MongoDB: chega de sofrer!**. NSTech. Disponível em: [https://medium.com/nstech/programa%C3%A7%C3%A3o-reativa-com-spring-boot-webflux-e-mongodb-chega-de-sofrer-f92fb64517c3]. Acesso em 18 out. 2020.