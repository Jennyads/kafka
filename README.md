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

intedependete dos topic, as partições deles podem estar no mesmo broker, por exemplo: partição 1 da sala com a partição 3 do client no BROKER A

Replication factor: pelo menos x cópia em outros brokers. Sempre vai ter a replica em diferentes brokers, o valor de replica pode ser determinado conforme o usuário quiser (criticidade do negocio). Assim garante que o sistema funcione corretamente e de forma infinita. 
Resumindo: quando crio um topic, determino a quantidade partições e replicações vai fazer. 
