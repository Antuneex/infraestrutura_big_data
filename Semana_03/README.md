# Semana 3: Provisionamento de Cluster e Experimentação com Big Data no Google Cloud Shell

Durante a terceira semana de estudos, a proposta foi reproduzir operações essenciais de um ecossistema de Big Data por meio do Google Cloud Shell. As atividades abrangeram a montagem de uma estrutura de Data Lake no ambiente local, manipulação de dados pelo terminal, testes de sobrecarga na CPU, gerenciamento de containers via Docker e automatização de tarefas com Shell Script.

## Atividades Realizadas

### 1. Montagem do Data Lake e Processamento (Simulação de MapReduce)
Inicialmente, construímos a estrutura base de diretórios (`raw`, `processed`, `logs`) para reproduzir as camadas de um Data Lake. Na sequência, empregamos comandos Linux para simular tanto a entrada de um arquivo CSV quanto o seu processamento.

**Comandos utilizados:** Criação dos diretórios com `mkdir -p`, inclusão de registros fictícios com `echo`, e tratamento dos dados em cadeia usando `cat`, `grep` (para filtrar), `wc -l` (para contar linhas) e `sort | uniq` (para ordenar e eliminar duplicatas).

### 2. Observação de Processos e Simulação de Sobrecarga (Stress Test)
A fim de compreender o impacto de processos sobre os recursos computacionais, recorremos a instrumentos de monitoramento e provocamos um cenário de saturação de processamento.

**Comandos utilizados:** O utilitário `top` foi acionado para acompanhar os processos em execução em tempo real. Em seguida, disparamos o comando `yes > /dev/null &` em background para saturar a CPU, encerrando-o posteriormente com `kill` a partir do PID (1947).

### 3. Implantação de Cluster com Docker (Nó Master e Worker)
Com o intuito de reproduzir um ambiente de processamento distribuído (similar ao Hadoop ou Spark), configuramos uma rede isolada e provisionamos dois containers Docker distintos: um exercendo o papel de nó coordenador e outro como nó de execução.

**Comandos utilizados:** `docker network create` para estabelecer uma rede privada, seguido de `docker run` para inicializar os containers `master` e `worker` a partir da imagem Ubuntu. Entramos no terminal do master via `docker exec`, instalamos o pacote `iputils-ping` e executamos `ping worker` para validar a comunicação entre os nós do cluster.

### 4. Automatização via Shell Script
Para eliminar a repetição manual de comandos na criação da infraestrutura, reunimos todas as instruções em um único script executável.

**Comandos utilizados:** Empregamos o editor `nano init_cluster.sh` para escrever o script, concedemos permissão de execução com `chmod +x` e o executamos com `./init_cluster.sh`.

---

## Respostas ao Questionário Teórico

**1 - Qual a relação entre a estrutura de diretórios criada (raw / processed / logs) e o conceito de Data Lake? Como isso se compara ao HDFS em produção?**

A organização de diretórios construída reproduz as camadas lógicas de um Data Lake — conhecidas como Bronze, Silver e Gold. A pasta `raw` concentra os dados brutos preservados em sua forma original (camada de ingestão); a `processed` reúne os dados após passarem por limpeza, filtragem e transformações; e a `logs` registra metadados e histórico de execuções. No HDFS (Hadoop Distributed File System) em ambiente produtivo, a mesma lógica de separação em camadas é adotada, mas os arquivos deixam de residir em um único disco local — eles são fragmentados em blocos e espalhados por múltiplos servidores (DataNodes), com replicação para assegurar tolerância a falhas e disponibilidade contínua.

**2 - Como o operador pipe `|` do Linux se relaciona com o modelo MapReduce? Descreva a analogia entre cada etapa.**

O pipe (`|`) encadeia comandos ao redirecionar a saída padrão (stdout) de um processo diretamente para a entrada padrão (stdin) do seguinte, permitindo o tratamento dos dados em fluxo contínuo. Esse mecanismo guarda uma analogia clara com o modelo MapReduce:

- **Map (Mapeamento):** Os primeiros comandos da cadeia, como `cat` e `grep`, desempenham o papel da função *Map*, percorrendo os dados linha a linha e aplicando regras de filtragem ou transformação de forma independente.
- **Reduce (Redução):** Os comandos posicionados ao final do pipe, como `wc -l` ou `uniq`, exercem a função de *Reduce*, consolidando, agregando ou agrupando os resultados produzidos pelas etapas anteriores.

**3 - O que aconteceu com a CPU ao simular carga com `yes > /dev/null`? Como isso afetaria um cluster Spark em produção?**

O comando `yes > /dev/null` estabelece um laço infinito de geração de texto que é descartado instantaneamente, levando o consumo de um núcleo da CPU a 100%. Em um cluster Spark produtivo, caso um job deficiente — seja por um laço infinito, um produto cartesiano acidental ou problemas de *Data Skew* — provoque esse tipo de saturação, o nó Worker afetado ficará completamente travado. Isso comprometeria o progresso de toda a DAG, pois o Spark aguardaria a conclusão daquela tarefa. Se a sobrecarga chegar a interromper a comunicação de rede do nó, o Master pode declará-lo inativo (por timeout) e redistribuir suas tarefas para os demais Workers, espalhando a pressão por todo o cluster.

**4 - O que cada container Docker (master e worker) representa em um cluster Hadoop real? Quais serviços rodam em cada um?**

Os dois containers espelham a arquitetura distribuída com divisão entre nó gerenciador e nós executores.

- **Master:** Corresponde ao nó de coordenação. Em um cluster Hadoop real, esse nó hospedaria o **NameNode** (responsável pelo catálogo de metadados e pelo mapeamento de onde cada bloco de dados está armazenado) e o **ResourceManager / YARN** (que distribui os recursos computacionais e escalona a execução das tarefas).
- **Worker:** Corresponde ao nó de processamento, incumbido da execução propriamente dita. Nele operariam o **DataNode** (que retém fisicamente os blocos de dados em disco) e o **NodeManager** (que recebe e executa as tarefas determinadas pelo Master).

**5 - Por que a automação via Shell Script é fundamental em ambientes de Big Data? Cite ferramentas de IaC que evoluem esse conceito.**

Infraestruturas de Big Data frequentemente envolvem dezenas ou centenas de servidores. Provisionar manualmente diretórios, instalar dependências e inicializar serviços em cada máquina individualmente é inviável em escala e altamente propenso a inconsistências. O Shell Script resolve isso ao padronizar e repetir as rotinas de configuração com confiabilidade. Para infraestruturas de maior porte, evoluímos para ferramentas de IaC (Infrastructure as Code), que são declarativas, idempotentes e mais robustas. Entre as mais relevantes estão o **Terraform** (especializado no provisionamento da infraestrutura em nuvem, como a criação das VMs) e o **Ansible**, **Chef** ou **Puppet** (voltados ao gerenciamento de configuração, responsáveis por instalar e ajustar os softwares dentro de cada nó do cluster).
