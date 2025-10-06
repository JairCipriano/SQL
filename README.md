 Desafio de Projeto de Banco de Dados - E-commerce

Este repositório documenta a solução para um desafio de projeto de banco de dados, que envolveu a análise e o refinamento de um esquema E-R (Entidade-Relacionamento) para uma plataforma de e-commerce. O objetivo foi evoluir um modelo inicial para suportar regras de negócio mais complexas, como diferentes tipos de clientes, múltiplas formas de pagamento e um sistema de entrega detalhado.

 Tecnologias Utilizadas
- **MySQL Workbench** (para modelagem E-R e engenharia direta)
- **SQL** (Linguagem de Definição de Dados - DDL)
- **GitHub** (para versionamento e documentação do projeto)
- **Markdown** (para a criação deste README)

 O Esquema do Banco de Dados

O modelo final foi projetado para ser robusto, normalizado e escalável, atendendo a todos os requisitos propostos no desafio.

![Diagrama ERD Final](DesafioSQL/DesafioSQL.png)

 Destaques da Modelagem

O refinamento do esquema inicial focou em quatro áreas principais para aumentar a sua completude e adequação a um sistema moderno.

1. Clientes: Pessoa Física e Jurídica (Generalização/Especialização)
Para atender ao requisito de que um cliente pode ser Pessoa Física (PF) ou Pessoa Jurídica (PJ), mas nunca ambos, foi implementado o conceito de **Generalização/Especialização**.
- A tabela `Clientes` atua como uma **superclasse**, contendo os dados comuns a todos os clientes (ID, Endereço).
- As tabelas `Pessoa Fisica` e `Pessoa Juridica` são **subclasses** que contêm os atributos específicos de cada tipo (`CPF`/`Data de Nascimento` para PF e `CNPJ`/`Razão Social` para PJ).
- O relacionamento 1-para-1 entre a superclasse e as subclasses garante a integridade da regra de negócio.

2. Múltiplas Formas de Pagamento
Para permitir que um cliente tenha várias formas de pagamento cadastradas, foi criada a tabela `Formas de pagamento`.
- Ela se relaciona com `Clientes` em um modelo **1-para-N (Um-para-Muitos)**.
- Isso permite que um único cliente possua múltiplos registros de pagamento (diferentes cartões de crédito, PIX, etc.) de forma organizada.

3. Detalhamento de Entrega
Para adicionar as informações de `status` e `código de rastreio`, e permitir que um pedido possa ser dividido em múltiplos envios, foi criada a tabela `Entrega`.
- Ela se relaciona com `Pedido` em um modelo **1-para-N**, onde um pedido pode ter várias entregas associadas.
- Essa abordagem é mais flexível e escalável do que simplesmente adicionar colunas à tabela `Pedido`.

4. Estrutura de Marketplace (Relacionamentos N:M)
O núcleo do modelo utiliza diversas **tabelas associativas (ou de ligação)** para gerenciar os relacionamentos Muitos-para-Muitos (N:M), característicos de uma plataforma de marketplace:
- **`Produto por Pedido`**: Conecta `Produtos` e `Pedidos`.
- **`Produtos_por_Vendedor`**: Conecta `Produtos` e `Vendedores` terceirizados.
- **`Disponibilizar produtos`**: Conecta `Produtos` e `Fornecedores`.
- **`Estoque_de_Produtos`**: Conecta `Produtos` e diferentes locais de `Estoque`.

Arquivos do Repositório

* **`Desafio SQL.png`**: Imagem do diagrama Entidade-Relacionamento (ERD) final.
* **`ScriptSQL.sql`**: Script SQL completo com os comandos `CREATE` para todas as tabelas, gerado via Forward Engineering no MySQL Workbench.
* **`README.md`**: Este arquivo de documentação.

Conclusão

Este desafio prático foi uma excelente oportunidade para aplicar conceitos avançados de modelagem de dados, como especialização, e para estruturar um esquema coeso e normalizado para um cenário de negócio real. O resultado é um banco de dados bem projetado, pronto para ser implementado.

# Desafio de Projeto: Modelagem de Banco de Dados para Oficina Mecânica

Este repositório documenta a solução para um desafio de projeto de banco de dados, que envolveu a criação de um esquema conceitual do zero para um sistema de gerenciamento de uma oficina mecânica. A partir de uma narrativa de negócio, o objetivo foi identificar e modelar todas as entidades, atributos e relacionamentos necessários para construir um sistema de controle de ordens de serviço.

### Tecnologias Utilizadas
- **MySQL Workbench** (para modelagem E-R e engenharia direta)
- **SQL** (Linguagem de Definição de Dados - DDL)
- **GitHub** (para versionamento e documentação do projeto)
- **Markdown** (para a criação deste README)

 O Esquema do Banco de Dados
O modelo final foi projetado para ser robusto, normalizado e escalável, atendendo a todos os requisitos propostos na narrativa do desafio. Ele estabelece uma base sólida para um sistema de controle e gerenciamento de serviços automotivos.

![Diagrama ERD Final](esquema_oficina.png)
*(Lembre-se de que o arquivo de imagem do diagrama no seu repositório deve se chamar "esquema_oficina.png" para que a imagem apareça aqui)*

 Destaques da Modelagem
A criação do esquema focou em estruturar corretamente o fluxo de informações da oficina, garantindo a integridade e a rastreabilidade dos dados.

1. A Ordem de Serviço como Entidade Central**
O coração do sistema é a tabela `OrdemServico`. Ela centraliza as operações e se conecta com todas as outras entidades principais:
- Relaciona-se com um `Veiculo`, que por sua vez pertence a um `Cliente`.
- É atribuída a uma `Equipe` de mecânicos responsável pela sua execução.
- Funciona como o ponto de partida para registrar os serviços e peças utilizados.

2. Relacionamentos 1-para-N Fundamentais**
Para estruturar as informações de forma organizada, diversos relacionamentos de **Um-para-Muitos (1:N)** foram implementados:
- **Cliente e Veiculo**: Um cliente pode possuir múltiplos veículos, mas cada veículo pertence a um único cliente.
- **Equipe e Mecanico**: Uma equipe é composta por vários mecânicos, mas cada mecânico está alocado em uma única equipe.

3. Gerenciamento de Serviços e Peças (Relacionamentos N:M)**
Um dos pontos mais importantes do modelo é a gestão flexível de serviços e peças em cada ordem de serviço. Para isso, foram usadas **tabelas associativas** para gerenciar os relacionamentos **Muitos-para-Muitos (N:M)**:
- **OSServicos**: Conecta `OrdemServico` e `Servico`, permitindo que uma OS tenha múltiplos serviços e que um mesmo tipo de serviço seja aplicado em várias OS.
- **OSPecas**: Conecta `OrdemServico` e `Pecas`, permitindo que uma OS utilize diversas peças e que um mesmo tipo de peça seja usado em múltiplas OS. A coluna `Quantidade` nesta tabela é crucial para o controle de estoque e cálculo de custos.

4. Separação de Dados com Tabelas de Referência**
Para evitar redundância e facilitar a manutenção, foram criadas tabelas que funcionam como catálogos:
- **Servico**: Armazena a lista de todos os serviços que a oficina oferece, com seu respectivo valor de mão-de-obra.
- **Pecas**: Funciona como um catálogo de peças, contendo seus nomes e valores unitários.

Essa abordagem normalizada garante que os preços sejam consistentes e fáceis de atualizar.

 Arquivos do Repositório
* **`esquema_oficina.png`**: Imagem do diagrama Entidade-Relacionamento (ERD) final.
* **`script_oficina.sql`**: Script SQL completo com os comandos `CREATE` para todas as tabelas, gerado via *Forward Engineering* no MySQL Workbench.
* **`README.md`**: Este arquivo de documentação.

 Conclusão
Este desafio prático foi uma excelente oportunidade para aplicar os conceitos fundamentais de modelagem de dados, traduzindo uma narrativa de negócio em um esquema relacional lógico e funcional. O resultado é um banco de dados coeso, normalizado e pronto para ser a base de um sistema de gerenciamento de oficina completo.

 Autor
**Francisco Jair Cipriano Nunes**
- [LinkedIn](http://www.linkedin.com/in/jair-cipriano-166984239)
- [GitHub](https://github.com/JairCipriano)
