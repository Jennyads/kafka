
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
