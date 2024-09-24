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
