# Web Server x Application Server

Apr 9, 2017 • Celso Crivelaro

Foi uma pergunta de uma amiga que me motivou a escrever este post. No começo da minha carreira essa diferença também não me era clara e demorei alguns anos (e alguns tropeços) para entendê-los.

Como acho muito legal compartilhar conhecimento, aqui vai uma explicação de cada um e com montar uma boa arquitetura.

Ambos trabalham como aplicação servidora que usa redes, entendem protocolo HTTP e normalmente respondem HTML. Mas na prática são muito mais que isso, e o objetivo este meu post é explicar o funcionamento de cada um e o porque se deve usar ambos ao mesmo tempo.

## Web Server

Quando foram criados, os web servers foram feitos para apenas servir HTML estático, imagens e pequenos arquivos. Hoje, evoluíram para aplicações robustas com múltiplos propósitos.

Implementações mais conhecidas: • Apache HTTP • Nginx

São aplicações compiladas e otimizadas para trabalhar com redes, gerenciamento de memória e paralelização de tarefas, trabalham com grande desempenho, e normalmente feitas em C, acessando diretamente as bibliotecas do sistema operacional. Ou seja, são “parrudos”.

Algumas funções interessantes que web servers podem desempenhar:

### Servir vários sites

Uma configuração comum em web servers, é ele mesmo responder a vários sites ou aplicações, assim, quando requisição chega pelo mesmo IP mas com host diferente, o web server sabe para quem redirecionar.

### Tempo de resposta

O web server sabe muito bem trabalhar com arquivos estáticos como uma página HTML, assets como imagens, vídeos, arquivos Javascript e CSS. 

Ele consegue fazer isso usando cache e compactação o que dá uma resposta muito mais rápida e menor tempo de transmissão. Se estamos falando de HTTP 2 (nova versão do protocolo HTTP, ainda em implementação), temos várias otimizações como resposta de várias requisições em um request e envio de dados binários. Tudo para otimizar o recurso de rede.

### Funções de Firewall

Um site pode ser um acesso alto repentino ou por um sucesso momentâneo ou mesmo por um ataque massivo por robôs programados por hacker. Para se proteger, o web server pode ter configurações de firewall para se proteger e recusar conexões, evitando uma sobrecarga no uso dos recursos computacionais.

Essas proteções são o próprio cache que envia dados em memória, o bloqueio de requisições vindos do mesmo IP em tempos muito curtos, timeout de conexões, entre outras funções.

### Proxy Reverso e Load Balancer

Outro modo de usar um web server é ele servir como Proxy Reverso, ou seja, ele redireciona as requisições para outra aplicação que responde HTTP e retorna ao usuário quando for respondido.

Com isso, o próprio web server pode trabalhar como balanceador de carga, distribuindo o tratamento de requisições pesadas para outras máquinas.

## Application Server

Essa é o servidor web que utiliza programação. Ao receber uma requisição HTTP com os parâmetros e URL, o servidor de aplicação roda um processamento programado e devolve uma página totalmente dinâmica.

Aqui entram os programadores especializados em uma linguagem de programação que rodam o seu software usando um banco de dados e outras integrações na maioria dos casos.

Alguns exemplos: • Java: Tomcat e Jetty • Ruby: Puma, Unicorn e Passenger • Python: Twisted, Tornado e Gunicorn • PHP: mod_php

Estes servers basicamente carregam as aplicações desenvolvidas em memória e fazem o processamento a partir das URL e dados de entrada. Com as informações processadas, os application servers geram um HTML que é retornado ao usuário.

Desta o site que é fornecido ao usuário é personalizado como o Home Banking, Redes Sociais ou sites de Compras.

O que tornar os applications server mais interessantes, não é só a resposta HTML ou atuar como servidores wes, mas com eles podemos criar também APIs que é uma forma de comunicação entre softwares remotos.

Tradicionalmente o formato de resposta pode ser um arquivo JSON ou XML que é interpretado por uma outra aplicação.

JSON pode ter o seguinte formato:

```
{
  “nome”: “Celso Crivelaro”,
  “site”: “http://crivelaro.me”,
   data_ultima_publicacao: “2017-04-08”
}
```

## Boa Prática: Web Server na frente de Application Server

Por que devemos fazer isso? Se o Application Server sabe fazer boa parte do trabalho, por que preciso de um web server?

Como o web server é mais robusto em trabalhar com conexões, têm funções de Firewall e pode ser um proxy reverso; ele pode trabalhar melhor com muitas conexões e transferindo a parte do processamento lento aos application servers. Assim, uma sobrecarga pode ser facilmente gerida pelo web server, passando a requisições ao application server

Para entender a escala do problema, um web server consegue gerenciar milhares de conexões na mesma máquina. Já o application server em uma máquina robusta aguenta algumas dezenas, chegando em centenas de conexões.

Se usarmos a configuração de proxy, podemos distribuir o processamento em diversas máquinas, podendo fazer um balanceamento e melhorando a disponibilidade, já que em uma eventual falha em uma máquina, as outras continuam respondendo.

### Como se comunicam?

- **Proxy Pass**: ou seja, o webserver pega a requisição HTTP, filtra e repassa ela totalmente para o application server.
- **Unix sockets**: Para quem usa Linux, pode usar um socket interno que basicamente a comunicação é feita por arquivos.
- **CGI (Common Gateway Interface)**: Protocolo para comunicação de proxies que funcionam como web services.

Referências:

- [Comparisons of webservers for Python](https://www.digitalocean.com/community/tutorials/a-comparison-of-web-servers-for-python-based-web-applications)
- Livro: [Desconstruindo a Web](https://www.casadocodigo.com.br/products/livro-desconstruindo-web)
- [Nginx](https://www.nginx.com/)
- [Apache](https://httpd.apache.org/)