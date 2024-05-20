# Modelo de Banco de Dados (Projeto Disciplinar)

## Visão Geral

Este projeto cria um modelo de banco de dados, incluindo entidades essenciais como alunos, professores, cursos, departamentos, disciplinas e matrizes curriculares. O banco de dados permite a geração e consulta de diversos relatórios, como históricos escolares de alunos, histórico de disciplinas ministradas por professores, lista de formandos, chefes de departamento e grupos de TCC.

## Índice

- [Visão Geral](#visão-geral)
- [Diagrama Relacional](#diagrama-relacional)
- [Esquema do Banco de Dados](#esquema-do-banco-de-dados)
- [Funcionalidades](#funcionalidades)
- [Configuração](#configuração)
- [Uso](#uso)
- [Integrantes](#integrantes)

## Esquema do Banco de Dados

O banco de dados inclui as seguintes tabelas:

- `alunos`: Informações sobre os alunos
- `professores`: Informações sobre os professores
- `cursos`: Informações sobre os cursos
- `departamentos`: Informações sobre os departamentos
- `chefe de departamento`: Informações sobre os chefes de departamentos
- `disciplinas`: Informações sobre as disciplinas
- `historico_escolar`: Históricos escolares dos alunos
- `historico_disciplina_professores`: Histórico de disciplinas ministradas pelos professores
- `alunos_formados`: Alunos formados, aprovados em uma matriz curricular (40 disciplinas)
- `grupo_tcc`: Grupos de TCC e seus orientadores


## Diagrama Relacional


## Funcionalidades

- Inserir alunos, professores, cursos, departamentos e disciplinas no banco de dados.
- Gerar históricos escolares dos alunos.
- Acompanhar as disciplinas ministradas pelos professores.
- Listar alunos que se formaram em um determinado semestre e ano.
- Identificar chefes de departamento.
- Acompanhar grupos de TCC e seus orientadores.

## Configuração

Este projeto utiliza o CockroachDB Cloud como banco de dados, a biblioteca Faker para geração de dados falsos e psycopg2 para inserção desses dados no banco de dados. Além disso, recomenda-se o uso das extensões SQLTools e SQLTools PostgreSQL/Cockroach Driver no VSCode para facilitar a visualização e gerenciamento do banco de dados (extensões utilizadas na criação do projeto).

- [SQLTools](https://marketplace.visualstudio.com/items?itemName=mtxr.sqltools)
- [SQLTools PostgreSQL/Cockroach Driver](https://marketplace.visualstudio.com/items?itemName=mtxr.sqltools-driver-pg)


## Uso

1. **Crie as tabelas:**
   Execute o arquivo `criacao_tabelas.py` após realizar a conexão com o banco de dados e definir o `DATABASE_URL`.

2. **Gere os dados aleatórios:**
   Execute o arquivo `DataGeneratos.py` para popular o banco de dados com dados fictícios.

3. **Consultas requeridas do projeto:**

### Consulta 1

```sql
SELECT
    he.id AS codigo_historico,
    he.aluno_id AS codigo_aluno,
    d.id AS codigo_disciplina,
    d.nome AS nome_disciplina,
    he.semestre,
    he.ano,
    he.nota_final
FROM
    historico_escolar he
JOIN
    disciplinas d ON he.disciplina_id = d.id
WHERE
    he.aluno_id = (
        SELECT
            aluno_id
        FROM
            historico_escolar
        ORDER BY
            RANDOM()
        LIMIT 1
    );
```


### Consulta 2

```sql
SELECT
    p.nome AS nome_professor,
    d.nome AS nome_disciplina,
    hdp.semestre,
    hdp.ano
FROM
    historico_disciplina_professores hdp
JOIN
    professores p ON hdp.professor_id = p.id
JOIN
    disciplinas d ON hdp.disciplina_id = d.id;
```

### Consulta 3

```sql
SELECT alunos.nome AS "Alunos Formados", alunos_formados.semestre, alunos_formados.ano
FROM alunos_formados
JOIN alunos ON alunos.id = alunos_formados.aluno_id;
```

### Consulta 4

```sql
SELECT p.nome AS nome_professor, d.nome AS nome_departamento
FROM professores p
JOIN professores_departamentos pd ON p.id = pd.professor_id
JOIN departamentos d ON pd.departamento_id = d.id;
```


### Consulta 5

```sql
SELECT
    gt.id AS id_grupo_tcc,
    gt.grupo AS numero_grupo,
    a.nome AS nome_aluno,
    p.nome AS nome_professor_orientador
FROM
    grupo_tcc gt
JOIN
    alunos a ON gt.aluno_id = a.id
JOIN
    professores p ON gt.professor_orientador_id = p.id;
```

## Integrantes

- Guilherme de Abreu (Matrícula: 22.222.028-7)
- Matheus Cucio (Matrícula: 22.121.035-4)