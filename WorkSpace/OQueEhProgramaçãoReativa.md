# O que é programação reativa?

Confira neste artigo o que vem a ser programação reativa, um princípio de desenvolvimento de software que vem ganhando espaço entre os desenvolvedores.

 8 meses atrás

[Artigos](https://www.treinaweb.com.br/blog)O que é programação reativa?

Programação reativa é um modelo ou um paradigma de programação criado inicialmente pela Microsoft que é orientado a fluxo de dados e propagações de estados.

Estes fluxos de dados (que também são chamados de streams) são em grande parte assíncronos, ou seja, as operações são independentes umas das outras e não precisam ser executadas em uma sequência específica.

Todas as ações quando falamos sobre programação reativa são transmitidas e detectadas por um fluxo de dados, como eventos, mensagens, chamadas e até mesmo as falhas. Aplicações reativas, então, são constituídas por reações a alterações nestes fluxos de dados.

### Vantagens e desvantagens da programação reativa

O paradigma reativo possui alguns conceitos, como a assincronia, a utilização de processos não-bloqueantes e orientação a mensagens e eventos. Estes conceitos trazem algumas vantagens de maneira natural.

Aplicações reativas tendem a ser mais escaláveis, pois todos os processos são fundamentados em cima de eventos não-bloqueantes. A execução de tarefas bloqueantes geralmente tem a tendência de criar bloqueios de recursos computacionais (como memória e processador), o que pode atrapalhar na escalabilidade da aplicação.

A programação reativa tem justamente como uma das principais premissas lidar com processos que não sobrecarreguem ou bloqueiem a utilização desses recursos computacionais.

Existem várias estratégias computacionais para a criação de processos não-bloqueantes, como a utilização de sub-rotinas (como as goroutines no Go ou as coroutines no Kotlin).

Essa abordagem também acaba trazendo por decorrência uma utilização mais eficiente dos recursos computacionais e uma responsividade naturalmente maior da aplicação (a aplicação tende a cair em menos situações em que a mesma trava ou oferece respostas lentas).

A programação reativa também oferece mecanismos mais flexíveis e robustos para lidar com situações de erro. Mecanismos naturais para a programação reativa (como a utilização de backpressure para controlar o fluxo de dados nos streams de dados de acordo com os consumidores) e o fato de que até mesmo as notificações de erro serem constituídas por eventos emitidos dentro de streams fazem com que o tratamento de erros seja mais eficiente do que em abordagens mais “tradicionais”.



![RxJS - Programação reativa]()

##### CursoRxJS - Programação reativa

[Conhecer o curso](https://www.treinaweb.com.br/curso/programacao-reativa-com-rxjs)

Porém, também existem desvantagens quando utilizamos o paradigma reativo. Uma delas é a curva de aprendizado: escrever aplicações baseando-se em fluxos de dados assíncronos é algo geralmente bem complicado no começo, pois é uma linha de pensamento com a qual a maioria das pessoas não está habituada inicialmente.

Isso certamente leva até mesmo à escrita de código com uma qualidade não tão boa, até que os conceitos da programação reativa sejam plenamente absorvidos. Outra desvantagem muito notável do paradigma reativo é o processo de logging, depuração e instrumentação.

Como, dentro de aplicações reativas, acabamos tendo inúmeros sub-processos sendo executados de maneira assíncrona e ao mesmo tempo, lidar com o log e o tracing dos eventos na aplicação pode se tornar confuso e um grande desafio, já que não temos operações sendo executadas de maneira sequencial.

Uma boa parte das ferramentas de instrumentação no mercado (como o NewRelic, Scout e Sentry) não oferecem ainda mecanismos plenamente robustos para instrumentação e monitoramento. Isso não quer dizer que os mecanismos existentes sejam ruins, mas eles ainda não apresentam o mesmo nível de maturidade que é oferecida em outras abordagens que não a reativa.

### Os pilares das aplicações reativas

Em 2014, um grupo de desenvolvedores criou a segunda versão de um documento que define o que faz com que aplicações sejam consideradas reativas. Esse documento é conhecido como [Manifesto Reativo](https://www.reactivemanifesto.org/).

Segundo a versão mais recente do Manifesto Reativo, existem 4 pilares que orientam como os sistemas reativos devem ser. São eles:

###### Responsividade (responsiveness)

Aplicações reativas precisam ser muito responsivas, oferecendo um feedback rápido para seus usuários sobre as ações que estes desempenham. Por exemplo: se você está em um e-commerce e clica para ir para o carrinho para fechar a compra, o e-commerce não pode demorar para ir para a página do carrinho ou, caso seja necessário demorar um pouco mais, algum feedback precisa ser dado para o usuário. Para ser responsivo, um sistema deve oferecer interações ricas em tempo real com os usuários, estabelecendo uma margem de tolerância que garantam uma qualidade de serviço. Esse comportamento transmite confiança ao usuário final, além de simplificar o tratamento de erros.

###### Resiliência (resilience)

Aplicações reativas devem reagir e se recuperar de falhas de software, hardware e conectividade, além de serem capazes de “lidar” com o componente que apresenta o comportamento incorreto. Para isso, existem algumas estratégias que podem ser adotadas, como mecanismos de replicação (onde uma aplicação possui uma cópia principal funcional e réplicas “adormecidas” que podem ser instantaneamente acionadas caso a cópia principal comece a falhar), mecanismos de contenção (como uma aplicação que é capaz de “segurar” as requisições realizadas para que sejam processadas novamente quando ela se recuperar de um episódio de falha), mecanismos de isolamento (como em uma aplicação que exibe um feedback de indisponibilidade de uma funcionalidade quando esta começa a apresentar um índice alto de falhas ao invés de permitir aos usuários continuar acessando o processo que está com problemas) e mecanismos de delegação (quando uma aplicação consegue utilizar mecanismos de fallback para ações defeituosas). Qualquer sistema que não esteja resiliente naturalmente não será responsivo após uma falha.

###### Elasticidade (elasticity)

A elasticidade de um sistema é verificada quando ele se mantém responsivo e resiliente, mesmo com altas variações de carga. O sistema irá reagir a grandes cargas, aumentando ou diminuindo os recursos alocados para servir estes mesmos acessos. Por exemplo: uma aplicação que é executada em um servidor com 4GB de memória precisa ser capaz de aumentar a quantidade de memória disponível ou ativar novas réplicas auxiliares quando ela sofrer um pico de acessos simultâneos. Mais que isso, a aplicação precisa ser capaz de voltar à configuração inicial quando este pico de acessos passar, evitando custos desnecessários. Se a aplicação consegue aumentar a quantidade de recursos consumidos em situações de carga, mas não consegue voltar às configurações originais quando estes picos passam, a aplicação não pode ser considerada elástica. E mais importante: esse processo de elasticidade precisa ser muito rápido e automático, garantindo que a aplicação continue responsiva e resiliente.

###### Guiado por mensagens (message driven)

Uma arquitetura orientada por mensagens é a base das aplicações reativas. Um sistema orientado por mensagem reage aos eventos em vez de compor aplicações por múltiplas threads síncronas, onde os sistemas são compostos de gerenciadores de eventos assíncronos e não bloqueantes. Isso quer dizer que, em uma aplicação reativa, ao invés de você ter chamadas explícitas entre componentes (um controller e um repository, por exemplo), você tem na verdade mensagens trocadas de maneira assíncrona entre estes dois componentes. Estas mensagens geralmente são trafegadas entre um meio transparente entre os componentes, como um barramento de eventos (event bus), e acabam constituindo justamente os data streams, citados no início deste artigo.

- [#vantagens](https://www.treinaweb.com.br/blog/tag/vantagens)
- [#desvantagens](https://www.treinaweb.com.br/blog/tag/desvantagens)
- [#pilares](https://www.treinaweb.com.br/blog/tag/pilares)
- [#programação reativa](https://www.treinaweb.com.br/blog/tag/programacao-reativa)
- [#responsividade](https://www.treinaweb.com.br/blog/tag/responsividade)
- [#elasticidade](https://www.treinaweb.com.br/blog/tag/elasticidade)
- [#resiliencia](https://www.treinaweb.com.br/blog/tag/resiliencia)
- [#mensagens](https://www.treinaweb.com.br/blog/tag/mensagens)