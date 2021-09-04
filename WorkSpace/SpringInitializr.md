# Spring Initializr

[Leave a Comment](https://acervolima.com/spring-initializr/#respond) / [Java](https://acervolima.com/category/java/), [Java-Spring](https://acervolima.com/category/java-spring/) / By [Acervo Lima](https://acervolima.com/author/jack_sparrow/)

Spring Initializr é uma ferramenta baseada na Web que gera a estrutura do projeto [Spring Boot](https://pt.acervolima.com/introducao-ao-spring-boot/) . O erro de grafia em initializr é inspirado em [initializr](http://www.initializr.com/) . Os IDEs modernos integraram o Spring Initializr que fornece a estrutura inicial do projeto. É muito fácil para os desenvolvedores selecionar a configuração necessária para seus projetos. A ferramenta Spring Initializr cuida da configuração a seguir para qualquer projeto baseado em Spring. 

- Ferramenta de construção ( [Maven](https://www.geeksforgeeks.org/introduction-apache-maven-build-automation-tool-java-projects/) ou [Gradle](https://pt.acervolima.com/gradle-build-tool-i-modern-open-source-build-automation/) ) para construir o aplicativo.
- Versão do Spring Boot (as dependências são adicionadas com base na versão).
- Dependências necessárias para o projeto.
- Idioma e sua versão.
- Metadados do projeto como nome, embalagem (Jar ou War), nome do pacote etc.

Com todas as informações fornecidas, o spring Initializr gera a estrutura do projeto Spring. Podemos usar o Spring Initializr a partir da Web ou IDE ou linha de comando.

**Spring Initializr Web**

Vamos aprender como usar a IU da web do Spring Initializr para gerar o projeto Spring Boot para o qual a etapa anterior é navegar para [start.spring.io](https://start.spring.io/) para obtê-lo. A janela que aparecerá é mostrada abaixo, que é a seguinte:

![IU da Web do Spring Initializr](https://media.geeksforgeeks.org/wp-content/uploads/20210604123105/SI.png)

A IU do Spring Initializr tem as seguintes opções,

- **Projeto:** Usando este, é possível criar um projeto [Maven ou Gradle](https://pt.acervolima.com/diferenca-entre-gradle-e-maven/) , ou seja; Maven ou Gradle podem ser usados como uma ferramenta de construção. A opção padrão é Projeto Maven. O projeto Maven é usado em todo o tutorial. 
- **Linguagem:** Spring Initializr fornece [Java](https://www.geeksforgeeks.org/java/) , [Kotlin](https://www.geeksforgeeks.org/kotlin-programming-language/) e Groovy como linguagem de programação para o projeto. Java é a opção padrão.
- **Versão do Spring Boot:** Usando este, é possível selecionar a versão do Spring Boot para seu projeto. A versão mais recente do Spring Boot é 2.5.0. As versões SNAPSHOT estão em desenvolvimento e não são estáveis.
- **Dependências do projeto:** Dependências são artefatos que podemos adicionar ao projeto. Estou selecionando Dependência da Web.
- **Metadados do projeto:** São as informações sobre o projeto. 

As informações nos metadados incluem os pontos-chave abaixo:

 **ID do grupo:** é o ID do grupo do projeto.

- Artefato: é o nome do Aplicativo.
- Nome Nome do aplicativo.
- Descrição: Sobre o projeto.
- Nome do pacote: é a combinação do grupo e do ID do artefato.
- Embalagem: Usando este frasco ou embalagem de guerra pode ser selecionada

**Gerar:** Quando a opção gerar é clicada, o projeto é baixado em formato zip. O arquivo zip pode ser descompactado e o projeto pode ser carregado em um IDE.

**Explorar:** Permite ver e fazer alterações no projeto gerado. 

**Estrutura do projeto:** o projeto Spring Boot tem a seguinte aparência:

![Estrutura do projeto de demonstração do Spring Boot](https://media.geeksforgeeks.org/wp-content/uploads/20210604131533/Screenshot20210604at11426PM-483x660.png)

Métodos: 

O Spring Initializr pode ser alcançado de duas maneiras, as seguintes:

1. Usando IDE
2. Usando 

**Método 1:** Spring Initializr usando IDE

Spring Initializr é suportado por vários IDEs como Spring Tool Suite (STS), IntelliJ IDEA Ultimate e IntelliJ IDEA Community Edition (opções de configuração limitada), Netbeans e VSCode.

Se você estiver usando o Netbeans, o [plug-in Spring Initializr](https://github.com/AlexFalappa/nb-springboot) pode ser adicionado ao IDE.

Se você estiver usando o VSCode, o [plugin vscode-spring-initializr](https://github.com/microsoft/vscode-spring-initializr) pode ser adicionado ao VSCode.

Uso do IntelliJ Community Edition:

**Passos a serem seguidos:**

1. Abra o IntelliJ CE IDE.
2. Clique em Novo Projeto.
3. Selecione o projeto Maven e o caminho inicial do Java JDK
4. Insira os detalhes dos metadados do projeto
5. Clique no botão Concluir.

Vamos representar pictoricamente as etapas acima para obter uma compreensão mais justa

- Abra o IntelliJ CE IDE.
- Clique em Novo Projeto.

![Clique em Novo Projeto](https://media.geeksforgeeks.org/wp-content/uploads/20210604154447/IntelliJCE1.png)

- Selecione Projeto Maven e caminho inicial Java JDK.

![Selecione o projeto Maven e o caminho do Java SDK](https://media.geeksforgeeks.org/wp-content/uploads/20210604154828/IntelliJCE2.png)

- Insira os detalhes dos metadados do projeto, como Nome, GroupId, ArtifactId.

![Insira os detalhes dos metadados do projeto, como Nome, GroupId, ArtifactId](https://media.geeksforgeeks.org/wp-content/uploads/20210604154943/IntelliJCE3.png)

- Clique no botão Concluir.

> **Observação:** se você estiver usando o IntelliJ Ultimate Edition, as etapas estão disponíveis [aqui](https://www.jetbrains.com/help/idea/new-project-wizard.html#spring-init) .

**Método 2:** Spring Initializr usando linha de comando

Muitos desenvolvedores adoram fazer coisas na linha de comando. Para eles, existe a opção de criar o projeto Spring usando utilitários de linha de comando como cURL ou HTTPie. Para usar cURL ou HTTPie, é necessário instalá-los antes do uso.

```
curl https://start.spring.io
```

O comando acima fornecerá instruções completas sobre como criar um projeto usando curl.

![Spring Initializr usando curl](https://media.geeksforgeeks.org/wp-content/uploads/20210604163550/CURL.png)

Vamos supor que você deseja gerar um projeto **demo.zip** baseado no Spring Boot 1.5.2.RELEASE, usando as dependências das ferramentas da web e do desenvolvedor (lembre-se de que esses dois IDs são exibidos nos recursos do serviço):

```
$ curl https://start.spring.io/starter.zip -d dependencies=web,devtools \
           -d bootVersion=1.5.2.RELEASE -o demo.zip.zip
```

Exatamente o mesmo projeto também pode ser gerado usando o comando HTTP:

```
$ http https://start.spring.io/starter.zip -d dependencies==web,devtools \
           -d bootVersion==1.5.1.RELEASE -o demo.zip
```

> **Nota:** Além de tudo isso, as equipes do Spring Initializr também fornecem APIs extensíveis para a criação de projetos baseados em JVM. Além disso, pode-se criar sua própria instância do Spring Initializr para seus próprios projetos. Ele também fornece várias opções para o projeto que são expressas em um modelo de metadados. O modelo de metadados nos permite configurar a lista de dependências suportadas por JVM e versões de plataforma, etc.

Artigo escrito por [Ganeshchowdharysadanala](https://auth.geeksforgeeks.org/user/Ganeshchowdharysadanala/articles) e traduzido por Acervo Lima de [Spring Initializr](https://www.geeksforgeeks.org/spring-initializr/). Licença: [CCBY-SA](https://creativecommons.org/licenses/by/4.0/)

### Postagens Relacionadas:

1. [Spring – ApplicationContext](https://acervolima.com/spring-applicationcontext/)
2. [Como criar uma API REST usando Java Spring Boot](https://acervolima.com/como-criar-uma-api-rest-usando-java-spring-boot/)
3. [Contêiner de aplicativos Java | Criação de um aplicativo Spring Boot usando Dockerfile](https://acervolima.com/conteiner-de-aplicativos-java-criacao-de-um-aplicativo-spring-boot-usando-dockerfile/)
4. [Programação Orientada a Aspectos e AOP no Spring Framework](https://acervolima.com/programacao-orientada-a-aspectos-e-aop-no-spring-framework/)