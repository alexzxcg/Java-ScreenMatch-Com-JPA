# Java: persistência de dados e consultas com Spring Data JPA

Projeto desenvolvido no segundo curso da formação Avançando com Java da Alura


## 🔨 Objetivos do projeto

- Evoluir no projeto Screenmatch, iniciado no primeiro curso da formação, criando um menu com várias opções;
- Modelar as abstrações da aplicação através de classes, enums, atributos e métodos;
- Consumir a API do [MyMemory](https://mymemory.translated.net/doc/spec.php);
- Utilizar o Spring Data JPA para persistir dados no banco;
- Conhecer vários tipos de banco de dados e utilizar o PostgreSQL;
- Trabalhar com vários tipos de consultas ao banco de dados;
- Aprofundar na interface JPARepository

## O que foi feito até agora?
- Dia 24 de setembro de 2024,
foi criada uma classe Série para representar a entidade com seus dados específicos. No construtor da classe, foi implementado   um tratamento usando Optional para converter o dado de avaliação, que vem como string, em um double. Para o atributo gênero,    foi criado um enum com as classificações de gênero, além de um método que faz a classificação de acordo com os dados     recebidos da API. No construtor, também foi adicionado um tratamento para considerar apenas o primeiro gênero fornecido pela API Ombdb.

- Dia 25 de setembro de 2024, implementação de funcionalidade para listar séries por gênero e integração com API de tradução. Inicialmente, a API do ChatGPT seria utilizada para traduzir a sinopse fornecida pela API OMDB, porém, devido a dificuldades na geração de uma chave utilizável, foi implementada a API MyMemory como alternativa. A integração foi bem-sucedida, e a tradução das sinopses está funcionando corretamente.
  
- Dia 26 de setembro de 2024, foi realizada a implementação da conexão com o banco de dados PostgreSQL para persistir os dados fornecidos pela API do OMDb, relacionados às séries buscadas. Utilizei JPA e Hibernate para realizar o mapeamento ORM (Object-Relational Mapping), permitindo que os dados sejam corretamente representados e armazenados no banco de dados. Foram aplicadas as anotações JPA necessárias para garantir o mapeamento das entidades e relacionamentos.
  
- Dia 27 de setembro de 2024, foi realizada a refatoração do código responsável pela busca de séries e episódios da API OMDb. Agora, o sistema permite ao usuário escolher uma série pelo nome, e os episódios são buscados e persistidos diretamente no banco de dados, ao     invés de serem apenas exibidos no console.
  Foi implementado o mapeamento ORM utilizando JPA e Hibernate para as classes Serie e Episodio. A classe Serie foi ajustada para refletir a relação um para muitos com os episódios, enquanto Episodio agora contém uma referência muitos para um para sua respectiva série.     Com isso, os dados dos episódios são corretamente armazenados e vinculados às suas séries no banco de dados.
  Essa refatoração otimizou o processo de busca e persistência, reduzindo a necessidade de chamadas repetidas à API e garantindo que os episódios estejam associados às séries de forma persistente, utilizando as anotações JPA necessárias para a integridade dos relacionamentos no banco de dados.

- Dia 30 de setembro de 2024, foi implementado dois métodos novos com buscas personalizadas usando as derived queries do [Spring Boot JPA](https://docs.spring.io/spring-data/jpa/reference/jpa/query-methods.html) para realizar buscas com base no nome da série e no nome de um ator. Para viabilizar essas consultas, foi fundamental ler a documentação e entender a estrutura básica de uma derived query no JPA.
  
- Dia 01 de outubro de 2024, aprofundei ainda mais meus conhecimentos sobre derived queries no Spring Boot JPA, criando novos métodos de buscas personalizadas para otimizar a consulta de séries no banco de dados. Utilizei várias anotações e operadores específicos para melhorar a precisão e a performance dessas consultas.

  Entre os novos métodos implementados, destacam-se:

  - Listagem do Top 5 séries: Aqui utilizei o operador OrderBy combinado com Desc, que ordena os resultados por uma coluna específica, como avaliação ou número de temporadas, de forma decrescente. Assim, foi possível limitar o resultado às cinco séries com maior destaque em uma métrica específica.

  - Busca por gênero: Este método permite filtrar séries por um gênero específico. A consulta usa a convenção de nomeação baseada em atributos do modelo, como findByGenre, facilitando a filtragem direta no banco de dados.

  - Consulta personalizada com base em temporadas e avaliação: Para essa consulta, implementei um método que permite buscar séries com um número mínimo de temporadas informado pelo usuário. Usei o operador Equals para garantir que o total de temporadas informado fosse exatamente o desejado, enquanto o operador GreaterThanEqual permitiu definir uma avaliação mínima, assegurando que somente séries com uma pontuação igual ou superior fossem retornadas.
    
  Essas funcionalidades aprimoram a flexibilidade do sistema, permitindo que o usuário refine suas buscas de maneira eficiente e com base em múltiplos critérios, aproveitando ao máximo o poder das derived queries do Spring Data JPA.

- Dia 02 de setembro de 2024, explorei o uso de JPQL (Java Persistence Query Language) para realizar consultas no banco de dados, focando em aumentar a flexibilidade e a manutenibilidade do código. Durante esse processo, refatorei um método e criei um novo, ambos com o objetivo de facilitar futuras refatorações e garantir a portabilidade, caso o banco de dados utilizado venha a ser alterado.
  - Método Refatorado: O método seriesPorTemporadaEAValiacao foi ajustado para buscar séries que tenham até um número máximo de temporadas e uma avaliação mínima, utilizando JPQL:
    ~~~~
    @Query("select s from Serie s WHERE s.totalTemporadas <= :totalTemporadas AND s.avaliacao >= :avaliacao")
    List<Serie> seriesPorTemporadaEAValiacao(int totalTemporadas, double avaliacao);
    ~~~~
    A função associada permite que o usuário forneça os parâmetros diretamente, permitindo filtrar as séries de forma personalizada e exibindo o resultado conforme as especificações.
    
  - Novo Método Criado: Foi implementado o método episodiosPorTrecho, que busca episódios com base em um trecho do título fornecido pelo usuário. Esse método utiliza o operador ILIKE para garantir uma busca insensível a maiúsculas e minúsculas:
    ~~~~
    @Query("SELECT e FROM Serie s JOIN s.episodios e WHERE e.titulo ILIKE %:trechoEpisodio%")
    List<Episodio> episodiosPorTrecho(String trechoEpisodio);
    ~~~~
    Assim, o usuário pode informar parte do nome de um episódio e obter uma lista de resultados com detalhes como o título da série e o número do episódio.

  A escolha de JPQL foi proposital, visando melhorar a flexibilidade do código para futuras refatorações. Como o JPQL é uma linguagem agnóstica de banco de dados, ele permite que as consultas permaneçam funcionais mesmo que haja mudanças no sistema de banco de dados,     por exemplo, de PostgreSQL para MySQL ou outro SGBD. Isso evita a dependência de funcionalidades específicas de um banco de dados, aumentando a portabilidade e reutilização do código em diferentes ambientes. Dessa forma, caso haja necessidade de ajustes nas consultas    ou na troca do banco de dados, as mudanças podem ser feitas de forma mais simples e sem a necessidade de reescrever grandes partes do código.

- Dia 03 de setembro de 2024, implementei dois novos métodos utilizando JPQL com o objetivo de aprofundar meus conhecimentos em consultas personalizadas e manipulação de dados complexos.

  - Listagem do Top 5 Episódios de uma Série: O primeiro método foi criado para listar os 5 episódios mais bem avaliados de uma série específica, informada pelo usuário. A consulta usa JPQL para ordenar os episódios em ordem decrescente com base no atributo de avaliação, garantindo que os episódios com maior pontuação apareçam primeiro. Essa abordagem permite ao usuário visualizar rapidamente os episódios de destaque de uma série, com flexibilidade para adaptar a ordenação conforme necessário.

  - Listagem de Episódios por Data de Lançamento: O segundo método busca todos os episódios lançados a partir de uma data específica informada pelo usuário. A consulta explora o uso de comparações de datas em JPQL, permitindo filtrar episódios com base em seu lançamento. Esse método foi projetado para entender melhor como JPQL trata dados de tipo Date e garantir que as consultas sejam eficientes e portáveis entre diferentes sistemas de banco de dados.

## Conclusão
O Screenmatch representa um marco no aprendizado e aplicação de conceitos avançados de programação em Java, com foco em persistência de dados e consultas complexas usando o Spring Data JPA. Ao longo do desenvolvimento, o projeto incorporou a funcionalidade de consumo de APIs, permitindo que dados sobre séries fossem obtidos de forma dinâmica e enriquecidos com traduções utilizando a API MyMemory.

A estrutura robusta do sistema, fundamentada no uso de JPA e Hibernate, garantiu uma modelagem eficaz das entidades e suas relações, facilitando a persistência e a recuperação eficiente de informações. As implementações de derived queries e JPQL não apenas melhoraram a performance das consultas, mas também proporcionaram ao usuário uma experiência de busca rica e personalizada, permitindo filtrar séries por diversos critérios.

Com esse projeto, fica evidente a importância de uma arquitetura bem planejada e a utilização de boas práticas no desenvolvimento de aplicações. O Screenmatch está posicionado para evoluir e se adaptar a novos desafios, refletindo o compromisso com a qualidade e a inovação no desenvolvimento de software.
