# 1. Nível II (N2)

## 2. Usando EH Cache
#### a. Ela é a Framework Caching que é mais rápida e leve. E é mais usada em Aplicacões do JAVA EE que usa o Hibernate.
#### b. Nesse caso, EH Cache é o provedor (provider) Nível 2 do hibernate.
#### c. É este provedor que armazenará o Cache no nível SessioFactory.
#### d. Ele suporta tanto o Caching em Memória RAM quanto em Disco ( dispõe ou remove em série no disco ).
#### e. Ele é bem poderoso porque nos permite configurar o "Timeout" para um objeto em particular no Cache. Ou seja, o ciclo de vida total de um objeto no Cache e etc. usando XML.
#### f. EH Cache cria arquivos XML.
#### g. Passos para usá-lo:
##### i. Adicionar a dependência no Maven. Colocando no pom.xml que trará os arquivos jar.
##### ii. Habilitar o Caching para a Aplicação no application.properties.
##### iii. Criar um ehcache.xml. É onde será especificado o timeout, o local para armazenar os objetos e etc, ou seja, o local temporário, onde o objeto deveria guardar os Caches. E ele deverá ser marcado ou anotado nas entidades.
##### iv. Test Caching: tanto para o Nível 1 quanto para o Nível 2: utilizar a anotação @Cache na entidade Pai.
### 3. Cache Concurrency Strategy
#### READ_ONLY
##### a. Só deve ser escolhido somente quando as entidades na Aplicacao nunca mudarem.
##### b. E se grande parte da Aplicacao só busca os dados no Database e os expõe ao usuário final.
##### c. Se o dado for lido e estiver pronto e você fazer uma atualização de gravação no database, isso vai gerar uma exceção.
##### d. Usar somente, então, quando apenas houver leitura de dados que são mostrados ao usuário final mas que nunca sofrerão atualizações (updates).
#### NONSTRICT_READ_WRITE
##### a. O Cache só será atualizado (updated) somente quando uma transação em particular inserir completamente o dado no Database.
##### b. Então, se nesse meio tempo, em relação ao processo acima, uma outra transação ocorrer e tentar ler esse dado, ele ficará obsoleto porque a outra transação não fez ainda a inserção no Database, e por isso esse não será atualizado. O dado que constará, será o antigo.
##### c. Ele é inconsistente.
#### READ_WRITE
##### a. Ele é consistente.
##### b. Enquanto uma transação está sendo atualizada, o dado, que também está sendo armazenado em Cache, e a transação está atualizando uma gravação em particular no Database, um "Lock" será associado com esse Cache do Objeto dentro do Cache.
##### c. E se outra transação aparecer e estiver tentando ler o dado e ver este "lock", então ela saberá que uma outra transação está tentando atualizar este objeto em particular. E não lerá o Cache e irá diretamente ao Database e pegará o último dado.
#### TRANSACTIONAL
##### a. Deve ser usado apenas quando estamos tratando com transações XA OU transações distribuídas. OU seja, mais de um Database.
##### b. Uma mudança em um Cache que poderia ser tanto um Commit ou um Rollback acontecerá através dos Databases e estas mudanças impactarão o Cache.
##### c. Isso é a razão de utilizarmos a anotação @Transactional.
