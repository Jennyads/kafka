<h2>Anotações do curso: "Domine Apache Kafka, Fundamentos e Aplicações Reais"</h2>
Dados: processsamos dados que são capazes de produzir informação e conhecimentos;
Dimensões do valor do dado: tempo (identificar fraude), relação (vendas do dia, semana, ano), valor, volume, variedade.
Apache Kafka: plataforma de streaming para pipelines de dados de alto desempenhos, análise de streaming, integração de dados e aplicações de missão crítica.

Exemplo: 
- Ouvinte de música (consumidor)
- Produtor de música (produtores)


Produtores querem publicar músicas e consumidores querem consumir 
Assim produtores publicam musicas pelo serviços de streaming e consequentemente o consumidor encontra essa música pelo serviço de streaming. Ambos concentram a música em um mesmo lugar. 

Exemplo:
folha de pagamento que envia dados para mais de um sistema (contabilidade, auditoria, finanças, DW, RH) - Problema de mesma informação compartilhada
E-commerce que lança centenas de logs, assim pode ter problema com registro no banco, erro no banco e perca dos logs. Problema: diferentes ritmos (lança uma promoção), indisponibilidade, perda de dados ou reprocessamento
Problemas: mesma informação compartilhada, sobrecarga (diferentes ritmos), indisponibilidade, perda de dados ou reprocessamento. 

Publiser - produzem dados para os kafka
Subscribers - consomem o que interessa (assina)
- Podem trabalhar em diferentes ritmos, consumidores podem ler dados mais de uma vez, alta disponibilidade e capacidade com recursos de cluster e particionamento.

Cluster:
Exemplo: uma aplicação que processa dados, assim a empresa aumenta e consequentemente a capacidade de processamento deve aumentar.
Solução de escalar verticalmente: faria como um upgrade para aumentar a demanda (aumentar capacidade de equipamento), para isso precisa parar a aplicação e requer configurações complexas, além de incompatibilidade.

Escalamento horizontal: adicionar mais equipamentos (novos lotes, redes de computadores para processar dados) que fazem parte desse novo cluster de processamento. Não requer parar o sistema para aumentar a capacidade, configurações mais simples, menos problemas com incompatibilidade, "hardware commodity", alta disponibilidade: tolerência a falhas. Rede de computadores com o mesmo objetivo - CLUSTER. 

Caso de aplicação de varejo com muitas divisões, pode fazer cópias do banco. Assim poderia dividir os dados por regiões do Brasil, por exemplo. 
Particionamento: dividir os dados fisicamente, possui parte dos dados, a aplicação consulta a partição que tem os dados de interesse. 

Replicações: pega as partições e replica em diferentes servidores. Sistema com uma probabilidade de falha é muito menor nas busca por dados.  


Em Apache Kafka, um broker é um servidor que armazena e processa dados. Um cluster é um grupo de brokers que trabalham juntos para fornecer um serviço de mensagens confiável e escalar.

Aqui está uma tabela que resume as principais diferenças entre broker e cluster:

Característica |	Broker |	Cluster
Definição |	Um servidor que armazena e processa dados |	Um grupo de brokers que trabalham juntos
Componentes |	Um broker é um servidor individual |	Um cluster é composto por vários brokers
Funções   |	Armazena e processa dados  |	Fornece um serviço de mensagens confiável e escalável


Um broker é um servidor que armazena e processa dados em Kafka. Cada broker é identificado por um ID exclusivo. Os brokers são responsáveis por armazenar os dados de tópicos, que são coleções de mensagens.

Os brokers podem ser configurados para replicar os dados de tópicos em vários brokers. Isso ajuda a garantir que os dados estejam disponíveis mesmo se um broker falhar.

Clusters

Um cluster é um grupo de brokers que trabalham juntos para fornecer um serviço de mensagens confiável e escalável. Os clusters são compostos por vários brokers, cada um com um ID exclusivo.

Os clusters são usados para aumentar a capacidade de processamento e armazenamento de Kafka. Eles também podem ser usados para melhorar a disponibilidade de dados.

Exemplos

Um exemplo de um broker é um servidor que armazena dados de um tópico de log. Um exemplo de um cluster é um grupo de brokers que armazenam dados de vários tópicos.

Conclusão

Brokers e clusters são conceitos importantes em Apache Kafka. Os brokers são os componentes básicos de Kafka, enquanto os clusters são usados para aumentar a capacidade e a disponibilidade de Kafka.

Kafka broker: serviço/servidor que o kafka recebe (intermediário), cada nó do cluster pode rodar uma ou mais intâncias de brokers. 
- Pode ter mais de um serviço em um mesmo servidor 
- Atribui numeração sequencial (offset) as mensagens 
- Salva em disco (não precisa ser consumido em tempo real, permite recuperar em falhas)
- Ckuster controller: "chefe" escolhido aleatoriamente


Bootstrap: aplicação que serve de porta de entrada para outros servidores.

- Não precisa conhecer a estrutura do cluster (faz conexão com apenas um broker)
- Brokers sabem a estrutura e informam ao cliente;
- Pode-se então conectar-se a um broker específico;
- Conexão a um broker é uma conexão ao cluster


- Um broker por ter "n" topics
- Um topic pode ter "n" partições
- Podem haver brokers que não tenham partições de determinados topics;

- Controler: é o primeiro broker a entrar no cluster, se ele cai, outros tentar ser, mas só um é eleito. Controller: controla a lista de brokers disponíveis no cluster.

As principais partições são chamadas de leaders, as copias são chamadas de followers e dependem do fator de replicação.
Followers apenas se mantem atualizados com o leaders. 

ISR - mostra quais partições (followers) que estão sincronizados com os leaders e são candidatas a ser leaders.  Necessário definir um mínimo de replicas na lista.

São distribuidas por um balanceamento, copias devem ficar em máquinas diferentes (tolerância a falhas), racks são considerados, pois o rack inteiro pode falhar. 

O broker pode ser leader ou follower de determinadas partições.
- A partição leader é responsável por todas as requisições de producers e consumers.
- O producer recebe uma lista de leaders e decide para para qual partição quer mandar os dados.
- O producer manda para o broker da partição leader.
- O broker persiste a mensagem e retorna uma confirmação (acknowledgment)
- O consumr também lê a mensagem da partição leader.

Segments: dados ficam em arquivos fisícos: diretórios nomeados de "logs".
- Arquivo de partições são divididos em arquivos menores chamados de segmentos. Os dados vão para o primeiro segmento, até o limite, então ele inicia um novo arquivo.


Offset:  um offset é um número que representa a posição de um determinado registro em um tópico. Os offsets são usados para rastrear o progresso de um consumidor de grupo à medida que consome mensagens de um tópico.Cada partição de um tópico Kafka tem seu próprio conjunto de offsets, que indicam o último registro que foi processado com sucesso pelo consumidor de grupo para aquela partição. Permite manter o estado, reiniciar e identificar mensagem de forma única.
Offset não reinicia em um novo segmento. O número do arquivo do segmento possui o número do primeiro offset do segmento. Offset não é único no tópico, e sim na partição. 



<h2>Kafka</h2>
Este é um sistema open source altamente distribuído, conhecido como Apache Kafka, que opera com fluxo contínuo de dados. Sua importância reside na capacidade de lidar com eventos em sistemas, auxiliando na geração de dados históricos que podem ser monitorados posteriormente. O Kafka coleta todos os eventos, armazena, manipula e preserva essas informações para fins específicos. Ele é altamente escalável e possui baixa latência, além de oferecer uma notável tolerância a falhas.

Arquitetura:
O Kafka opera em um ambiente de cluster distribuído, no qual diversas máquinas, chamadas "brokers", trabalham juntas para gerenciar a troca de dados. O Zookeeper é um componente-chave que atua como sistema de descoberta de serviços, coordenando os brokers em execução. Ele gerencia erros, recuperações e permissões. Adicionar um novo broker é uma tarefa que o Zookeeper realiza com eficiência, garantindo a integridade do cluster Kafka.

Tópicos e Partições:
Os dados no Kafka são organizados em tópicos, que podem ser considerados como canais que direcionam as informações para serem armazenadas. Os consumidores leem esses tópicos, e as mensagens são enfileiradas em partições. A separação dos tópicos em várias partições, também chamadas de "segmentos", acelera o processo de leitura e permite escalabilidade. Não é necessário que as partições de um tópico residam no mesmo broker.

Registro (Record):
Cada registro de dados no Kafka possui uma estrutura de mensagem que consiste em cabeçalhos (metadados opcionais), uma chave (que ajuda a manter o contexto da mensagem) e um valor (o conteúdo da mensagem). O valor pode estar em diferentes formatos, como texto, JSON ou protobuf. Além disso, cada registro possui um carimbo de data e hora (timestamp) que indica quando foi gerado.

Tópico Compactado:
Os tópicos compactados no Kafka mantêm uma versão compacta do log, o que ajuda a reduzir o armazenamento e otimiza a leitura de dados.

Distribuição de Partições e Fatores de Replicação:
As partições podem ser distribuídas independentemente dos tópicos, o que significa que diferentes partições podem residir em diferentes brokers. O fator de replicação determina quantas cópias de uma partição são mantidas em vários brokers. Isso garante a redundância e a disponibilidade dos dados. O valor do fator de replicação é determinado com base na importância do dado para o negócio.

Entrega de Mensagens:
O Kafka usa um algoritmo de round-robin para distribuir as mensagens nas partições por padrão, o que não garante a ordem das mensagens. Para garantir a ordem, é importante direcionar mensagens com a mesma chave para a mesma partição.

Partições Líderes e Seguidoras:
Cada partição tem um líder e seguidoras. Quando um consumidor lê uma mensagem, ele a lê da partição líder. Se o líder falhar, o Kafka seleciona uma das seguidoras como a nova líder.

Produtores:
Os produtores criam mensagens com um tópico, chave e valor. As mensagens são serializadas, enviadas para uma partição e broker específico. Os produtores podem especificar diferentes níveis de confirmação (ACK) para garantir a entrega das mensagens.

Formatos de Garantia de Entrega:

At Most Once: Pode perder algumas mensagens.
At Least Once: Garante que as mensagens sejam entregues pelo menos uma vez, mas algumas podem chegar duplicadas.
Exactly Once: Garante que cada mensagem seja entregue exatamente uma vez.
Indepotente Off: Armazena mensagens duplicadas em caso de falhas de conexão.
Indepotente On: Identifica e descarta mensagens duplicadas.
Consumidores:
Os consumidores são responsáveis por ler as mensagens. Eles podem ser programados para ler todas as partições e podem ser organizados em grupos para facilitar a leitura e garantir alta disponibilidade.

Segurança:
O Kafka suporta criptografia de mensagens durante a transmissão e permite a autenticação e autorização para proteger o acesso aos dados.

Kafka Connect e Kafka REST Proxy:
O Kafka Connect é uma ferramenta que permite a integração de dados de várias fontes em tópicos do Kafka. O Kafka REST Proxy fornece uma interface HTTP para interagir com o Kafka, simplificando a comunicação com o sistema sem a necessidade de drivers específicos. Ambos são úteis para a integração de dados em um ambiente Kafka.


<h3>Anotações</h3>
É um sistema open source que trabalha de forma distribuida.
Trabalha com streaming de dados. 
Porque é necessário? Quase todos os sistemas são orientados a eventos, assim ajuda a gerar dados históricos para serem monitorados.
Pega todos os eventos e salva esses dados, manipula e guarda as informações para usos específicos. 
É escalável e tem baixa latência. Tem grande tolerância a falhas. 

Producer (gerador de dado) -> encaminhado para o kafka (funciona no formato de cluster, diversas maquinas que rodam (broker que tem seu próprio banco de dados)) -> consumer (sistema interessado em pegar o dado do kafka) fica olhando otempo inteiro 
Zookeeper - sistema de service-discovery, orquesta os brokers que o kafka está rodando. Gerencia erros e recuperações, além de permissões. Se precisa add um novo broker, o zookeper se responsabiliza por isso. Cuida do cluster/kafka.

Topics: como se fossse um cano que joga a informação que vai ser armazenado no kafka. O consumer lê o topic. Como se fosse um grande log, as mensagens ficam enfileiradas em partições. 

Topic sale -> sepera o topic em partições (boneca, casa, carro) vão ajudar o processo de leitura mais rápido e de forma paralela, escala mais, não necessariamente precisa estar no mesmo broke -> segmentos (diversos arquivos, banco de dados) 

Record:
Cada registro tem uma estrutura de mensagem com: headers (metadata, informações que podem ser úteis, não é obrigatório), key ajuda a manter o contexto de uma mensagem, value é o conteudo da mensagem que está sendo enviado(pode estar em vários formatos, text, json, protobuf), timestamp toda vez que o record for gerado, gera o timestamp. 

Compacted topic: 
log compactado pega o resumo 

Distrubuição de partições:
topic: sale
topic: clients 

intedependete dos topic, as partições deles podem estar no mesmo broker, por exemplo: partição 1 da sale com a partição 3 do client no BROKER A

Replication factor: pelo menos x cópia em outros brokers. Sempre vai ter a replica em diferentes brokers, o valor de replica pode ser determinado conforme o usuário quiser (criticidade do negocio). Assim garante que o sistema funcione corretamente e de forma infinita. 
Resumindo: quando crio um topic, determino a quantidade partições e replicações vai fazer. 

Delivery: por padrão o kafka não gera uma regra principal para entregar as mensagens em cada partição, usa algoritmo de rond robin para fazer essas distribuições. 

Não consegue garantir a ordem das partições, assim parte do principal qaue as mensagens vão estar desordenadas. Para pegar de forma ordenada, pega o mesmo "aqui", que tem a chave por exemplo vendas, que vai para a mesma partição. 

Partição líder: das replicações, uma é lider e as outras são seguidoras, toda vez que alguém consumir a mensagem, vai ser referente a do líder. Só se o líder cair, vai para o próximo. 

Producer: a mensagem é criada com topic, chave e valor. Ao enviar, a mensagem vai ser serializada (formato determminado), mandada para partição e broker. 
Ack 0: não vai avisar/dar retorno se a a mensagem foi gravada no broker (assim torna o processo mais rápido)
Ack 1: dá o retorno se a mensagem foi lida e gravada pelo líder (garantia de entrega).
Ack -1 ALL: dá o retorno que a mensagem foi lida e gravada no broker lider e nos seus seguidores, só retorna quando for gravado em todos (mais demorado)

Formatos: 
- at most once: pode perder algumas mensagens
- at least once: garante que as mensagens sejam entregues pelo menos uma vez, algumas podem chegar duplicadas (necessário tratar o programa para remover mensagens duplicadas)
- exacly onde: garante que mande 1 vez


- Indepotente off: manda/guarda mensagem duplicada em caso de falha de conexão/erro
- Indepotente on: consegue identificar mensagem duplicada e exclui (descarta mensagens iguais)



Consumers:
Responsáveis por ler as mensagens, pode ser um programa/software qualquer desenvolvido para isso. O consumer lê todas as partições. 
Pode ter grupos de consumidores para facilitar a leitura das partições e deixar mais rápido. Nesses grupos, o kafka vai fazer a distribuição entre eles. 
Um consumer por partição, o bom é ter a mesma quantidade de partição para cada consumer. 

Quando o consumer para de responder, o kafka faz um rebalanceamento, fazendo os apontamentos. Toda vez que muda o número de consumers, o kafka faz o rebalanceamento.

Segurança:
é possivel trabalhar com criptozação de mensagens no processo de transmissão.
Quando é gravada no broker, não fica criptografada.
Pode trabalhar com autorização ou autenticação.

Kafka connect:
Forma de pegar informações de um lugar e jogar em outro lugar.
Diversos conectores que conseguem pegar dados por exemplo do twitter, mysql e coloca nos topicos.
é como se fosse um cluster(várias máquinas workers) que joga no kafka.
Depois de jogar no kafka, ainda dá para jogar em outro local externo.

Kafka REST Proxy:
App -> HTTP -> REST Proxy -> Kafka
Para que não precisa se conectar ao kafka por um driver padrão. Só faz uma requisição HTTP e joga os dados no kafka por uma API.
