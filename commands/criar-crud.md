Gere um CRUD completo em Java para a entidade: $ARGUMENTS

Use o padrão DAO (Data Access Object) do FIAP Fase 6. Inclua:

## Estrutura a Gerar

### 1. Model (`$ARGUMENTS.java`)
- Atributos privados com tipos adequados
- Construtor completo e construtor vazio
- Getters e Setters
- `toString()` para debug

### 2. DAO Interface (`I$ARGUMENTSDao.java`)
- `create($ARGUMENTS obj)`
- `findById(int id)`
- `findAll()`
- `update($ARGUMENTS obj)`
- `delete(int id)`

### 3. DAO Implementation (`$ARGUMENTSDaoImpl.java`)
- Implementa a interface DAO
- Usa JDBC com `PreparedStatement` (nunca concatenar SQL)
- Trata `SQLException` adequadamente
- Fecha conexões no bloco `finally` ou usa try-with-resources

### 4. SQL (`create_table_$ARGUMENTS.sql`)
- CREATE TABLE com tipos Oracle SQL
- PRIMARY KEY com SEQUENCE + TRIGGER (padrão Oracle)
- Constraints relevantes (NOT NULL, VARCHAR2)

### Padrão de Conexão
Use `ConnectionFactory.getConnection()` — padrão FIAP.

Gere código funcional e compilável. Nomes em português se a entidade for em português.
