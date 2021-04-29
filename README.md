
## TRABALHANDO COM HIVE 🐝

Esse repositório tem como objetivo demonstrar uma prática com o framework Hive em conjunto com HDFS. Lembrando que essa prática foi utilizando o docker!

---

📢  ETAPA 1: DESLOCANDO O ARQUIVO LOCAL PARA O HDFS
 
Antes de ir manipular qualquer tipo de tabela vamos acessar e utilizar o beeline, que é um cliente Hive que está incluindo nos principais clusters. 
![Print terminal: Utilizando Hive com HDFS](https://github.com/gacarvalho/practice-hive-hdfs/blob/main/Image/2.%20desafio_1.png?raw=true)

  > Questão: Enviar o arquivo local “/input/exercises-data/populacaoLA/populacaoLA.csv” para o diretório no HDFS “/user/aluno/<nome>/data/populacao”

Antes de enviar o arquivo local para o diretório HDFS é necessário criar uma pasta para arquivo, e para isso vamos aplicar o seguinte comando ```root@namenode:/# hdfs dfs -mkdir /user/aluno/gabriel/data/populacao```

Agora vamos deslocar o arquivo com o comando ```root@namenode:/# hdfs dfs -put /input/exercises-data/populacaoLA/populacaoLA.csv /user/aluno/gabriel/data/populacao```
![Print terminal: Utilizando Hive com HDFS](https://github.com/gacarvalho/practice-hive-hdfs/blob/main/Image/3.%20desafio_1.png?raw=true)

📢  ETAPA 2: CRIAÇÃO DE UMA TABELA INTERNA

  > Questão: Criar a Tabela Hive no BD empresastartup - TABELA INTERNA: pop com os campos:
  
- [x] zip_code - int
- [x] total_population - int
- [x] median_age - float
- [x] total_males - int
- [x] total_females - int
- [x] total_households - int
- [x] average_household_size - float

E as propriedades:

- [x] Delimitadores: Campo ‘,’ | Linha ‘\n’
- [x] Sem Partição
- [x] Tipo do arquivo: Texto
- [x] tblproperties("skip.header.line.count"="1")

Agora vamos criar uma tabela interna utilizando o hive com uma consulta SQL ```create table empresastartup.pop(zip_code int, total_population inr, median_age float, total_males int, total_females int, total_households int, average_household_size float) row format delimited fields terminated by ',' lines terminated by '\n' stored as textfile tblproperties("skip.header.line.count"="1");```
![Print terminal: Utilizando Hive com HDFS](https://github.com/gacarvalho/practice-hive-hdfs/blob/main/Image/4.%20desafio_1.png?raw=true)

Agora podemos consultar essa tabela nas imagens abaixo, podendo observar a sua estrutura comos seus tipos de dados! Na segunda imagem aplicando o comando ```desc formatted empresastartup.pop;``` é possível observar o caminho que foi gravado.
![Print terminal: Utilizando Hive com HDFS](https://github.com/gacarvalho/practice-hive-hdfs/blob/main/Image/7.desafio_2.png?raw=true)
![Print terminal: Utilizando Hive com HDFS](https://github.com/gacarvalho/practice-hive-hdfs/blob/main/Image/6.%20desafio_1.png?raw=true)

📢  ETAPA 3: INSERÇÃO DE DADOS NA TABELA
 
  > Questão: Selecionar os 10 primeiros registros da tabela pop
  
Antes de mais nada vamos listar os primeiros 10 registros da tabela pop! Como é possível observar não foi encontrado nenhum registro.
![Print terminal: Utilizando Hive com HDFS](https://github.com/gacarvalho/practice-hive-hdfs/blob/main/Image/8.desafio_2.png?raw=true)

  > Carregar o arquivo do HDFS “/user/aluno/<nome>/data/população/populacaoLA.csv” para a tabela Hive pop
  
O arquivo já está no HDFS, uma questão interessante é mapear o diretório /user/aluno/gabriel/data/populacao/. Por que? Porque o nosso arquivo pode estar em vários "pedaços". Mas neste caso vamos mapear o arquivo de forma direta.
![Print terminal: Utilizando Hive com HDFS](https://github.com/gacarvalho/practice-hive-hdfs/blob/main/Image/9.desafio_2.png?raw=true)

  > Questão: Contar a quantidade de registros da tabela pop
  
Pronto, agora aplicando uma consulta SQL ```select * from pop limit 10;``` é possível observar que agora a nossa tabela pop tem registro!

Realizando a última questão podemos contar a quantidade de registros da tabela pop com a seguinte consulta ```select count (*) from pop;```
![Print terminal: Utilizando Hive com HDFS](https://github.com/gacarvalho/practice-hive-hdfs/blob/main/Image/10.desafio_2.png?raw=true)

📢  ETAPA 4: CRIAÇÃO DE UMA TABELA EXTERNA E TRABALHANDO COM SELEÇÃO DE TABELAS
  
Para trabalhar com a seleção de tabelas, vamos criar uma pasta no namenode e posteriormente realizar uma consulta do diretório ```$ docker exec -it namenode hdfs dfs -mkdir /user/aluno/gabriel/data/nascimento```. Na figura abaixo é possível observar os diretórios já criados!
![Print terminal: Utilizando Hive com HDFS](https://github.com/gacarvalho/practice-hive-hdfs/blob/main/Image/11.desafio_3.png?raw=true)

E agora vamos verificar se a base de dados e tabela está presente! Verificar se a base de dados as tabelas estão presentes é processo importante quando trabalhamos com dados.
 
Quando trabalhamos com hive é possível criar dois tipos de tabelas: Tabelas Internas e Tabelas Externas. Quando trabalhamos com Tabelas Externas, o Hive não move os dados para seu diretório de armazém. Se a tabela externa for descartada, os metadados da tabela serão excluídos, mas não os dados. Agora, se trabalhamos com tabelas internas, o Hive move os dados para seu diretório de armazém. Se a tabela for eliminada, os metadados da tabela e os dados serão excluídos.Para criar a tabela externa nascimento foi necessario aplicar o seguinte comando ```create external table nascimento(nome string, sexo string, frequencia int) partitioned by (ano int) row format delimited fields terminared by ',' lines terminated by '\n' stored as textfile location '/user/aluno/gabriel/data/nascimento';```
![Print terminal: Utilizando Hive com HDFS](https://github.com/gacarvalho/practice-hive-hdfs/blob/main/Image/13.desafio_3.png?raw=true)

Após a criação de uma tabela externa, vamos adicionar a partição a tabela nascimento do ano de 2015. Na segunda imagem é possível observar que temos um arquivo para cada ano, por exemplo: yob1999. 
![Print terminal: Utilizando Hive com HDFS](https://github.com/gacarvalho/practice-hive-hdfs/blob/main/Image/14.desafio_3.png?raw=true)
![Print terminal: Utilizando Hive com HDFS](https://github.com/gacarvalho/practice-hive-hdfs/blob/main/Image/15.desafio_3.png?raw=true)

Observando esse ponto vamos mover da nossa pasta para a nossa partição utilizando o comando ```hdfs dfs -put /input/exercises-data/names/yob2015.txt /user/aluno/gabriel/nascimento/ano=2015```
![Print terminal: Utilizando Hive com HDFS](https://github.com/gacarvalho/practice-hive-hdfs/blob/main/Image/16.desafio_3.png?raw=true)
![Print terminal: Utilizando Hive com HDFS](https://github.com/gacarvalho/practice-hive-hdfs/blob/main/Image/18.desafio_3.png?raw=true)
 
E agora vamos verificar se a nossa partição tem registros com a consulta ```select * from nascimento limit 20```
![Print terminal: Utilizando Hive com HDFS](https://github.com/gacarvalho/practice-hive-hdfs/blob/main/Image/17.desafio_3.png?raw=true)

Pronto. Agorape possível verificar as partições dentro do diretório nascimento com os anos 2015, 2016 e 2017.
![Print terminal: Utilizando Hive com HDFS](https://github.com/gacarvalho/practice-hive-hdfs/blob/main/Image/19.desafio_3.png?raw=true)

  > Questão: Selecionar os 5 primeiros registros da tabela nascimento pelo ano de 2016;
  
Utilizando a consulta ```select * from nascimento where ano=2015 limit 5``` é possível observar os 5 primeiros registros do ano de 2015.
![Print terminal: Utilizando Hive com HDFS](https://github.com/gacarvalho/practice-hive-hdfs/blob/main/Image/20.desafio_3.png?raw=true)

  > Questão: Contar a quantidade de nomes de crianças nascidas em 2017 e contar a quantidade de crianças nascidas em 2017
 
Agora vamos contar a quantidade de nomes com a consulta ```select count(nome) as qtd from nascimento where ano=2017;``` e o sistema irá retornar a quantidade de nomes de crianças do ano de 2017! Vale lembrar que quando aplicamos consultas sobre uma base de dados ou partição o Hive aplicar um MapReduce. 
 
Depois de saber a quantidade de nomes no ano de 2017 vamos contar a quantidade de crianças nascidas em 2017 com o comando ```select sum(frequencia) as qtd from nascimento where ano=2017;```
![Print terminal: Utilizando Hive com HDFS](https://github.com/gacarvalho/practice-hive-hdfs/blob/main/Image/21.desafio_4.png?raw=true) 
 
  > Questão: Contar a quantidade de crianças nascidas por sexo no ano de 2015 e mostrar por ordem de ano decrescente a quantidade de crianças nascidas por sexo
  
Para realizar esse processo vamos contar a quantidade de crianças nascidas por sexo no ano de 2015 com a consulta ```select sexo, sum(frequencia) as qtd from nascimento where ano=2015 group by sexo;``` e assim o sistema irá retornar a quantidade de crianças que nasceram em 2015 filtrado por sexo.

Agora vamos mostrar por ordem de ano descrecente a quantidade de crianças nascidas por sexo, e para isso vamos consultar com o comando ```select ano, sexo, sum(frequencia) as qtd from nascimento group by ano, sexo order by ano desc;```
![Print terminal: Utilizando Hive com HDFS](https://github.com/gacarvalho/practice-hive-hdfs/blob/main/Image/22.desafio_4.png?raw=true) 

  > Questão: Mostrar por ordem de ano decrescente a quantidade de crianças nascidas por sexo com o nome iniciado com ‘A’ e qual nome e quantidade das 5 crianças mais nascidas em 2016
  
Para resolver a primeira questão vamos mostrar por ordem de ano decrecente a quantidade de crianças por sexo com o nome iniciado com 'A', e para isso vamos aplicar a seguinte consulta ```select ano, sexo, sum(frequencia) as qtd from nascimento where nome like 'A%' group by ano, sexo order by ano desc;``` e só assim o sistema irá retornar a quantidade de crianças com a letra 'A' nascidas de acordo com cada ano. 
  
Na segunda questão vamos verificar qual o nome e quantidade das 5 crianças mais nascidas em 2016 com o comando ```select nome, max (frequencia) as qtd from nascimento where ano=2016 group by nome order by qtd desc limit 5;```
![Print terminal: Utilizando Hive com HDFS](https://github.com/gacarvalho/practice-hive-hdfs/blob/main/Image/23.desafio_4.png?raw=true) 

  > Questão: Qual nome e quantidade das 5 crianças mais nascidas em 2016 do sexo masculino e feminino

Agora para resolver a últimas questão queremos saber qual o nome e quantidade das 5 crianças mais nascidas em 2016 do sexo masculino e feminino, e para isso vamos utilizar a seguinte consulta ```select nome, max(frequencia) as qtd, sexo from nascimento where ano=2016 group by nome, sexo order by qtd desc limit 5;```
![Print terminal: Utilizando Hive com HDFS](https://github.com/gacarvalho/practice-hive-hdfs/blob/main/Image/24.desafio_4.png?raw=true) 
