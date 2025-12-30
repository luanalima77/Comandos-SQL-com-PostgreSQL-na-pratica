# 📚 Sistema de gestão de alunos e cursos (Supabase)

Este projeto é um exemplo de banco de dados para gerenciamento de alunos, cursos e matrículas usando Supabase (PostgreSQL na nuvem). Ele inclui a criação de tabelas, inserção de dados e consultas básicas.

## 🗂 Estrutura do banco de dados

### Tabelas

1. **alunos**  
   - id: identificador único do aluno (PRIMARY KEY);  
   - nome: nome completo do aluno;  
   - turma: turma do aluno;  
   - curso: curso que o aluno está matriculado;  
   - data_nascimento: data de nascimento;  
   - nacionalidade: nacionalidade do aluno.  

2. **cursos**  
   - id: identificador único do curso (PRIMARY KEY);  
   - nome: nome do curso;  
   - duracao_anos: duração do curso em anos; 
   - tipo_curso: tipo de curso (ex: bacharelado).  

3. **matriculas**  
   - id: identificador único da matrícula (PRIMARY KEY);  
   - aluno_id: referência ao aluno (alunos.id), ou seja, chave estrangeira; 
   - curso_id: referência ao curso (cursos.id), ou seja, chave estrangeira;  
   - data_matricula: data da matrícula (padrão: data atual).  

## 🚀 Como usar este projeto com Supabase
Supabase é uma plataforma que oferece PostgreSQL na nuvem, com editor SQL, tabelas e APIs prontas para uso. Para testar este projeto, siga os passos abaixo, respectivamente:

### 1. Crie uma conta no Supabase
- Acesse [https://supabase.com/](https://supabase.com/) e clique em Sign Up para criar sua conta gratuita;
- Após o cadastro, faça login na sua conta.

### 2. Crie um novo projeto
- Clique em New Project;
- Preencha os campos obrigatórios:
  - Nome do projeto: escolha um nome para seu banco de dados;
  - Senha do banco de dados: defina uma senha segura **(memorize ou anote essa senha)**;
  - Região: escolha a região mais próxima de você;
- Clique em Create new project;
- Aguarde alguns minutos até que o projeto seja criado.

### 3. Acesse o SQL Editor
- No painel do projeto, clique em SQL Editor;
- Aqui você pode digitar e executar comandos SQL diretamente no banco de dados Supabase;
- **Importante**: execute primeiro os scripts de criação de tabelas, depois os scripts de inserção de dados.

### 4. Visualize os dados
- Clique na aba Table Editor;
- Você verá todas as tabelas criadas (alunos, cursos, matriculas) e os dados inseridos;
- É possível adicionar, editar ou remover registros diretamente nesta interface.

### 5. Consultas SQL
- No SQL Editor, você pode executar múltiplas consultas, com JOINs, por exemplo.
<br><br>

## 📝 Scripts SQL

### Script para criação das tabelas

```sql
CREATE TABLE alunos (
  id SERIAL PRIMARY KEY,
  nome TEXT NOT NULL,
  turma TEXT NOT NULL,
  curso TEXT NOT NULL,
  data_nascimento DATE,
  nacionalidade TEXT NOT NULL
);

CREATE TABLE cursos (
  id SERIAL PRIMARY KEY,
  nome TEXT NOT NULL,
  duracao_anos INT,
  tipo_curso TEXT NOT NULL
);

CREATE TABLE matriculas (
  id SERIAL PRIMARY KEY,
  aluno_id INT REFERENCES alunos(id) ON DELETE CASCADE,
  curso_id INT REFERENCES cursos(id) ON DELETE CASCADE,
  data_matricula DATE DEFAULT CURRENT_DATE
);
```

### Scripts para inserção de dados nas tabelas alunos e cursos
#### Tabela alunos
Para inserir alunos na tabela alunos, utilize a seguinte estrutura:

```sql
INSERT INTO alunos (nome, turma, curso, data_nascimento, nacionalidade)
VALUES 
('Nome do Aluno', 'Turma', 'Curso', 'AAAA-MM-DD', 'Nacionalidade'),
('Outro Aluno', 'Turma', 'Curso', 'AAAA-MM-DD', 'Nacionalidade');
```

Exemplo genérico de inserção de dados na tabela alunos:
```sql
INSERT INTO alunos (nome, turma, curso, data_nascimento, nacionalidade)
VALUES 
('João Silva', '1A', 'Engenharia da Computação', '2000-01-01', 'Brasileiro'),
('Maria Souza', '1B', 'Ciência da Computação', '2001-02-02', 'Brasileira');
```

#### Tabela cursos

Para inserir registros na tabela `cursos`:

```sql
INSERT INTO cursos (nome, duracao_anos, tipo_curso)
VALUES 
('Nome do Curso', DuraçãoEmAnos, 'Tipo do Curso'),
('Outro Curso', DuraçãoEmAnos, 'Tipo do Curso');
```

Exemplo genérico de inserção de dados na tabela cursos:
```sql
INSERT INTO cursos (nome, duracao_anos, tipo_curso)
VALUES 
('Engenharia da Computação', 4, 'Bacharelado'),
('Ciência da Computação', 4, 'Bacharelado');
```

### Script para criação e consulta de matrículas
Para inserir registros na tabela matriculas:

```sql
INSERT INTO matriculas (aluno_id, curso_id, data_matricula)
VALUES 
(ID_Aluno, ID_Curso, 'AAAA-MM-DD'),
(ID_Aluno, ID_Curso, 'AAAA-MM-DD');
```

Para visualizar as matrículas com os nomes dos alunos e cursos:

```sql
SELECT a.nome AS aluno, c.nome AS curso, m.data_matricula
FROM matriculas m
JOIN alunos a ON m.aluno_id = a.id
JOIN cursos c ON m.curso_id = c.id;
```

## 💡 Observações e dicas

### 1. Ordem de execução dos scripts
É importante seguir a sequência correta ao executar os scripts SQL:

1. **Criação das tabelas**  
   - Sempre comece criando as tabelas (alunos, cursos, matriculas) antes de inserir qualquer dado;  
   - Isso garante que todas as referências e restrições de chave estrangeira estejam disponíveis.

2. **Inserção de dados**  
   - Após criar as tabelas, insira os registros nas tabelas alunos e cursos primeiro;  
   - Somente depois insira os registros na tabela matriculas, já que ela depende dos IDs de alunos e cursos.

3. **Consultas**  
   - Execute consultas somente após a inserção dos dados para garantir que haja informações para retornar.

Siga essa ordem para evitar erros de integridade, como tentar adicionar uma matrícula para um aluno ou curso que ainda não existe.

### 2. Integridade referencial no Supabase
- Chaves primárias (PRIMARY KEY): cada tabela possui um identificador único (id) para garantir que cada registro seja distinto;  
- Chaves estrangeiras (FOREIGN KEY): a tabela matriculas referencia alunos.id e cursos.id. Isso evita que você insira uma matrícula com um aluno ou curso inexistente;  
- ON DELETE CASCADE: se um aluno ou curso for removido, todas as matrículas relacionadas são automaticamente excluídas, mantendo a consistência do banco.


## Conclusão

Este projeto fornece uma base sólida para entender bancos de dados relacionais usando PostgreSQL no Supabase, com tabelas de alunos, cursos e matrículas.  

Com ele, você pode:
- Criar e gerenciar tabelas com integridade referencial; 
- Inserir, consultar e atualizar dados de forma segura;  
- Praticar JOINs e consultas complexas;   
💡 Dica final: experimente criar novas tabelas, relacionamentos e consultas. Isso ajuda a consolidar o aprendizado e permite construir sistemas cada vez mais completos. 

## ✍️ Autora
**Luana de Jesus Lima**  
[Meu LinkedIn](https://www.linkedin.com/in/luana-de-jesus-lima)  

### ✍️ Projeto com fins educacionais