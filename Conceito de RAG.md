## Conceitos de RAG com LangChain e LangGraph

### LLM's 

Um grande modelo de linguagem (do inglês large language model - LLM) é um modelo computacional projetado para realizar tarefas de processamento de linguagem natural, especialmente a geração de linguagem, usando relações contextuais derivadas de um grande conjunto de dados de treinamento. Os LLMs podem gerar, resumir, traduzir e analisar texto em uma variedade de contextos, e são a base tecnológica dos chatbots modernos. Os LLMs podem imitar com precisão os padrões da linguagem natural porque são treinados em coleções de texto escrito por humanos. Pelo mesmo motivo, dados de treinamento enviesados ou imprecisos podem tornar a saída de um LLM menos confiável.

O fluxo de uso de uma LLM é relativamente simples: fazemos uma pergunta ou solicitação, chamada de prompt, e o modelo utiliza os padrões que aprendeu durante seu treinamento, juntamente com o contexto da nossa pergunta, para construir uma resposta. Mas e se o modelo não tiver conhecimento suficiente para responder corretamente? O que podemos fazer?

### Fine-Tuning VS RAG

O `Fine-Tuning` é uma técnica que consiste em realizar um treinamento adicional em um modelo de linguagem já existente, utilizando dados específicos para adaptá-lo a determinadas tarefas ou necessidades.

O `RAG (Retrieval-Augmented Generation)` por sua vez, ao invés de ensinar ao modelo, eu conecto ele ao conhecimento necessário para que ele possa me dar a resposta necessária, como uma base de dados externa ou uma API por exemplo. Desta forma eu tenho um modelo mais generalista mas que busca o que é necessário, reduzindo o custo de treinamento.

Cada uma das abordagens tem suas vantagens e desvantagens. Se eu quiser padronizar as respostas, tom, estilo e formato o fine-tuning é perfeito para este caso. Se eu quiser atualizar o conhecimento da LLM com frequência ou quiser utilizar documentos e políticas internas de uma instituição, o RAG é a melhor escolha.

### Arquitetura Geral do RAG

O usuário faz uma pergunta e o LLM, que pode ser um agente de IA, busca em documentos conectados a ele, como PDFs, Google Sheets, Docs ou bases de dados. Ele realiza essa busca, consulta e traz as informações relevantes desses documentos, reprocessando-as para gerar a resposta final ao usuário. Isso significa que podemos fornecer nossos dados ao LLM, que podem incluir regras de home office, dados de faturamento de vendas de consultores, informações de atendimento ou regras de benefícios de uma empresa. Esses dados são disponibilizados ao modelo como informações adicionais durante a consulta, permitindo que ele responda perguntas sobre conteúdos específicos que não necessariamente fizeram parte do seu treinamento.

O fluxo na prática é o seguinte: o usuário faz a pergunta, o modelo não possui aquela informação. Precisamos ampliar o contexto do modelo para que ele possa fornecer uma resposta coerente. O que ele faz? Primeiro, precisamos preparar os documentos que serão utilizados como fontes de conhecimento. Essas informações são transformadas em representações numéricas, chamadas de embeddings, que podem ser armazenadas em um banco de dados vetorial, conhecido como vector store.

Quando o usuário faz uma pergunta, o sistema consulta essa base para encontrar informações relacionadas à solicitação. Os dados encontrados são enviados à LLM como contexto, permitindo que ela construa uma resposta fundamentada nas informações disponibilizadas.

### Prompt Tradicional x Prompt com RAG

Ao utilizarmos prompts tradicionais, a LLM normalmente constrói suas respostas com base no conhecimento adquirido durante seu treinamento e nas informações que fornecemos diretamente na conversa. Isso pode resultar em respostas incorretas ou imprecisas, principalmente quando perguntamos sobre assuntos muito específicos ou informações às quais o modelo não teve acesso.

Com o RAG, podemos fornecer informações externas relevantes durante a consulta, permitindo que o modelo produza respostas mais contextualizadas e fundamentadas. Embora isso não elimine completamente o risco de erros, pode aumentar consideravelmente a confiabilidade das respostas.

Utilizando o LangChain para organizar esse processo, conseguimos conectar o modelo às nossas fontes de conhecimento e construir soluções voltadas para problemas reais de negócio.

Vamos utilizar como exemplo a necessidade de validar se um funcionário pode retirar sua cesta de Natal em uma empresa.

Se utilizarmos uma LLM comum, como ChatGPT, Copilot ou Gemini, sem fornecer informações adicionais, ela não terá acesso aos dados internos da empresa nem às regras necessárias para realizar essa validação. Dessa forma, a LLM poderá fornecer uma resposta genérica, informar que não possui informações suficientes ou até mesmo gerar uma resposta incorreta, sem embasamento nos dados reais.

É aí que entra o RAG.

Ao utilizarmos o RAG, podemos conectar nosso sistema a uma base de conhecimento externa que contenha as informações necessárias para realizar essa validação, como a lista de funcionários autorizados e as regras para retirada das cestas de Natal.

Assim, quando perguntarmos se determinado funcionário pode retirar sua cesta, o sistema buscará as informações relevantes nessa base e as fornecerá à LLM como contexto, permitindo que ela construa uma resposta fundamentada nos dados da empresa.

É claro que o RAG não elimina completamente o risco de alucinações ou respostas incorretas, mas pode reduzi-lo significativamente, tornando as respostas mais contextualizadas, coerentes e confiáveis.