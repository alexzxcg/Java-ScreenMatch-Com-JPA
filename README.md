# Java: persistência de dados e consultas com Spring Data JPA

Projeto desenvolvido no segundo curso da formação Avançando com Java da Alura


## 🔨 Objetivos do projeto

- Evoluir no projeto Screenmatch, iniciado no primeiro curso da formação, criando um menu com várias opções;
- Modelar as abstrações da aplicação através de classes, enums, atributos e métodos;
- Consumir a API do ChatGPT;
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
