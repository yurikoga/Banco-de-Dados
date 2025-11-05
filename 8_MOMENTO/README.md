-- Busca da tabela principal
SELECT CONCAT(primeiro_nome, " ",sobrenome ) AS FULL_NAME,
email, 
departamento_nome AS DEPARTAMENTO, 
salario FROM funcionarios

-- juntando a tabela secundária
INNER JOIN departamentos

-- com a condição:
ON funcionarios.departamento_id = departamentos.departamento_id
WHERE funcionarios.departamento_id = 6;


# Momento 

Bem-vindo à base de dados da empresa **Momento**!

A Momento é uma empresa inovadora do setor de tecnologia e vendas de produtos temáticos de super-heróis e cultura pop. Fundada em 1980, a empresa começou como uma pequena loja em Gotham City e hoje possui escritórios em diversos países, com departamentos especializados em Vendas, Tecnologia, Recursos Humanos, Marketing, Financeiro e Dados.

Nestes exercícios, você vai explorar o banco de dados MongoDB da empresa e responder perguntas que revelam insights importantes sobre o negócio. Prepare-se para usar queries básicas, agregações complexas, e operações avançadas do MongoDB!

---

## Nível 1: Conhecendo a Empresa

### Seu Primeiro Dia na Momento

Você acabou de ser contratado pela Momento! Vamos começar incluindo você no sistema.

**1.2** Agora que você faz parte da equipe, quantos funcionários temos ao total na empresa?
R: A empresa possui 43 funcionarios, contando comigo.
   select distinct count(funcionario_id) as qtd_funci_depart, cargos.cargo_nome, cargos.cargo_id FROM cargos inner join funcionarios on funcionarios.cargo_id = cargos.cargo_id group by cargo_id;

**1.3** Quantos funcionários trabalham especificamente no Departamento de Tecnologia?
R: ao todo trabalham 5 funcionários da área de tecnologia.
      select count(funcionarios.funcionario_id) as qtd_funcionarios, departamentos.departamento_nome from funcionarios inner join departamentos on funcionarios.departamento_id = departamentos.departamento_id where funcionarios.departamento_id = 6;
   sendo eles: Alexander Hunold, Bruce Ernst, David Austin, Valli Stark e Diana Lorentz.
      select funcionarios.funcionario_id, concat(funcionarios.primeiro_nome, " ", funcionarios.sobrenome) as nome_funcionario, departamentos.departamento_nome from funcionarios inner join departamentos on funcionarios.departamento_id = departamentos.departamento_id where funcionarios.departamento_id = 6;

**1.4** Liste todos os departamentos que existem na empresa. Quantos são?
R: os departamentos são: Administração, Marketing, Suprimentos, Recursos Humanos, Transportes, Tecnologia, Relações Públicas, Vendas, Executivo, Finanças, Contabilidade, Biotecnologia, Tecnologias Avançadas.
      select departamento_nome as nome_depart from departamentos,
   sendo 13 departamentos diferentes
      select count(departamento_nome) as qtd_depart from departamentos;

**1.5** Quantos escritórios a Momento possui? Em quais países?
R:Possui ao todo 9 escritorios que estão distribuidos nas regiões da: United States of America, Canada, United Kingdom, Wakanda, United Kingdom, Germany.
      select escritorio_nome, escritorios.pais_id, paises.pais_nome from escritorios inner join paises on escritorios.pais_id = paises.pais_id;
      
---

## Nível 2: Análise Financeira Básica

O CFO precisa de relatórios urgentes sobre os custos da empresa.

**2.1** Quantos funcionários trabalham no Departamento de Vendas?
R:Ao todo existem 5 funcionarios que trabalham no Departamento de Vendas, sendo eles: John - Russell, Karen - Partners, Jonathon - Taylor, Jack - Livingston, Kimberly - Grant
   select 
   	concat(funcionarios.primeiro_nome, " - ",funcionarios.sobrenome) funcionarios,
       departamentos.departamento_nome
   from funcionarios 
   inner join departamentos 
   on funcionarios.departamento_id = departamentos.departamento_id 
   where departamentos.departamento_nome = "Vendas";

**2.2** Qual é o custo total com salários do Departamento de Vendas?
R: Custo total com salários do deartamento de vendas é igual a R$ 51500,00
   [select 
    concat("R$ ",sum(funcionarios.salario)) salarios
from funcionarios 
inner join departamentos 
on funcionarios.departamento_id = departamentos.departamento_id 
where departamentos.departamento_nome = "Vendas" ;]


**2.3** Qual é a média salarial da empresa, excluindo os cargos de CEO, CMO e CFO?
R:[ -- query que mostra a média salarial da empresa, excluindo os cargos de CEO, CMO e CFO
select 
	avg(funcionarios.salario)
from funcionarios
join cargos
on funcionarios.cargo_id = cargos.cargo_id
where funcionarios.cargo_id not in(4,7,10);
-- query que formata o valor obtido da média, tendo como resultado: 8,431.54
select format(8431.538718, 2);]

**2.4** Qual é a média salarial do Departamento de Tecnologia?
R:[ -- query que mostra a média salarial do Departamento de Tecnologia
select
    avg(funcionarios.salario)
from funcionarios
join departamentos
on funcionarios.departamento_id = departamentos.departamento_id
where departamentos.departamento_id = 6;
-- query que formata o valor obtido da média, tendo como resultado: 5,760.00
select format(5760.000000,2);]

**2.5** Qual departamento possui a maior média salarial?
R:departamento de Tecnologias Avançadas com média salarial de R$21.815,00
[select
	departamentos.departamento_nome,
	departamentos.departamento_id,
    avg(funcionarios.salario) média_salarial
from departamentos
join funcionarios
on funcionarios.departamento_id = departamentos.departamento_id
group by departamentos.departamento_id
order by média_salarial desc
limit 1;]


**2.6** Qual departamento possui o menor número de funcionários?
R:os departamentos que possuem apenas um funcionário são administração, recursos humanos, relações púlicas, contabilidade e biotecnologia
[select
	count(funcionarios.departamento_id) funci_departa,
    departamentos.departamento_nome departamento
from departamentos
join funcionarios
on funcionarios.departamento_id = departamentos.departamento_id
group by departamentos.departamento_nome
having funci_departa = 1
order by funci_departa;]
---

## Nível 3: Recursos Humanos

O RH está fazendo uma análise demográfica da empresa.

**3.1** Quantos funcionários da empresa Momento possuem cônjuges?
R:-- total de funcionarios que tem conjuges
[select
    count(funcionarios.primeiro_nome) funcinarios_que_tem_conjuges
from dependentes
inner join funcionarios
on dependentes.funcionario_id = funcionarios.funcionario_id
where dependentes.relacionamento like "%conjuge%"
group by relacionamento;]
-- são eles:
[select
	concat(funcionarios.primeiro_nome, " ", funcionarios.sobrenome) nome_colaborador,
    dependentes.relacionamento tem
from dependentes
inner join funcionarios
on dependentes.funcionario_id = funcionarios.funcionario_id
where dependentes.relacionamento like "%conjuge%";]

**3.2** Quantos funcionários possuem filhos registrados?
R:[-- 32 funcionarios tem filhos
select
    count(funcionarios.primeiro_nome) funcinarios_que_tem_filho
from dependentes
inner join funcionarios
on dependentes.funcionario_id = funcionarios.funcionario_id
where dependentes.relacionamento like "%filha%";]
[	-- são eles
select
	concat(funcionarios.primeiro_nome, " ", funcionarios.sobrenome) nome_colaborador,
    dependentes.relacionamento tem
from dependentes
inner join funcionarios
on dependentes.funcionario_id = funcionarios.funcionario_id
where dependentes.relacionamento like "%filha%";]

**3.3** Qual funcionário foi contratado há mais tempo na empresa?
R:Steven Wayne do departamento Executivo, data de contratação: 17/06/1987.
[select
	funcionarios.funcionario_id,
	funcionarios.data_contratacao,
    concat(funcionarios.primeiro_nome, " ",funcionarios.sobrenome) nome_funcionario,
    departamentos.departamento_nome,
    escritorios.escritorio_nome,
    escritorios.endereco
from funcionarios
inner join departamentos
on funcionarios.departamento_id = departamentos.departamento_id
inner join escritorios
on departamentos.escritorio_id = escritorios.escritorio_id
order by data_contratacao 
limit 1;]

**3.4** Qual funcionário foi contratado há menos tempo na empresa?
R:Sabrina Portela Spellman do departamento de Finanças, data de contratação: 28/20/2024;
[select
	funcionarios.funcionario_id,
	funcionarios.data_contratacao,
    concat(funcionarios.primeiro_nome, " ",funcionarios.sobrenome) nome_funcionario,
    departamentos.departamento_nome,
    escritorios.escritorio_nome,
    escritorios.endereco
from funcionarios
inner join departamentos
on funcionarios.departamento_id = departamentos.departamento_id
inner join escritorios
on departamentos.escritorio_id = escritorios.escritorio_id
order by data_contratacao desc
limit 1;]

**3.5** Liste os 5 funcionários com mais tempo de casa, ordenados pela data de contratação.
R:Estes são:Steven Wayne, Jennifer Whalen, Neena Kochhar, Alexander Hunold e Bruce Ernst.
[select
	funcionarios.funcionario_id,
	funcionarios.data_contratacao,
    concat(funcionarios.primeiro_nome, " ",funcionarios.sobrenome) nome_funcionario,
    departamentos.departamento_nome,
    escritorios.escritorio_nome
from funcionarios
inner join departamentos
on funcionarios.departamento_id = departamentos.departamento_id
inner join escritorios
on departamentos.escritorio_id = escritorios.escritorio_id
having funcionarios.data_contratacao > '1980-01-01' and funcionarios.data_contratacao < '1991-12-31'
order by data_contratacao
limit 5;]

**3.6** Quantos funcionários foram contratados na década de 1990 (entre 1990-1999)?
R: 37 funcionarios ao todo
[select
	count(funcionarios.data_contratacao) total_funcionarios
from funcionarios
where funcionarios.data_contratacao > '1990-01-01' and funcionarios.data_contratacao < '1999-12-31';]

**3.7** Como a média salarial da Momento evoluiu ao longo dos anos? Agrupe por ano de contratação e calcule a média salarial.
R:[select 
	data_contratacao, 
    avg(salario) 
from funcionarios 
where data_contratacao > '1987-01-01' and data_contratacao < '2025-01-01'
group by data_contratacao;]

---

## Nível 4: Operações e Escritórios

**4.1** Liste todos os escritórios e seus respectivos países.

**4.2** Qual é o custo total de suprimentos em cada escritório? Ordene do mais caro ao mais barato.

**4.3** Qual escritório possui a maior quantidade de diferentes tipos de suprimentos?

**4.4** Qual é o suprimento mais caro (considerando preço unitário) em toda a empresa?

**4.5** Calcule o valor total do inventário de suprimentos da empresa (quantidade × preço unitário de todos os itens em todos os escritórios).

---

## Nível 5: Produtos e Vendas

**5.1** Quais produtos foram vendidos pela Momento? Liste todos os produtos únicos.

**5.2** Qual é o produto mais vendido (maior quantidade total)?

**5.3** Qual é o produto menos vendido?

**5.4** Pensando na relação quantidade × valor unitário, qual produto gerou mais receita para a empresa?

**5.5** Qual é o produto mais caro (maior preço unitário) no catálogo?

**5.6** Qual foi o faturamento total da empresa em vendas?

**5.7** Quantas vendas foram realizadas no mês de junho de 2023?

**5.8** Qual vendedor realizou mais vendas (em quantidade de transações)?

**5.9** Qual vendedor gerou mais receita para a empresa?

---

## Nível 6: Operações de Atualização

A empresa está crescendo e mudanças precisam acontecer!

**6.1** Um novo departamento foi criado: **Inovações**. Ele será alocado no escritório "Wayne Offices". Adicione-o ao banco de dados.

**6.2** O departamento de Inovações está sem funcionários. Transfira 2 funcionários do departamento de Tecnologia para Inovações.

**6.3** A empresa decidiu dar um aumento de 10% para todos os funcionários do departamento de Tecnologia. Atualize os salários.

**6.4** O funcionário "Bruce Ernst" foi promovido a "Senior Web Developer" e recebeu um aumento para $5.000. Atualize suas informações.

**6.5** Adicione um novo suprimento ao escritório "Wayne Offices": 15 unidades de "Headsets" com preço unitário de $150 cada.

**6.6** Todos os funcionários contratados antes de 1990 estão aposentando. Remova-os do banco de dados (CUIDADO: execute um find antes para ver quantos serão afetados!).

---

## Nível 7: Análise Avançada com Agregações

**7.1** Crie um relatório mostrando cada departamento com: nome do departamento, número de funcionários, salário total e média salarial.

**7.2** Liste os 3 cargos mais comuns na empresa e quantos funcionários ocupam cada cargo.

**7.3** Encontre todos os funcionários que ganham acima da média salarial de seus respectivos departamentos.

**7.4** Calcule a taxa de crescimento da empresa por ano (quantos funcionários foram contratados a cada ano).

**7.5** Crie um ranking dos vendedores, mostrando: nome do vendedor, número de vendas realizadas e receita total gerada.

**7.6** Encontre os produtos que foram vendidos por apenas um vendedor.

---

## Nível 8: Desafios e Otimização

**8.1** **Desafio de Performance**: Encontre todos os funcionários do departamento de Vendas que possuem dependentes (cônjuge OU filhos). Quantas formas diferentes você consegue resolver isso?

**8.2** **Desafio de Lógica**: Encontre funcionários que trabalham em escritórios diferentes do escritório associado ao seu departamento (se houver alguma inconsistência).

**8.3** **Consulta Complexa**: Crie um relatório completo de um escritório mostrando:
   - Nome do escritório
   - País
   - Número de departamentos naquele escritório
   - Número total de funcionários
   - Custo total com salários
   - Custo total com suprimentos

**8.4** **Desafio de Agregação**: Encontre o "departamento mais equilibrado" - aquele com a menor diferença entre o maior e menor salário.

**8.5** **Text Search**: Encontre todos os produtos que contêm a palavra "Uniforme" no nome.

**8.6** **Date Range**: Encontre todas as vendas que ocorreram no segundo trimestre de 2023 (abril-junho).

## Nível 9: Desafios Ninja

**9.1** **Múltiplas Soluções**: Encontre todos os funcionários cujo salário está entre $6.000 e $10.000. Resolva de 3 formas diferentes.

**9.2** **Data Quality**: Encontre todos os funcionários que NÃO possuem email ou telefone cadastrado.

**9.2** **Desafio de Lógica Avançada**: Crie uma query que encontre "funcionários solitários" - aqueles que são os únicos em seu cargo dentro de seu departamento.

**9.2* **Pipeline Complexo**: Crie um relatório que mostre, para cada país:
   - Nome do país
   - Número de escritórios
   - Número de departamentos
   - Número de funcionários
   - Receita total (soma dos salários + custo de suprimentos)

---

## Nível 10: Casos Práticos e Simulações

### Cenário 1: Expansão Internacional
A Momento quer abrir um novo escritório no Brasil.

**10.1** Crie o novo escritório "Momento Brasil" em São Paulo com suprimentos iniciais de sua escolha.

**10.2** Crie um novo departamento "Operações LATAM" vinculado a este escritório.

**10.3** Contrate 5 novos funcionários para este departamento (você e 4 colegas de turma).

### Cenário 2: Crise Financeira

A empresa precisa reduzir custos!

**10.4** Identifique os 3 escritórios com maiores custos de suprimentos.

**10.5** Sugira onde cortar custos: liste suprimentos com quantidade acima de 50 unidades.

**10.6** Calcule quanto a empresa economizaria se reduzisse 20% dos salários acima de $15.000 (apenas calcule, não execute).

### Cenário 3: Auditoria de Vendas

O departamento financeiro suspeita de irregularidades.

**10.7** Encontre vendas que não possuem vendedor associado.

**10.8** Verifique se há vendedores registrados nas vendas que não existem na coleção de funcionários.

**10.9** Identifique produtos que aparecem nas vendas mas com preços unitários muito diferentes entre transações (variação > 10%).

### Cenário 4: Relatório Executivo

O CEO pediu um dashboard completo.

**10.10** Crie UMA ÚNICA query de agregação que retorne:
   - Total de funcionários
   - Custo total com salários
   - Número de departamentos
   - Número de escritórios
   - Receita total de vendas
   - Produto mais vendido

---

## Dicas Gerais

- Sempre verifique os dados antes de executar operações de atualização ou remoção.
- Utilize índices para otimizar consultas frequentes.
- Documente suas queries para facilitar a revisão futura.
- Divida consultas complexas em etapas menores para facilitar o entendimento.
- Pratique a criação de backups antes de fazer alterações significativas no banco de dados.
- Explore a documentação oficial do MongoDB para aprofundar seu conhecimento sobre operadores e agregações.

--- 

