# 📝 Resposta do Laboratório: A Wiki Perdida dos Arquivos Corporativos

> Preencha este arquivo com a sua proposta de solução.
>
> Sua resposta deve explicar como transformar os documentos brutos da pasta `raw/` em uma Wiki Corporativa Inteligente, pesquisável e segura usando apenas serviços da AWS.

---

## 👤 Identificação

**Nome:**  
Anderson Augusto Das Neves Santos Pequeno

**Data:**  
03/09/2026

**Link do repositório:**  

https://github.com/AugustoBT52/laboratorio-wiki-aws/edit/main/resposta.md
---

# ✅ Quest 1: O Mapa dos Arquivos Perdidos

## 1.1 Formatos encontrados na pasta `raw/`

Descreva quais tipos de arquivos existem dentro da pasta `raw/`.

```md
Exemplo de como responder, com o formato e o que ele implica:
- <extensao>: <nasce digital ou precisa de OCR?>, <o que da para extrair>
```

> Abra a pasta e liste o que voce encontrou de fato. Esta quest avalia a sua
> leitura do acervo, entao a resposta certa e a que corresponde aos arquivos.

**Sua resposta:**

```md
 PDF: nasce digital, não precisa de OCR; permite extrair o texto e a estrutura textual do documento.
PNG: imagem digitalizada, precisa de OCR; permite extrair textos e informações estruturadas, como tabelas.
 CSV: nasce digital, não precisa de OCR; contém dados tabulares estruturados, como registros e campos das oportunidades de vendas.```

---

## 1.2 Principais desafios encontrados

Explique quais dificuldades esses documentos podem apresentar.

```md
Exemplo:
- Arquivos sem padrão de nomenclatura
- Documentos escaneados com baixa qualidade
- Textos manuscritos ou parcialmente ilegíveis
- Atas com estruturas diferentes
- Informações importantes espalhadas em vários formatos
```

**Sua resposta:**

```md
 O PNG contém uma ata de reunião em formato de imagem, então é necessário usar OCR para transformar a imagem em texto.
 O carimbo sobre uma das datas pode dificultar a leitura dessa informação durante o OCR.
 A tabela possui a abreviação "MI" para representar milhões. Dependendo da forma como o conteúdo for extraído, isso pode ser interpretado apenas como a sigla "MI", perdendo seu significado.
 Os arquivos possuem formatos diferentes e, por isso, precisam de tratamentos diferentes: o PDF possui texto digital, o PNG precisa de OCR e o CSV possui dados organizados em tabela.
- No PDF, a repetição da informação de que o documento é fictício pode acabar sendo extraída junto com o conteúdo e gerar um pouco de ruído na hora de organizar e indexar os dados.```

---

## 1.3 Informações importantes a serem extraídas

Liste quais informações precisam ser identificadas para transformar os documentos em conhecimento pesquisável.

**Sua resposta:**

```md

Título e assunto

Data da reunião ou do documento

Participantes e responsáveis

Decisões tomadas

Atividades e pendências

Prazos

Resultados e informações importantes

Valores e dados de vendas

Origem e tipo do documento

```

---

## 1.4 Estratégia de classificação inicial

Como você classificaria os documentos sem depender de subpastas dentro de `raw/`?

**Sua resposta:**

```md
Os documentos podem ser classificados por meio de metadados, sem precisar criar subpastas dentro de `raw/`. Eu usaria informações como:

Nome do arquivo  
Tipo/formato do arquivo  
Data do documento  
Categoria ou assunto  
Origem do documento  
Tipo de conteúdo, como texto, imagem ou dados de tabela.  
Necessidade de OCR  
Nível de acesso ou sensibilidade```

---

# ✅ Quest 2: O Portal de Entrada na AWS

## 2.1 Armazenamento dos arquivos brutos

Explique como os arquivos da pasta `raw/` seriam enviados e armazenados na AWS.

Serviços que você pode considerar:

- Amazon S3
- AWS IAM
- AWS KMS
- Amazon S3 Versioning
- Amazon S3 Lifecycle

**Sua resposta:**

```md
Os arquivos da pasta `raw/` seriam enviados para um bucket do Amazon S3, mantendo os arquivos originais sem alterações. O acesso ao bucket seria controlado pelo AWS IAM, permitindo que apenas usuários e serviços autorizados possam acessar os arquivos.

O S3 Versioning poderia ser utilizado para manter versões dos arquivos caso algum documento fosse substituído ou alterado. O AWS KMS poderia ser utilizado para criptografar os dados armazenados.

O S3 Lifecycle poderia ser utilizado posteriormente para definir regras de retenção e movimentação dos arquivos, caso fosse necessário reduzir custos ou remover arquivos após determinado período.```

---

## 2.2 Preservação dos arquivos originais

Explique como garantir que os arquivos originais sejam mantidos intactos e rastreáveis.

**Sua resposta:**

```md
Para manter os arquivos originais intactos, eles seriam armazenados no bucket S3 em uma área destinada aos arquivos brutos, sem serem modificados durante o processamento. O S3 Versioning poderia ser utilizado para manter versões anteriores caso algum arquivo fosse substituído.

Também seria importante manter informações como o nome do arquivo, data de envio e origem, permitindo identificar e rastrear cada documento ao longo do processamento.

Além disso, as alterações e tentativas de acesso aos arquivos podem ser registradas pelo AWS CloudTrail. Esses registros podem ser monitorados pelo Amazon CloudWatch, que pode gerar alertas caso sejam identificadas alterações ou tentativas de acesso não autorizadas.```

---

## 2.3 Extração de texto dos documentos

Explique como cada tipo de arquivo seria processado.

Considere:

- PDFs escaneados;
- Imagens;
- PDFs digitais;
- Arquivos `.txt`;
- Arquivos `.docx`;
- Arquivos `.md`.

Serviços que você pode considerar:

- Amazon Textract
- AWS Lambda
- AWS Step Functions
- Amazon S3
- Amazon CloudWatch

**Sua resposta:**

```md
Os arquivos seriam processados de acordo com o seu formato e com a forma como seus dados estão armazenados.

Para o PNG, seria utilizado o Amazon Textract para realizar o OCR e extrair o texto e as informações estruturadas da imagem.

Para o PDF digital, não seria necessário utilizar OCR, pois o documento já possui uma camada de texto. Nesse caso, o texto poderia ser extraído diretamente e posteriormente normalizado.

Para o CSV, seria utilizado o AWS Glue para realizar o tratamento dos dados tabulares e organizar seu esquema.

A arquitetura também poderia ser estendida posteriormente para outros formatos, como TXT, DOCX e Markdown, adicionando etapas específicas de extração conforme a necessidade.```

---

## 2.4 Tratamento de falhas

Explique como sua solução identificaria e registraria erros de processamento.

**Sua resposta:**

```md
Os erros de processamento seriam identificados pelo AWS Step Functions, que permite definir novas tentativas (Retry) para falhas temporárias e tratar erros que continuarem acontecendo por meio de exceções (Catch).

Um exemplo seria a entrada de um arquivo em um formato não permitido ou que ainda não possui um processo definido na solução. Nesse caso, o arquivo seria direcionado para o tratamento de erro, em vez de seguir para uma etapa de processamento incompatível.

Os erros e informações do processamento seriam registrados no Amazon CloudWatch, permitindo acompanhar quais arquivos apresentaram problemas e em qual etapa eles ocorreram. O CloudWatch também poderia gerar alertas quando ocorrerem falhas.

Após um erro, o arquivo poderia ser marcado como falho e posteriormente reprocessado, sem precisar processar novamente os arquivos que já foram concluídos com sucesso.
```

---

# ✅ Quest 3: A Relíquia dos Metadados

## 3.1 Padronização dos textos processados

Explique como os textos extraídos seriam limpos, normalizados e preparados para consulta.

**Sua resposta:**

```md
Os textos extraídos seriam tratados para remover informações desnecessárias, corrigir problemas de formatação e padronizar o conteúdo. Também seriam organizados os metadados, como nome, tipo, data e origem do documento.

No caso de informações extraídas de imagens, seria necessário revisar possíveis erros do OCR, como números, datas, siglas e textos que estejam sobrepostos por outros elementos.

Depois do tratamento, os resultados dos diferentes tipos de arquivo seriam enviados para uma função AWS Lambda responsável por padronizar e normalizar os conteúdos, mantendo uma estrutura comum de texto e metadados. Esses dados seriam então armazenados no S3 para posterior indexação e consulta.```

---

## 3.2 Metadados propostos

Defina quais metadados você extrairia de cada documento.

| Metadado | Por que ele é importante? |
|---|---|
| Nome do documento | Preencha aqui |
| Tipo do documento | Preencha aqui |
| Data identificada | Preencha aqui |
| Tema principal | Preencha aqui |
| Participantes | Preencha aqui |
| Decisões tomadas | Preencha aqui |
| Responsáveis | Preencha aqui |
| Próximos passos | Preencha aqui |
| Nível de confidencialidade | Preencha aqui |
| Caminho do arquivo original | Preencha aqui |

Adicione outros metadados, se necessário.

---

## 3.3 Uso de IA para enriquecimento dos documentos

Explique como o Amazon Bedrock poderia ajudar a identificar temas, decisões, responsáveis, pendências e resumos dos documentos.

**Sua resposta:**

```md
O Amazon Bedrock poderia ser utilizado para analisar os textos já extraídos e normalizados, identificando informações importantes presentes nos documentos.

A IA poderia identificar os principais temas abordados, decisões tomadas durante reuniões, responsáveis por cada atividade, pendências e prazos, além de gerar resumos dos documentos.

Essas informações poderiam ser organizadas como metadados e utilizadas posteriormente para melhorar a busca. O Amazon Bedrock Knowledge Bases poderia utilizar os documentos processados para criar uma base de conhecimento pesquisável, permitindo encontrar informações relevantes nos documentos e utilizá-las posteriormente na geração de respostas.```

---

## 3.4 Armazenamento dos metadados

Explique onde os metadados seriam armazenados e como seriam conectados aos documentos originais.

Serviços que você pode considerar:

- Amazon S3
- Amazon DynamoDB
- AWS Glue Data Catalog
- Amazon Bedrock Knowledge Bases

**Sua resposta:**

```md
Os metadados poderiam ser armazenados junto aos documentos processados no Amazon S3, contendo informações como nome, tipo, data, origem, categoria e nível de acesso.

Cada documento teria uma identificação que permitiria relacionar os metadados ao arquivo original armazenado no S3, mantendo a rastreabilidade desde o documento bruto até o conteúdo processado.

O AWS Glue Data Catalog poderia ser utilizado para catalogar e organizar os dados estruturados, principalmente os provenientes do CSV. O Amazon Bedrock Knowledge Bases utilizaria os documentos processados e seus metadados para realizar a indexação, permitindo buscas por informações e filtros.

O DynamoDB poderia ser utilizado em uma evolução da solução caso fosse necessário manter informações adicionais de controle, como status e histórico de processamento.```

---

# ✅ Quest 4: O Oráculo da Wiki Inteligente

## 4.1 Estratégia de indexação

Explique como os documentos seriam divididos em trechos menores e preparados para busca semântica.

**Sua resposta:**

```md
Os documentos processados seriam divididos em trechos menores (content chunking, conforme a documentação da AWS), facilitando a localização das informações relevantes durante uma busca.

O Amazon Bedrock Knowledge Bases pode realizar essa divisão durante a ingestão dos documentos, utilizando diferentes estratégias de chunking, como tamanho fixo, hierárquico ou semântico. A estratégia pode ser escolhida de acordo com o tipo e a estrutura dos documentos.

Depois da divisão, os chunks podem ser transformados em embeddings e armazenados em um índice vetorial, mantendo a relação com o documento original. Isso permite realizar buscas semânticas e também manter a rastreabilidade das informações utilizadas nas respostas.```

---

## 4.2 Busca semântica e base vetorial

Explique como embeddings seriam gerados e onde seriam armazenados.

Serviços que você pode considerar:

- Amazon Bedrock Knowledge Bases
- Amazon OpenSearch Serverless
- Amazon Aurora PostgreSQL com pgvector
- Amazon S3 Vectors
- Modelos de embeddings no Amazon Bedrock

**Sua resposta:**

```md
Os embeddings seriam gerados a partir dos chunks dos documentos usando um modelo de embeddings disponível no Amazon Bedrock. Eles transformariam o conteúdo dos trechos em representações vetoriais, permitindo encontrar informações com significado semelhante, mesmo quando as palavras usadas forem diferentes.

O Amazon Bedrock Knowledge Bases poderia cuidar desse processo durante a ingestão dos documentos. Para armazenar os vetores, poderia ser utilizado o Amazon S3 Vectors, mantendo também a relação com os documentos e seus metadados.

Assim, quando o usuário fizer uma pergunta, ela também poderá ser transformada em um embedding e comparada com os vetores armazenados, encontrando os trechos mais relevantes para responder à pergunta.```

---

## 4.3 Geração de respostas com IA

Explique como a Wiki responderia perguntas em linguagem natural com base nos documentos originais.

Considere explicar:

- Como a pergunta do usuário seria recebida;
- Como os trechos relevantes seriam recuperados;
- Como o Amazon Bedrock geraria a resposta;
- Como a resposta indicaria as fontes utilizadas.

**Sua resposta:**

```md
A pergunta do usuário seria recebida por uma interface web da Wiki, que poderia ser desenvolvida e hospedada utilizando o AWS Amplify. O usuário faria sua autenticação pelo Amazon Cognito e, após o login, poderia enviar perguntas em linguagem natural pela interface.

A pergunta seria enviada para o backend e encaminhada ao Amazon Bedrock Knowledge Bases, que buscaria na base vetorial os chunks mais relevantes dos documentos. Esses trechos seriam utilizados como contexto para o Amazon Bedrock gerar uma resposta em linguagem natural.

A resposta seria apresentada na interface da Wiki junto com as fontes utilizadas, indicando os documentos e trechos que serviram de base para a resposta. Dessa forma, o usuário poderia conferir a origem das informações e manter a rastreabilidade da resposta.```

---

## 4.4 Interface de consulta

Proponha como os usuários acessariam essa Wiki Inteligente.

Serviços que você pode considerar:

- Amazon Q Business
- AWS Amplify
- Amazon API Gateway
- AWS Lambda
- Amazon Cognito

**Sua resposta:**

```md
A Wiki poderia ser disponibilizada por meio de uma interface web desenvolvida e hospedada com o AWS Amplify. O Amazon Cognito seria utilizado para fazer a autenticação dos usuários e controlar o acesso à aplicação.

Após o login, o usuário poderia fazer perguntas pela interface. As requisições seriam encaminhadas pelo Amazon API Gateway para uma função AWS Lambda, que faria a comunicação com o Amazon Bedrock Knowledge Bases e com o Amazon Bedrock.

A resposta gerada pela IA seria então retornada para a interface, junto com as fontes utilizadas, permitindo que o usuário consulte a origem das informações.

O Amazon Q Business também poderia ser uma alternativa para disponibilizar uma interface de consulta baseada nos documentos corporativos, mas para esta arquitetura seria utilizado o Amplify junto com API Gateway, Lambda e Cognito.```

---

## 4.5 Segurança, auditoria e monitoramento

Explique como controlar acesso, proteger dados, auditar consultas e monitorar custos, erros e qualidade das respostas.

Serviços que você pode considerar:

- AWS IAM
- AWS KMS
- Amazon Cognito
- AWS CloudTrail
- Amazon CloudWatch
- Amazon Macie
- AWS Cost Explorer

**Sua resposta:**

```md
O AWS IAM seria utilizado para definir o controle de acesso tanto dos usuários quanto dos recursos da arquitetura. Cada serviço teria somente as permissões necessárias para executar sua função. Por exemplo, uma função Lambda poderia ter permissão para acessar determinados objetos do S3, enquanto o Step Functions teria permissão para executar apenas os recursos necessários ao processamento e à consulta no Amazon Bedrock.

O AWS KMS seria utilizado para proteger os dados por meio de criptografia, principalmente os documentos armazenados no Amazon S3. O Amazon Cognito seria responsável pela autenticação dos usuários que acessam a interface da Wiki.

O Amazon Macie poderia ser utilizado para analisar os arquivos armazenados no S3 e identificar informações potencialmente sensíveis. Dessa forma, os arquivos da área `raw/` e os arquivos processados poderiam ser classificados e monitorados de acordo com o nível de sensibilidade das informações presentes.

O AWS CloudTrail registraria as ações realizadas nos recursos da AWS, permitindo identificar quem realizou determinada operação, qual recurso foi acessado ou alterado e quando isso aconteceu. O Amazon CloudWatch poderia monitorar esses registros, além dos logs e métricas dos serviços utilizados, permitindo gerar alertas em situações consideradas suspeitas ou fora do comportamento esperado.

Por exemplo, caso um usuário tivesse seu nível de acesso alterado ou ocorresse uma tentativa de acesso a um recurso para o qual ele não possui permissão, o CloudTrail poderia registrar a operação. O CloudWatch poderia monitorar esse comportamento e gerar um alerta para que a situação fosse analisada. Dessa forma, o controle de acesso, a auditoria e o monitoramento funcionariam de forma integrada.

Por fim, o AWS Cost Explorer seria utilizado para acompanhar os custos da solução, permitindo identificar o consumo dos serviços utilizados, acompanhar a evolução dos gastos e identificar possíveis aumentos inesperados de custo.```

---

# 🧩 Arquitetura Final da Solução

Agora reúna tudo em uma visão única.

## 1. Visão geral

Explique em poucas linhas a ideia central da sua arquitetura.

**Sua resposta:**

```md
A arquitetura proposta utiliza serviços AWS para armazenar, processar, organizar e disponibilizar os documentos corporativos em uma Wiki inteligente.

Os arquivos originais seriam armazenados no Amazon S3 e processados de acordo com seu formato, utilizando AWS Step Functions para orquestrar o fluxo, Amazon Textract para OCR, AWS Glue para os dados estruturados e AWS Lambda para extração e normalização. Os documentos processados seriam utilizados pelo Amazon Bedrock Knowledge Bases para gerar embeddings, realizar busca semântica e fornecer os conteúdos relevantes ao Amazon Bedrock para geração das respostas.

O acesso à Wiki seria feito por uma interface web utilizando AWS Amplify e Amazon Cognito, com API Gateway e Lambda no backend. A solução também utilizaria IAM, KMS, CloudTrail, CloudWatch, Macie e Cost Explorer para controle de acesso, proteção dos dados, auditoria, monitoramento e acompanhamento dos custos.```

---

## 2. Serviços AWS utilizados

| Serviço AWS | Papel na solução |
|---|---|
| Amazon S3 | Armazenar os arquivos originais (raw/) e os documentos processados, mantendo os dados disponíveis para processamento e consulta. |
| Amazon Textract | Realizar o OCR dos documentos em formato de imagem, como o PNG, extraindo textos e informações estruturadas, como tabelas. |
| Amazon Bedrock | Utilizar modelos de IA para analisar os conteúdos e gerar respostas em linguagem natural a partir das informações recuperadas. |
| Amazon Bedrock Knowledge Bases | Organizar os documentos processados, realizar o chunking e a geração de embeddings, além de permitir a busca semântica pelos conteúdos relevantes para responder às perguntas. |
| AWS Lambda | Executar códigos específicos da solução, como extração e normalização de conteúdos e processamento das requisições feitas pela interface da Wiki. |
| AWS Step Functions | Orquestrar o fluxo de processamento dos arquivos, controlando a sequência das etapas, as diferentes rotas de processamento e o tratamento de erros. |
| Amazon CloudWatch | Monitorar logs, métricas e erros dos serviços, além de permitir a criação de alertas para situações que precisem de atenção. |
| AWS IAM | Controlar as permissões de usuários e recursos, garantindo que cada usuário ou serviço tenha apenas os acessos necessários. |
| AWS KMS | Proteger os dados por meio de criptografia, principalmente os documentos armazenados no Amazon S3. |

Adicione, remova ou ajuste os serviços conforme sua proposta.

---

## 3. Fluxo de dados de ponta a ponta

Descreva o caminho dos dados desde a pasta `raw/` até a Wiki Inteligente.

```md
Exemplo de estrutura:

1. Arquivos estão inicialmente na pasta raw/
2. Arquivos são enviados para o Amazon S3
3. Documentos escaneados passam pelo Amazon Textract
4. Arquivos digitais têm seus textos extraídos
5. Textos são limpos e padronizados
6. Metadados são extraídos
7. Conteúdos são indexados em uma base pesquisável
8. Usuário pesquisa na Wiki
9. IA responde com base nos documentos originais
```

**Sua resposta:**

```md
1 Os arquivos começam na pasta `raw/`, onde estão os documentos originais em PDF, PNG e CSV.
2 Esses arquivos são enviados para um bucket do Amazon S3, onde ficam armazenados sem alterações.
3 O AWS Step Functions organiza o processamento e identifica qual tipo de tratamento cada arquivo precisa.
4 O PNG passa pelo Amazon Textract, que faz o OCR e transforma a imagem em texto e informações estruturadas.
5 O PDF, por já possuir texto digital, tem seu conteúdo extraído diretamente, sem precisar de OCR.
6 O CSV é processado pelo AWS Glue para organizar os dados da tabela.
7 Depois disso, os conteúdos são tratados e padronizados usando AWS Lambda, junto com os metadados de cada documento.
8 Os conteúdos já tratados são armazenados no Amazon S3 e enviados para o Amazon Bedrock Knowledge Bases.
9 O Knowledge Bases divide os documentos em partes menores, gera os embeddings e os armazena em uma base vetorial, permitindo fazer buscas por significado.
10 O usuário acessa a Wiki por uma interface web feita com AWS Amplify e realiza o login usando o Amazon Cognito.
11 Quando o usuário faz uma pergunta, ela passa pelo Amazon API Gateway e chega a uma função AWS Lambda.
12 A Lambda consulta o Amazon Bedrock Knowledge Bases, que procura os trechos mais relevantes para aquela pergunta.
13 Esses trechos são enviados para o Amazon Bedrock, que usa as informações encontradas para gerar a resposta.
14 A resposta volta para a Wiki junto com as fontes utilizadas, permitindo que o usuário veja de quais documentos as informações foram retiradas.
```

---

## 4. Diagrama textual da arquitetura

Crie um diagrama simples usando texto.

```md
Exemplo:

raw/ → Amazon S3 → Lambda/Step Functions → Textract → S3 Processado → Bedrock Knowledge Bases → Interface de Consulta → Usuário Final
```

**Sua resposta:**

```md
raw/
  ↓
Amazon S3
  ↓
Step Functions
  ↓
PDF → extração de texto
PNG → Textract (OCR)
CSV → Glue
  ↓
Lambda → tratamento e organização
  ↓
S3 Processado
  ↓
Bedrock Knowledge Bases
  ↓
Chunks + Embeddings + Busca vetorial
  ↓
Amazon Bedrock
  ↓
API Gateway + Lambda
  ↓
Amplify + Cognito
  ↓
Usuário```

---

## 5. Riscos e limitações

Liste possíveis desafios da sua solução.

```md
Exemplo:
- Documentos ilegíveis podem prejudicar a extração de texto.
- OCR pode gerar erros em documentos com baixa qualidade.
- Custos podem aumentar conforme o volume de documentos.
- Metadados inferidos por IA podem precisar de validação humana.
- Respostas geradas por IA devem sempre referenciar documentos de origem.
```

**Sua resposta:**

```md
Documentos com baixa qualidade ou ilegíveis podem prejudicar a extração de texto pelo OCR.

O OCR pode apresentar erros na leitura de datas, números e siglas.

Documentos com informações desnecessárias ou repetidas podem atrapalhar a extração e a normalização dos conteúdos.

Os custos podem aumentar conforme a quantidade de documentos processados e o número de consultas realizadas.

As respostas geradas pela IA podem conter erros, por isso devem ser baseadas nos documentos recuperados e apresentar as fontes utilizadas.

O controle de permissões precisa ser configurado corretamente para evitar acesso indevido aos documentos.```

---

## 6. Melhorias futuras

Descreva como a solução poderia evoluir.

```md
Exemplo:
- Criar uma interface web para consulta.
- Criar um chat interno para perguntas sobre atas.
- Adicionar controle de acesso por departamento.
- Criar dashboard de decisões e pendências.
- Gerar alertas automáticos sobre ações em aberto.
- Integrar com ferramentas corporativas.
```

**Sua resposta:**

```md
Aprofundar o conhecimento sobre o AWS Step Functions para automatizar cada vez mais os processos da solução, como o controle de acesso, o processamento e o tratamento de erros.

Implementar regras mais avançadas de controle de acesso, permitindo adaptar as permissões de acordo com o departamento ou função do usuário.

Expandir a arquitetura para suportar novos tipos de documentos e novos processos de tratamento.

Criar mecanismos para acompanhar a qualidade das respostas e identificar possíveis melhorias na busca e na organização dos conteúdos.

Integrar a Wiki com outras ferramentas corporativas, permitindo que ela faça parte dos processos já utilizados pela empresa.```

---

# 🧠 Checklist Final

Antes de entregar, confirme se sua solução responde:

- [ ] Como transformar documentos escaneados em texto?
- [ ] Como lidar com diferentes formatos dentro da mesma pasta `raw/`?
- [ ] Como armazenar os documentos originais?
- [ ] Como preservar a rastreabilidade entre resposta e documento fonte?
- [ ] Como organizar metadados?
- [ ] Como criar busca semântica?
- [ ] Como usar Amazon Bedrock na solução?
- [ ] Como proteger documentos sensíveis?
- [ ] Como monitorar falhas?
- [ ] Como a empresa usaria essa Wiki no dia a dia?

---

# 🏁 Conclusão

Escreva uma breve conclusão defendendo sua solução como se estivesse apresentando para uma liderança técnica ou de negócio.

**Sua resposta:**

```md
A solução proposta utiliza serviços AWS de forma integrada para transformar documentos corporativos em uma base de conhecimento pesquisável e acessível por meio de uma Wiki Inteligente.

A arquitetura permite automatizar o processamento dos diferentes formatos de arquivos, realizar buscas semânticas e gerar respostas baseadas nos documentos disponíveis, mantendo a rastreabilidade das informações. Além disso, recursos de segurança, auditoria e monitoramento ajudam a proteger os dados e acompanhar o funcionamento da solução.

A arquitetura também pode evoluir conforme a necessidade da empresa, principalmente com o aprofundamento do uso do AWS Step Functions e de outros serviços AWS, permitindo automatizar novos processos e tornar a solução mais completa e escalável.```
