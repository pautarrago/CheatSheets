*Base de Dades: Part pràctica*

**Tema 03: Àlgebra relacional**

- Reanomenament:
    R = R{a -> a'}

- Unió:
    R = T $\cup$ S

Cal que T i S siguin relacions compatibles.

- Inetsercció:
    R = T $\cap$ S

Cal que T i S siguin relacions compatibles.

- Diferència:
    R = T - S

Cal que T i S siguin relacions compatibles.

- Producte Cartesià:
    R = TxS

- Selecció:
    R = R(cond{a})

- Projecció:
    R = R[a]

- Join:
    
    - Equi-join:
        R = T[cond{t,s}]S
    
    - Natural join:
        R = T[t*s]S
    
    - Natural join implícit:
        R = T*S

**Tema 03: SQL**

```SQL
create table <nom_taula>
    (<nom_col> <domini> [restr_col] [val_defecte])
    [, ...]
    [, restr_taula];    
```

- restr_col:
    - unique
    - primary key
    - references nom_taula [nom_col]
    - check (cond{})
    - not null

- restr_taula:
    - unique (nom_cols)
    - primary key (nom_cols)
    - foreign key (nom_cols) references nom_taula [nom_cols]
    - check (cond{})

```SQL
insert into <nom_taula> [<nom_cols>]
(values {v, ...}) | <query>;
```

```SQL
delete from <nom_taula>
where cond{};
```

```SQL
update <nom_taula>
set <nom_col> = {v} [, ...]
where cond{};
```

```SQL
select [distinct | all] <nom_cols> | *
from <nom_taules>
[where cond{}];
```

Per defecte all.

- operadors cond{}:
    - *, +, -, /
    - =, <, >, <=, >=, <>
    - not, and, or
    - nom_col between c1 and c2
    - nom_col in (c1, c2 [,..., vN])
    - nom_col like c
    - nom_col is [not] null

- joins:
    - taula1 t1, taula2 t2
    - taula1 t1 inner join taula2 t2 on cond{}
    - taula t1 natural inner join taula2 t2

```SQL
select [distinct | all] <nom_cols> | *
from <nom_taules>
[where cond{}]
order by nom_col [desc | asc];
```
Per defecte asc.

```SQL
select [distinct | all] <func_agreg> | *
from <nom_taules>
[where cond{}];
```
- func_agreg:
    - count()
    - sum()
    - min()
    - max()
    - avg()

```SQL
select [distinct | all] <nom_cols> | *
from <nom_taules>
[where cond{}]
group by <nom_cols>
[having cond{}];
```

```SQL
select [distinct | all] <nom_cols> | *
from <nom_taules>
[where cond{}]

union

select [distinct | all] <nom_cols> | *
from <nom_taules>
[where cond{}];
```
**Tema 04: Components lògics**

- Esquemes:

```SQL
create schema [[<nom_cat>] <nom_esq>]
[authorization <id_usuari>]
[<llist_els>];
```

```SQL
drop schema <nom_esq> [restrict | cascade];
```

- Connexions:

```SQL
connect to <nom_serv> [as nom_connex]
[user <id_usuari>];
set schema <nom_esq>;
```

```SQL
disconnect <nom_connex> | default | current | all;
```

- Sessions:

```SQL
set session characteristics as <mode_transac> [,...];
```

- Transaccions:

```SQL
set transaction <mode_transac> [, ...];
```
    - mode_transac:
        - read only | read write
        - read committed | repeatable read | serializable (*)

```SQL
start transaction <mode_transac> [, ...];
```

```SQL
commit [work] [and [no] chain];
```

```SQL
rollback [work] [and [no] chain];
```

- Dominis:

```SQL
create domain <nom_domini> as <domini>
[default literal | temps | user | null]
[constraint <nom_restr> check (cond{}) [, ...]];
```

- Assercions:

```SQL
create assertion <nom_assert> check (cond{});
```

- Vistes:

```SQL
create view <nom_vista> [(<nom_cols> [, ...])]
as <query>
[with check option];
```

Només són actualitzables les vistes amb el query:
    - select sobre una sola relació, sense agregats ni distinct.
    - els atributs del select han d'incloure tots els atributs amb restriccions not null que no tinguin valor per defecte.
    - sense group by.

- Priveligis:

```SQL
grant <nom_privilegis> on <nom_objs> to <id_usuaris>
[with grant option];
```

- nom_privilegis:
    - select [nom_atribut [, ...]]
    - insert [nom_atribut [, ...]]
    - update [nom_atribut [, ...]]
    - delete
    - references
    - usage
    - trigger
    - execute
    - under
    - all

```SQL
revoke [grant option for] <nom_privilegis> on <nom_objs> from <id_usuaris>
{cascade | restrict};
```

```SQL
create role <nom_rol>
<sentencies_grant>;
```
**Tema 04: Procediments emmagatzemats**

```SQL
create function <nom_func>(<variables>)
returns [set of] <nom_tipus> as $$
[declare <declaració_variables>]
begin
    ...;
    return [next] <variable>;
[excpetion <gestió_excpecions>]
end;
$$language plpgsql;
```
- declaració_variables:
    - declaració tipus previ:
```SQL
create type <nom_tipus> as
(<nom_domini> <domini> [, ...]);
```
    - dins el procediment:
```SQL
declare
    <nom_variable> [constant] <nom_tipus> [not null] [{default | :=} <expressió>]
    [, ...];
```
    - carregar la variable:
```SQL
... into <nom_variable>
```
    ó bé
```SQL
... := <nom_variable>
```
- sentencies condicionals:
```SQL
if cond{} then ...
[elseif cond{} then ... [, ...]]
[else ...]
end if;
```
- condició found:
    retorna true si el select previ retorna una fila o més, false si no en troba cap.

- sentencies iteratives:
    - for:
```SQL
for <target> in [reverse] <query>
loop ...
end loop;
```
        ó bé si coneixem el nombre d'iteracions
```SQL
for <nom_variable> in [reverse] <expressió1> .. <expressió2>
loop ...
end loop;
```
    - while:
```SQL
while <expressió>
loop ...
end loop;
```
    - loop:
```SQL
loop ...
exit [when <expressió>]; ...;
end loop;
```
- gestió_excepcions:
```SQL
exception
    when <nom_error> | cond{} then <handler_statements>
    [, ...]
```
    - dins del procediment:
```SQL
if cond{} then raise exception 'missatge', <nom_error>
end if;
```
    - capturar errors predefinits:
```SQL
when raise_exception then
raise excpetion '%', sqlerrm;
```

```SQL
drop function <nom_func>(<variables>);
```

**Tema 04: Disparadors**

```SQL
create trigger <nom_trigger>
before | after ... on <nom_taula> [, ...]
[for [each] row | statement]
execute procedure <nom_func>;
```
- procediments específics per a disparadors:
```SQL
create function <nom_func>(<variables>)
returns trigger as $$
[declare <declaració_variables>]
begin
    ...;
    return [next] <variable_trigger>;
[excpetion <gestió_excpecions>]
end;
$$language plpgsql;
```

- variables implícites dels triggers:
    - TG_OP = {INSERT | DELETE | UPDATE}
    - per un for each row:

| | INSERT | UPDATE | DELETE |
|:------- |:-------:|:-------:|:-------:|
|OLD| No definit| Valors de la tupla abans de l'update| Valors de la tupla abans del delete|
|NEW| Valors tuples després de l'insert| Valors de la tupla després de l'update| No definit|
    - per un for each statement OLD i NEW tenen valor nul.
    
- return del procediment:
    - per un before for each row:
        - return new: per INSERT i UPDATE s'executa el canvi establit, pel DELETE no fa res.
        - return old: per DELETE i UPDATE no s'executa el canvi establit, pel INSERT no fa res.
        - return null: no s'executa la sentència.
    - per la resta return null, ja que el valor de retorn és ignorat.

- instead of, trigger per actualització de vistes:
```SQL
create trigger <nom_trigger>
instead of {INSERT | UPDATE | DELETE} on <nom_vista>
[for [each] row | statement]
execute procedure <nom_func>;
```

**Tema 06: JDBC**
- statement:
cada cop que s'executa es compila.
```java
Statement s = createStatement();

String sql_query = "...";
Resultset r = s.executeQuery(sql_query);
```

- prepared statement:
es compila un cop i s'executa els cops que vulguis.
```java
String sql_query = "...";
PreparedStatement p = prepareStatement(sql_query);

Resultset r = p.executeQuery();
p.close();
```
    - modificació del preparedStatement:
posarem ? on en sql_query on volem la variable.
```java
p.setString(num_variable, "...");
```

- tractament resultSets:
```java
XXX getXXX(num_columna); //retorna l'element de la tupla en la columna num_columna
```

```java
bool r.next(); //avança el cursor a la tupla següent, retornant un boolea de s'hi n'hi ha
```

```java
bool r.wasNull();
```

```java
r.close();
```

- tancament connexió:
c és la connexió establerta en un inici
```java
c.commit(); 
```
ó bé
```java
c.rollback();
```

```java
c.close();
```

- excepcions:
```java
try{
    ...
}
catch(SQLException se){...}
```

```java
se.getSQLState(); //retorna l'id de l'error
```

```java
se.getMessage(); //retorna un string amb descripció breu de l'error
```