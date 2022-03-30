# plano-saude-database

Uma agência de planos de saúde deseja automatizar seu sistema de cadastro de segurados. Cada cliente deve informar os dados dados pessoais (nome, data de nascimento e email) e pode possuir diversos dependentes. Cada um dos segurados deve estar associado a um contrato (que possui os segurados, data de início da vigência e os produtos associados). Cada produto (plano de saúde) possui um código de registro na ANS, uma descrição e o valor.

```sql
CREATE TABLE cliente (
    id int not null primary key generated always as identity,
    nome varchar(100), 
    data_nascimento date,
    email varchar(100)
)

CREATE TABLE produto (
    id int not null primary key identity generated always as identity,
    cod_ans int not null unique,
    descricao varchar(100),
    valor money
)

CREATE TABLE contrato (
    id int not null primary key identity generated always as identity,
    data_inicio date not null
)

CREATE TABLE contrato_produto (
    id int not null primary key identity generated always as identity,
    contrato_id int not null,
    produto_id int not null
    constraint contrato_fk foreign key (contrato_id) references contrato(id),
    constraint produto_fk foreign key (produto_id) references produto(id)
)

CREATE TABLE contrato_cliente (
    id int not null primary key identity generated always as identity,
    contrato_id int not null,
    cliente_id int not null,
    constraint contrato_fk foreign key (contrato_id) references contrato(id),
    constraint cliente_fk foreign key (cliente_id) references cliente(id)
)

CREATE TABLE dependente (
    id int not null primary key identity generated always as identity,
    titular int not null,
    dependente int not null,
    contrato_id int not null,
    constraint titular_fk foreign key (titular) references cliente(id),
    constraint dependente_fk foreign key (dependente) references cliente(id),
    constraint contrato_fk foreign key (contrato_id) references contrato(id),
    constraint dependente_unique unique(titular, dependente)
)
```
