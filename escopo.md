# PDV CodeXCode

## Resumo
O projeto PDV CodeXCode é um projeto focado em servir um sistema Ponto de Venda (PDV) para um negócio.
Esse documento delimita as funcionalidades, limitações e restrições do projeto, visando assim um melhor entendimento do escopo

## O que o cliente pode fazer
- Gerenciar usuários
    - Criar conta com senha criptografada via bcrypt.
        - Conta é salva no banco de dados de nome pdv, o banco de dados utilizado nesse projeto é o PostgreSQL
    - Logar no sistema com segurança
        - Autenticação é feita via JSON Web Token
- Visualizar categorias das vendas

## O que o cliente não pode fazer
- Restrições de segurança
    - Usuário não pode acessar certas funcionalidades sem estar devidamente autenticado
    - Detalhar o perfil de outro usuário
    - Editar o perfil de outro usuário
- Restrições gerais
    - Editar o id de si mesmo, de outros usuários ou id de qualquer categoria
    - Criar novas tabelas no banco de dados
    - Criar novas categorias

## Banco de Dados
O banco de dados a ser utiliado é o PostgreSQL

#### Tabelas do banco:
- ### Categorias
    - id (serial primary key)
    - descricao (text not null)
- ### Usuarios
    - id (serial primary key)
    - nome (text not null)
    - email (text not null unique)
    - senha  (text not null)

## Endpoints

- #### `GET` `/categoria`
    Essa rota retorna todas as categorias cadastradas no banco de dados.

    As categorias a seguir são registradas previamente:
    - ### **Categorias**

        -   Informática
        -   Celulares
        -   Beleza e Perfumaria
        -   Mercado
        -   Livros e Papelaria
        -   Brinquedos
        -   Moda
        -   Bebê
        -   Games

- #### `POST` `/usuario`
    Essa rota é utilizada para criação dos usuários
    
    O usuário deve ser informado no body no seguinte padrão:
    ```JSON
    {
        "nome":"nome do usuário",
        "email":"email único no sistema",
        "senha":"senha a ser criptografada com bcrypt"
    }
    ```

    O retorno será um objeto apenas com nome e email do usuário, por razões de segurança em nenhum momento a senha é retornada nas requisições.

- #### `POST` `/login`
    Essa rota retorna o token do usuário caso email e senha estejam corretos
    
    Exemplo de retorno:
    ```JSON
    {
        "usuario":"objeto com dados do usuário com exceção da senha",
        "token":"token para ser utilizado nas outras rotas"
    }
    ```

- #### `GET` `/usuario`
    Rota que detalha as informações do usuário logado

    Exemplo de retorno:
    ```JSON
    {
        "nome":"nome do usuário",
        "email":"email do usuário"
    }
    ```

- #### `PUT` `/usuario`
    Rota para atualizar **todas** as informações do usuário

    As informações devem ser informadas no corpo da requisição da seguinte forma:

    ```JSON
    {
        "nome":"novo nome",
        "email":"novo email",
        "senha":"nova senha"
    }
    ```
## Deploy da aplicação
A aplicação estará rodando no site [cyclic](https://vast-teal-yak-hem.cyclic.app/), lá será possível testar os endpoints.
