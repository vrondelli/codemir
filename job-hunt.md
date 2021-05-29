# A caça do trabalho(Job Hunt)

Então há um tempo estava procurando algum lugar para trabalhar depois de tirar ferias.

Duas empresas me passaram um teste desses que eles falam que é pra saber como você programa.

Desde que comecei a programar tive contato com um tal de DDD(Domain driven design), já nas primeiras linhas de código essa filosofia de ver o software pela otica do domínio, que é o que realmente importa, porque é nele que estão as regras de negócio e o core do business, já estava presente.

# Então como seria se o domínio fosse a maior preocupação em seu software?

Você precisa modela-lo de forma em que a comunicação entre todos os times envolvidos no projeto e o software em si estejam em sincronia, fica muito mais fácil produzir software de qualidade quando todos estão na mesma pagina, quando todos falam a mesma lingua.

Portanto é importante que quem conhece a necessidade do público, o produto, tem de falar com termos que estejam a associados a conceitos no software.

E qual a relação do DDD, ou pelo menos o meu entendimento do que é isso, que inclusive esta em constante evolução com esses testes?

Quando fui faze-los a minha grande preocupação foi em encontrar conceitos e palavras chaves, que traduzem e condensam as regras de negocio dos projetos, dentro das especificações fornecidas do projeto ou documentação e traduzir em código, de uma forma padronizada, assim criando uma linguagem de dominio, permitindo que a informação corra de uma ponta até a outra no desenvolvimento do projeto, assim possibilitando que todos os players envolvidos saibam exatamente do que estão falando e sabem onde encontrar as definições dos termos dessa linguagem, seja em documentações ou no codigo.

O Primeiro teste é para uma empresa que o CEO entrou em 2020 para forbes antes de 30, com apenas 24 anos. O teste era criar 2 microserviços em linguagens distintas que retorna uma lista de produtos com desconto.

E outro era para outra empresa que está em um ponto muito legal no mercado. Apostaria nela, um serviço de validação de boletos, o mais legal desse teste que a situação era bem real, onde você tem que ir na documentação do banco do brasil e extrair a regra de validação.

# Cenarios bem distintos um do outro:

O primeiro eu não terminei porque não consegui terminar, Fiquei com preguiça de aprender uma outra linguagem além de javascript/typescript, até pensei sem dar um migué e fazer um serviço em js e outro em ts, mas fiquei com vergonha, me diverti bastante em tentar criar as regras de descontos de uma forma que se criasse um padrão para novas regras e esse padrão fosse mais maleável possível em questão de estratégia de negócio, estava até bem confiante em conseguir passar no teste, se não tivesse que fazer uma api em outra linguagem, até tentei em python, mas minha vontade de fazer da melhor forma possivel me deixou na preguiça porque nao queria entregar uma parte razoavelmente boa e outra de qualquer jeito.

Já o segundo eu fiz só metade porque o tempo era apertado, o recrutador até estava me acelerando um pouco, curti bastante fazer a validação do boleto de título bancário, foi um exercício legal de operação de dados, nesse passei no teste. nesse até pensei em um pouco em tentar fazer de uma forma mais funcional, mas ia me gastar mais tempo do que fazer de uma mais imperativativa,

# Segue as regras do primeiro:

O teste consiste em escrever 2 microserviços que possibilitam retornar uma lista de produtos com desconto personalizado para cada usuário.

## Restrições

1.  Os serviços desse teste devem ser escritos usando linguagens distintas
2.  Os serviços desse teste devem se comunicar via [gRPC](https://grpc.io/)
3.  Utilize [docker](https://www.docker.com/) para provisionar os serviços
4.  Para facilitar, os serviços podem usar um banco de dados compartilhado

## Avaliação

1. Conversaremos sobre a estrutura do código, escolha do banco, e outras decisões que foram tomadas
2. Discutiremos como esse sistema evoluiria ao longo do tempo
3. Considere que as regras de descontos irão mudar com o tempo
4. Considere que mais pessoas irão trabalhar junto com você nesse projeto

## Serviço 1: Desconto invidual de produto

- Este serviço recebe um id de produto e um id de usuário e retorna um desconto.

Produto exemplo:

```
{
    id: string
    price_in_cents: int
    title: string
    description: string
    discount: {
        pct: float
        value_in_cents: int
    }
}
```

Usuário exemplo:

```
{
    id: string
    first_name: string
    last_name: string
    date_of_birth: Date
}
```

- As regras de descontos da aplicação são:
  - Se for aniversário do usuário, o produto terá 5% de desconto
  - Se for black friday (nesse exemplo ela pode ser fixada dia 25/11) o produto terá 10% de desconto
  - O desconto não pode passar de 10%

## Serviço 2: Listagem de produtos

- Expõe uma rota HTTP tal que `GET /product` retorne um json com uma
  lista de produtos.

  - Essa rota deve receber opcionalmente via header `X-USER-ID` um id de usuário.

  - Para obter o desconto personalizado este serviço deve utilizar o serviço 1.

  - Caso o serviço 1 retorne um erro, a lista de produtos ainda precisa ser retornada, porém com esse produto que deu erro sem desconto.

  - Se o serviço de desconto (1) cair, o serviço de lista (2) tem que continuar funcionando e retornando a lista normalmente, só não vai aplicar os descontos.

# Segue as regras do segundo

## Teste Back End

O teste consiste em escrever um programa em Node.js que expõe uma API na qual é dada como entrada uma linha digitada de um boleto e que retorna:

- Se a linha digitada é válida
- O valor do boleto, se existir
- A data de vencimento do boleto, se existir
- Os 44 dígitos correspondentes ao código de barras desse boleto

- É essencial que seja feita a validação dos dígitos verificadores.

- Como um dos objetivos do teste é avaliar o raciocínio lógico do candidato, é imprescindível que a validação do boleto seja realizada sem auxílio de bibliotecas de terceiros.

- Existem 2 tipos de boletos que seguem regras diferentes: títulos bancários e pagamentos de concessionárias. O código deve funcionar corretamente para ambos.

- Não há especificações especiais sobre a arquitetura da API. Estruture os endpoints da forma que achar melhor, justificando suas escolhas sempre que possível.

## Documentação

- https://storage.googleapis.com/slite-api-files-production/files/b8def5e9-f732-4749-88ea-25270cb71c4d/Titulo.pdf
- https://storage.googleapis.com/slite-api-files-production/files/222c4ec7-9056-4149-aa42-e66b135f523a/Convenio.pdf

# Resoluções

- https://github.com/vrondelli/billet-validator
- https://github.com/vrondelli/discounts-service

Bonus Compressor de imagem:
https://github.com/vrondelli/images-compressor

Como você faria esses 2 desafios?
