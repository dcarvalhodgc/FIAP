# Roteiro do vídeo — FinHub

> Sugestão de duração: **8 a 10 minutos**, caso a disciplina não determine outro limite. Todos os integrantes devem aparecer e explicar uma parte. Substituam os marcadores pelos nomes reais e adaptem as falas ao estilo do grupo.

## Preparação

- Usar os diagramas em tela cheia e conferir se os textos estão legíveis;
- Gravar em ambiente silencioso e testar o áudio;
- Não ler todo o README: contar o problema, as decisões e os riscos;
- Cada integrante deve fazer a transição para o próximo;
- Inserir no README um link do vídeo com permissão de visualização.

## Bloco 1 — Problema, storytelling e objetivo

**Responsável:** `[Integrante 1]`
**Tempo sugerido:** 1min30s a 2min

### Fala orientadora

“Nosso projeto é o FinHub, um agregador de contas por Open Finance. O problema que escolhemos é a fragmentação da vida financeira. Uma pessoa que possui contas em diferentes bancos precisa abrir vários aplicativos para descobrir seu saldo total e acompanhar transações.

Na nossa história, Marina possui três instituições. Pelo FinHub, ela se autentica, autoriza o compartilhamento na instituição e passa a consultar uma visão consolidada. A proposta não é guardar senhas bancárias e nem realizar pagamentos nesta primeira versão.

Definimos uma Arquitetura Mínima Viável somente para leitura: contas, saldos, transações, consentimento, revogação e atualização dos dados. Queremos aprender se essa visão realmente reduz esforço e se conseguimos oferecer uma experiência segura mesmo dependendo de APIs externas.”

### Mostrar

- Resumo executivo do README;
- Storytelling;
- Escopo incluído e excluído.

### Transição

“Agora, `[Integrante 2]` apresentará os usuários, as perguntas e os riscos que orientaram a arquitetura.”

## Bloco 2 — Stakeholders, aprendizado e riscos

**Responsável:** `[Integrante 2]`
**Tempo sugerido:** 2 minutos

### Fala orientadora

“O usuário principal é o titular dos dados, que quer consultar sua posição financeira em um único lugar. Também consideramos atendimento e operação, porque eles precisam explicar falhas e recuperar sincronizações com rastreabilidade.

A pergunta de maior impacto é se o FinHub poderá acessar o ecossistema diretamente ou precisará de uma instituição parceira. Por isso, nossa primeira ação de aprendizado é validar o caminho de participação e desenvolver em sandbox.

Os maiores riscos são acesso indevido, uso de consentimento revogado, impossibilidade de participação em produção e inconsistência dos dados. Tratamos esses riscos com autenticação forte, menor privilégio, criptografia, validação do consentimento antes da sincronização, modelo canônico, idempotência e uma decisão de go/no-go antes de produção.

O pior cenário seria expor dados financeiros ou continuar consultando uma instituição sem consentimento válido. Por isso, segurança, privacidade e consentimento são critérios de bloqueio.”

### Mostrar

- Tabela de perguntas;
- Plano de aprendizado;
- Matriz de riscos;
- Stakeholders.

### Transição

“Com essas perguntas e riscos definidos, `[Integrante 3]` explicará a arquitetura inicial e os níveis de contexto e containers.”

## Bloco 3 — Freeform, C4 Contexto e Containers

**Responsável:** `[Integrante 3]`
**Tempo sugerido:** 2min30s a 3min

### Fala orientadora — Freeform

“O Freeform conta a jornada completa. O cliente acessa a aplicação web, autentica-se e passa pelo API Gateway. O backend controla consentimentos e constrói o painel. Quando o cliente solicita uma atualização, o backend publica uma mensagem; o worker processa em segundo plano, consulta as APIs autorizadas, normaliza e persiste os dados. Logs, métricas e traces acompanham todo o fluxo. O CRM aparece tracejado como evolução, fora da MVA.”

### Fala orientadora — Contexto

“No nível de contexto, o FinHub é tratado como um único sistema. Vemos os clientes e analistas que interagem com ele, além do provedor de identidade, das instituições Open Finance e do CRM futuro. Esse desenho nos ajuda a definir fronteiras e responsabilidades sem entrar em tecnologia interna.”

### Fala orientadora — Containers

“No nível de containers, abrimos o FinHub. Temos uma aplicação React/Next.js, gateway, backend modular em Java e Spring Boot, fila RabbitMQ, worker e PostgreSQL. A sincronização é assíncrona para isolar latência e indisponibilidade de terceiros. O painel lê a última versão persistida e mostra o horário da atualização. Escolhemos um monólito modular com worker separado para evitar a complexidade prematura de vários microserviços.”

### Mostrar

- `arquitetura-freeform.jpg`;
- `c4-contexto.jpg`;
- `c4-containers.jpg`.

### Transição

“Por fim, `[Integrante 4]` mostrará o nível de componentes, os padrões e as decisões tomadas sob incerteza.”

## Bloco 4 — Componentes, padrões, decisões e encerramento

**Responsável:** `[Integrante 4]`
**Tempo sugerido:** 2min30s a 3min

### Fala orientadora — Componentes

“O nível de componentes amplia o worker de sincronização. O consumer recebe o pedido e coordena o caso de uso. O validador verifica se o consentimento continua válido. O registro de conectores escolhe o adaptador adequado à instituição. A política de resiliência aplica timeout, retry e circuit breaker. Depois, o normalizador converte respostas externas para o modelo canônico e o serviço de persistência grava de forma idempotente.”

### Fala orientadora — Padrões

“Os padrões essenciais são API Gateway, processamento assíncrono, Ports and Adapters, Adapter, Strategy, Factory, Repository, retry com backoff, circuit breaker e consumidor idempotente. Também existem padrões implícitos: consistência eventual, defesa em profundidade, fail-safe e anti-corruption layer.”

### Fala orientadora — Decisões

“As decisões mais difíceis foram retirar pagamentos da primeira versão, aceitar consistência eventual e escolher um backend modular em vez de muitos microserviços. Algumas decisões foram tomadas sob incerteza, como a frequência de atualização e o modelo de participação. Mantivemos essas escolhas reversíveis por configuração e por interfaces de integração.

Não houve uma decisão sem retorno nesta fase. Justamente por isso, certificação e contrato de produção ficaram condicionados aos resultados dos experimentos. Concluímos que os diagramas estão completos para a MVA lógica, mas ainda precisam de visões de implantação, recuperação, dados e segurança antes de produção.”

### Encerramento

“O FinHub demonstra como perguntas, riscos e aprendizado orientam uma arquitetura. A solução começa pequena, mas já possui limites, padrões e controles que permitem evoluir com evidência. Obrigado.”

### Mostrar

- `c4-componentes.jpg`;
- Tabela de padrões;
- Decisões sob incerteza;
- Checklist C4.

## Adaptação conforme o tamanho do grupo

- **2 integrantes:** cada pessoa apresenta dois blocos;
- **3 integrantes:** unir problema e riscos; manter arquitetura e componentes separados;
- **4 integrantes:** usar a divisão proposta;
- **5 ou mais:** separar Freeform, Contexto e Containers entre pessoas diferentes e dividir requisitos/riscos.

## Checklist final da gravação

- [ ] Todos os integrantes falaram;
- [ ] O problema e a proposta de valor ficaram claros;
- [ ] O escopo somente leitura foi explicado;
- [ ] As perguntas e os riscos foram relacionados às decisões;
- [ ] Os quatro diagramas foram mostrados de forma legível;
- [ ] Contexto, Containers e Componentes foram diferenciados;
- [ ] Padrões essenciais e ocultos foram mencionados;
- [ ] Incertezas e limites da arquitetura foram reconhecidos;
- [ ] O link do vídeo foi testado em janela anônima;
- [ ] O README contém nomes, RMs e link final.
