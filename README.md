# A3_BancoDados_SQL_-Rodrigo_Leandro_Mendes_Duarte-
1-Link do DB Fiddle com o script completo (DDL + DML + Queries).

R:https://www.db-fiddle.com/f/bts5u6dvLB6twn9PhtbYQz/1

2-Diagrama ER atualizado (PDF ou PNG).

R: ![image](https://github.com/user-attachments/assets/e7fd7e87-9114-4ec8-a3c7-705c0645889a)

3 - README.md explicando:

Significado de cada tabela e coluna

Como reproduzir o ambiente no DB Fiddle

Exemplos de resultado das consultas

R:Significado das Tabelas e Colunas
Com base no seu DB Fiddle:

Usuario

Propósito: Armazena as informações dos usuários da plataforma.
Colunas:
usuario_id: Identificador único para cada usuário (Chave Primária, Auto Incremento).
nome: Nome completo do usuário.
email: Endereço de e-mail do usuário (Único, para login e contato).
senha: Senha do usuário.
criado_em: Data e hora de quando o usuário foi registrado.
Criptomoeda

Propósito: Catálogo das criptomoedas disponíveis na plataforma.
Colunas:
cripto_id: Identificador único para cada criptomoeda (Chave Primária, Auto Incremento).
nome_cripto: Nome da criptomoeda (ex: Bitcoin, Ethereum).
simbolo: Símbolo/ticker da criptomoeda (ex: BTC, ETH) (Único).
descricao: Breve descrição da criptomoeda.
Carteira

Propósito: Representa as carteiras dos usuários, onde eles podem ter saldo em moeda fiduciária (como BRL, USD) e, indiretamente através da AtivoCarteira, suas criptomoedas.
Colunas:
carteira_id: Identificador único para cada carteira (Chave Primária, Auto Incremento).
usuario_id: Identificador do usuário dono da carteira (Chave Estrangeira referenciando Usuario.usuario_id).
nome_carteira: Um nome dado pelo usuário para identificar a carteira (ex: "Principal", "Investimentos").
saldo_fiat: Saldo em moeda fiduciária (ex: Reais, Dólares) disponível na carteira.
criado_em: Data e hora de criação da carteira.
AtivoCarteira

Propósito: Tabela crucial que vincula uma Carteira a uma Criptomoeda, indicando a quantidade daquela criptomoeda que o usuário possui em uma carteira específica e o preço médio de compra. É o seu portfólio de criptoativos.
Colunas:
ativo_carteira_id: Identificador único para cada registro de ativo em carteira (Chave Primária, Auto Incremento).
carteira_id: Identificador da carteira onde o ativo está (Chave Estrangeira referenciando Carteira.carteira_id).
cripto_id: Identificador da criptomoeda possuída (Chave Estrangeira referenciando Criptomoeda.cripto_id).
quantidade: Quantidade da criptomoeda que o usuário possui nessa carteira.
preco_medio_compra: Preço médio pago pela criptomoeda (útil para calcular lucros/prejuízos).
UNIQUE (carteira_id, cripto_id): Garante que para cada carteira, uma criptomoeda específica apareça apenas uma vez (não faz sentido ter duas entradas de "Bitcoin" para a mesma carteira; a quantidade total deve estar em uma única entrada).
Transacao

Propósito: Registra todas as transações realizadas, como compras, vendas de criptomoedas, depósitos e saques de moeda fiduciária.
Colunas:
transacao_id: Identificador único para cada transação (Chave Primária, Auto Incremento).
carteira_id: Identificador da carteira envolvida na transação (Chave Estrangeira referenciando Carteira.carteira_id).
cripto_id: Identificador da criptomoeda envolvida na transação (pode ser NULL para transações puramente fiduciárias, como um depósito). (Chave Estrangeira referenciando Criptomoeda.cripto_id).
tipo_transacao: Tipo da transação (COMPRA, VENDA, DEPOSITO_FIAT, SAQUE_FIAT, etc.).
quantidade_cripto: Quantidade de criptomoeda transacionada (ex: 0.5 BTC).
preco_unitario_cripto: Preço de uma unidade da criptomoeda no momento da transação (em moeda fiduciária).
valor_total_fiat: Valor total da transação em moeda fiduciária (ex: quantidade_cripto * preco_unitario_cripto para compras/vendas, ou o valor do depósito/saque).
taxa: Taxas da transação (em moeda fiduciária).
data_transacao: Data e hora em que a transação ocorreu.
descricao_transacao: Uma descrição opcional para a transação.
Como Usar/Reproduzir no DB Fiddle
O principal O link (https://www.db-fiddle.com/f/bts5u6dvLB6twn9PhtbYQz/1) já contém:

Painel Esquerdo (Schema SQL): Aqui você coloca os comandos CREATE TABLE para definir a estrutura do seu banco de dados e os comandos INSERT INTO para popular as tabelas com dados de exemplo.

Você pode editar o esquema aqui se precisar fazer alterações (ex: adicionar uma nova coluna, alterar um tipo de dado).
Após editar, clique em "Run" (ou Ctrl+Enter / Cmd+Enter) no topo para aplicar as mudanças. Se houver erros, eles serão mostrados abaixo do painel esquerdo.
Painel Direito (Query SQL): Aqui é onde você escreve e executa suas consultas SQL (SELECT, UPDATE, DELETE, etc.) para interagir com os dados.

Escreva sua consulta.
Clique em "Run" (ou Ctrl+Enter / Cmd+Enter).
Os resultados aparecerão abaixo deste painel.
Para continuar usando e experimentando:

Adicionar mais dados: Vá ao painel esquerdo e adicione mais comandos INSERT INTO abaixo dos existentes. Clique em "Run".
Modificar o esquema: Altere os comandos CREATE TABLE no painel esquerdo. Lembre-se que se já existirem dados, algumas alterações podem falhar (ex: adicionar uma coluna NOT NULL sem valor padrão a uma tabela com dados). Clique em "Run".
Testar consultas: Use o painel direito para escrever suas queries e ver os resultados.

### Exemplos de Consultas SQL

- **Histórico de transações por usuário:** Junta as tabelas `Transacao`, `Carteira`, `Usuario` e `Criptomoeda` para listar todas as compras e vendas de criptoativos de um usuário específico, exibindo data, símbolo e valores.

- **Total movimentado por carteira:** Agrupa as transações por carteira e soma o valor total de cada uma para mostrar o montante que passou por ali, incluindo tanto depósitos quanto compras.

- **Soma de quantidades negociadas por cripto:** Agrupa as transações por criptomoeda e soma as quantidades negociadas.
Isso mostra o volume total movimentado, não o saldo atual. Para o saldo real, a consulta deve ser feita na tabela `AtivoCarteira`, que consolida as posses de cada ativo.
