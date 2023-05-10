# Few-Shot-Editais-TI

Estes notebooks comparam o few-shot do GPT3 (ada, babbage e currie), GPT3.5 (Davinci) e [SetFit](https://github.com/huggingface/setfit) utilizando o modelo [sentence-transformers-ult5-pt-small](tgsc/sentence-transformer-ult5-pt-small) e [paraphrase-multilingual-mpnet-base-v2](sentence-transformers/paraphrase-multilingual-mpnet-base-v2), com zero-shot do ChatGPT. O modelo [ult5-pt-small](tgsc/ult5-pt-small) com fine-tune.

Os modelos utilizados da OpenAI foram: 'text-ada-001'; 'text-babbage-001'; 'text-curie-001'; 'text-davinci-003'; e 'gpt-3.5-turbo'. Foi utilizado 'temperature' = 0 para usarmos *greedy decoding* e remover a aleatoriedade das respostas. Os modelos 'ada', 'babbage' e 'curie', sem o 'text-{modelo}-001', não retornaram classificações direito.

O dataset consiste em objetos de editais de licitação, classificados em 'Tecnologia da informação', se o objeto se referir a essa área, e 'Outras compras' para todo o resto (obras, saúde, escritório, automóveis, etc). Foram separados 100 exemplos aleatórios de validação (reproduzidos em todo o notebook com o seeds), com 50 exemplos de cada classe. Se decidiu por 100, pois o aumento do número de exemplos começa a ficar proibitivo para o teste com o davinci. Para o davinci, 100 exemplos custa perto de $3.5 doláres, então o dataset inteiro passaria dos $160 (quase mil reais). 

Utilizou-se 12 shots, 6 exemplos de cada classe. Para o gpt-3.5-turbo (ChatGPT) utilizou-se zero-shot. Por fim, foi feito o fine-tune do modelo [ult5-pt-small](tgsc/sentence-transformer-ult5-pt-small) com o restante (6535 exemplos). A tabela de resultados se encontra abaixo.

| Notebook | Colab |
|-----------------------------------|---------------|
| Classificação-SetFit-Few-shot.ipynb | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/thacio/Few-Shot-Editais-TI/blob/main/Classifica%C3%A7%C3%A3o-SetFit-Few-shot.ipynb) |
| Classificação-Finetunning-ult5.ipynb | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/thacio/Few-Shot-Editais-TI/blob/main/Classifica%C3%A7%C3%A3o-Finetunning-ult5.ipynb) |
| Classificacao-GPT3e3.5-Few-Shot.ipynb | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/thacio/Few-Shot-Editais-TI/blob/main/Classificacao-GPT3e3.5-Few-Shot.ipynb) |
| Classificacao-ChatGPT-Zero-Shot.ipynb | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/thacio/Few-Shot-Editais-TI/blob/main/Classificacao-ChatGPT-Zero-Shot.ipynb) |


| Modelo - Qtd. Shots                                                                                                                     | Parâmetros | Acuraccy | Recall TI | Precision TI | Recall Outras compras | Precision Outras compras |
|-----------------------------------------------------------------------------------------------------------------------------------------|------------|----------|-----------|--------------|-----------------------|--------------------------|
| Curie (4o prompt) - 12 shot                                                                                                             | 6.7B       | 0.5      | 1         | 0.5          | 0                     | 0                        |
| Baggage (4o prompt) - 12 shot                                                                                                           | 1B         | 0.5      | 0.98      | 0.5          | 0.02                  | 0.5                      |
| Curie (3o prompt) - 12 shot                                                                                                             | 6.7B       | 0.51     | 1         | 0.505        | 0.02                  | 1                        |
| Baggage (3o prompt) - 12 shot                                                                                                           | 1B         | 0.53     | 0.98      | 0.515        | 0.08                  | 0.8                      |
| Ada (1o prompt) - 12 shot                                                                                                               | -          | 0.55     | 0.78      | 0.534        | 0.32                  | 0.592                    |
| Ada (2o prompt) - 12 shot                                                                                                               | -          | 0.56     | 0.6       | 0.555        | 0.52                  | 0.565                    |
| Baggage (1o prompt) - 12 shot                                                                                                           | 1B         | 0.6      | 0.96      | 0.558        | 0.24                  | 0.857                    |
| Baggage    (2o prompt) - 12 shot                                                                                                        | 1B         | 0.62     | 0.96      | 0.571        | 0.28                  | 0.875                    |
| Curie (1o prompt) - 12 shot                                                                                                             | 6.7B       | 0.55     | 0.96      | 0.527        | 0.14                  | 0.77                     |
| Curie (2o prompt) - 12 shot                                                                                                             | 6.7B       | 0.52     | 0.9       | 0.511        | 0.14                  | 0.583                    |
| Ada (3o prompt) - 12 shot                                                                                                               | -          | 0.72     | 0.92      | 0.657        | 0.52                  | 0.866                    |
| Ada (4o prompt) - 12 shot                                                                                                               | -          | 0.74     | 0.9       | 0.681        | 0.58                  | 0.852                    |
| [sentence-transformer-ult5-pt-small](tgsc/sentence-transformer-ult5-pt-small)   - 12 shot                                               | 51M        | 0.84     | 0.8       | 0.869        | 0.88                  | 0.814                    |
| [paraphrase-multilingual-mpnet-base-v2](https://huggingface.co/sentence-transformers/paraphrase-multilingual-mpnet-base-v2)   - 12 shot | 278M       | 0.86     | 0.8       | 0.909        | 0.92                  | 0.821                    |
| ChatGPT - zero shot                                                                                                                     | 175B       | 0.91     | 0.86      | 0.955        | 0.96                  | 0.872                    |
| Davinci-003 (1o prompt) - 12 shot                                                                                                       | 175B       | 0.95     | 0.96      | 0.941        | 0.94                  | 0.959                    |
| ult5-pt-small - 6535 fine-tune                                                                                                          | 82.4M      | 0.97     | 0.96      | 0.979        | 0.98                  | 0.96                     |



## Observações e comentários

- Seria interessante saber a partir de qual tamanho de rede ultrapassaria a performance do SetFit, mas não existe modelo da OpenAI disponível intermediário entre 6.7B e 175B. 

- Após a realização dos testes, como o set de validação foi selecionado aleatoriamente, um dos exemplos está classificado errado: 'Intenção de Registrar Preços para eventual aquisição de material - CONSUMO - UNIFORME ESCOLAR - Processo Original nº 23305.003766.2018-54.' = 'Tecnologia da informação'. Na prática, todos os modelos ganham 1% de acurácia e há um pequeno aumento em todas as métricas. Vou deixar da forma como está, sem efetuar os recálculos, para manter os mesmos resultados dos notebooks.

- O modelo Curie, apesar de ser 6 vezes o tamanho do baggage, possui desempenho pior nesse dataset específico. Mesmo com algumas tentativas de mudança de prompt, o desempenho piorou. Além disso, tentar o prompt com letras minúsculas piorou o resultado nos 3 modelos GPT3 (ada, babage e curie). Os outros prompts testados tiveram pior performance, inclusive substituir as expressões 'Tecnologia da informação' por 'informática'. Como cada tentativa é paga, manteve-se os mesmos prompts do notebook que funcionaram com o DaVinci, Ada e Babbage. 

- Infelizmente não sei quem criou o dataset para os devidos créditos.

- Agradecimentos ao Ricardo Akl, Sylvio e [Edans](https://github.com/edanssandes), por quem tive conhecimento do dataset, e ao Edans também pela criação do [notebook de classificação zero-shot do ChatGPT]((https://github.com/edanssandes/LicitacoesChatGPT/blob/main/Classificacao.ipynb) no qual o notebook Classificacao-ChatGPT-Zero-Shot.ipynb foi inteiramente baseado.

## Prompts do GPT3
Os prompts 1 e 2 foram feitos da forma tradicional:
```
Detalhamento da instrução:
Exemplo => Classe
Exemplo => Classe
Exemplo a ser classificado =>
```

## Prompt 1
```
Vou apresesentar exemplos de objetos de licitação e você classificará o objeto em 'Tecnologia da Informação', caso o objeto se refira a licitação de itens de Tecnologia da informação ou materiais e equipamentos de informática, ou 'Outras compras' para qualquer outro tipo de compra que não seja TI, como escritório, obras, saúde, entre outras.
O último exemplo estará vazio para que você realize a classificação.
Apresente apenas a classificação nessa duas classes, sem qualquer outra palavra adicional.
Exemplos:
Prestação de serviços de operação e suporte da Central de Atendimento de Telecomunicações do Senado Federal, realizados por equipe técnica residente, nas dependências da Coordenação de Telecomunicações   COOTELE, durante 12 (doze) meses consecutivos, de acordo com as quantidades, periodicidade, especificações, obrigações e demais condições do edital e seus anexos. => Outras compras
Aquisição de uma Solução de Gestão Jurídica, com licenças de uso perpétuo, em conjunto com serviços de implementação (planejamento, instalação, implantação da Solução e migração de dados), serviço de captura de processos, andamentos e publicações, manutenção/suporte técnico (incluindo atualização de versão), treinamento, consultoria/ manutenção evolutiva e implantação de funcionalidades adicionais, conforme as especificações do Edital e de seus Anexos => Tecnologia da Informação
Registro de preços para eventual aquisição de rastreadores portáteis com tecnologia GPS (Global Positioning System) => Tecnologia da Informação
Contratação de serviços continuados de analise, validação e transcrição de dados e eventos atualmente registrados em imagens e microfilmes que integram o acervo de documentos Meteorológicos do Instituto Nacional de Meteorologia - INMET, contemplando a prestação de serviços de digitação, visando a atualização do Banco de Dados Meteorológico conforme condições, quantidades e exigências estabelecidas no presente Edital e seus anexos => Tecnologia da Informação
Contratação de serviços continuados de apoio técnico especializado, suporte e atendimento aos usuários dos recursos de Tecnologia da Informação => Tecnologia da Informação
Registro de Preço para aquisição de ESPARGIDORES LACRIMOGÊNEOS PIMENTA (OC), na versão para uso coletivo em Operações de Controle de Distúrbios - OCD, e também em outro modelo, de uso individual, para porte do Policial Rodoviário Federal; conforme condições, quantidades, exigências e estimativas, inclusive as encaminhadas pelos órgãos e entidades participantes, estabelecidas no Termo de Referência. => Outras compras
Registro de preço para possível aquisição de insumos   pó de brita, brita 0 e brita 1 a serem empregados na Obra de Pavimentação de Logradouros, no município de Araguari-MG e  pó de brita, brita 0, brita 1 e brita 2 e pedra de mão a serem empregados na Obra de Construção de Infraestrutura para Hangares na Base Aérea de São Pedro da Aldeia/RJ => Outras compras
Registro de Preços, pelo prazo de até 12 meses, para eventual aquisição de Materiais da marca Sigma para atender as necessidades de Bio Manguinhos. => Outras compras
Contratação de serviços de coleta, transporte, tratamento e destinação final de resíduos sólidos de saúde (lixo hospitalar) para o Hospital Geral de Belém. => Outras compras
Solução comunicação privada e corporativa com fornecimento de software do ambiente colaborativo e de smartphone com licença de uso do ambiente colaborativo, que garanta a inviolabilidade da comunicação e permita a contribuição entre usuários usando mensagens de texto, voz, vídeo e documentos. => Tecnologia da Informação
Pregão destinado para eventual aquisição de material de higiene e limpeza => Outras compras
Registro de Preços para eventual aquisição de no-break s destinados às diversas Unidades Administrativas do Banco do Nordeste, incluindo a instalação, configuração e testes, bem como a assistência técnica durante o período de garantia. ITEM 1   No-break 3kVA; ITEM 2   No-break 10kVA. => Tecnologia da Informação
[Objeto a ser classificado] =>
```

## Prompt 2
Não salvei o prompt, mas a mudança foi trocar os tremos de 'Tecnologia da informação' por informática;

## Prompt 3
Os prompts 3 e 4 foram feitos de acordo com os exemplos mostrado ao final do paper original do GPT3 - [Language Models are Few-Shot Learners](https://arxiv.org/pdf/2005.14165.pdf).

```
Detalhamento da instrução:
Exemplo: [objeto de exemplo]
Classe: [Classificação]
Exemplo: [objeto de exemplo]
Classe: [Classificação]
Exemplo: [objeto a ser classificado]
Classe:
```

```
Vou apresesentar exemplos de objetos de licitação e você classificará o objeto em 'Tecnologia da Informação', caso o objeto se refira a licitação de itens de Tecnologia da informação ou materiais e equipamentos de informática, ou 'Outras compras' para qualquer outro tipo de compra que não seja TI, como escritório, obras, saúde, entre outras.
A classificação deve ser em apenas uma dessas duas classes, sem qualquer outra palavra adicional.
Exemplo: Prestação de serviços de operação e suporte da Central de Atendimento de Telecomunicações do Senado Federal, realizados por equipe técnica residente, nas dependências da Coordenação de Telecomunicações   COOTELE, durante 12 (doze) meses consecutivos, de acordo com as quantidades, periodicidade, especificações, obrigações e demais condições do edital e seus anexos. 
Classe: Outras compras
Exemplo: Aquisição de uma Solução de Gestão Jurídica, com licenças de uso perpétuo, em conjunto com serviços de implementação (planejamento, instalação, implantação da Solução e migração de dados), serviço de captura de processos, andamentos e publicações, manutenção/suporte técnico (incluindo atualização de versão), treinamento, consultoria/ manutenção evolutiva e implantação de funcionalidades adicionais, conforme as especificações do Edital e de seus Anexos 
Classe: Tecnologia da Informação
Exemplo: Registro de preços para eventual aquisição de rastreadores portáteis com tecnologia GPS (Global Positioning System) 
Classe: Tecnologia da Informação
Exemplo: Contratação de serviços continuados de analise, validação e transcrição de dados e eventos atualmente registrados em imagens e microfilmes que integram o acervo de documentos Meteorológicos do Instituto Nacional de Meteorologia - INMET, contemplando a prestação de serviços de digitação, visando a atualização do Banco de Dados Meteorológico conforme condições, quantidades e exigências estabelecidas no presente Edital e seus anexos 
Classe: Tecnologia da Informação
Exemplo: Contratação de serviços continuados de apoio técnico especializado, suporte e atendimento aos usuários dos recursos de Tecnologia da Informação 
Classe: Tecnologia da Informação
Exemplo: Registro de Preço para aquisição de ESPARGIDORES LACRIMOGÊNEOS PIMENTA (OC), na versão para uso coletivo em Operações de Controle de Distúrbios - OCD, e também em outro modelo, de uso individual, para porte do Policial Rodoviário Federal; conforme condições, quantidades, exigências e estimativas, inclusive as encaminhadas pelos órgãos e entidades participantes, estabelecidas no Termo de Referência. 
Classe: Outras compras
Exemplo: Registro de preço para possível aquisição de insumos   pó de brita, brita 0 e brita 1 a serem empregados na Obra de Pavimentação de Logradouros, no município de Araguari-MG e  pó de brita, brita 0, brita 1 e brita 2 e pedra de mão a serem empregados na Obra de Construção de Infraestrutura para Hangares na Base Aérea de São Pedro da Aldeia/RJ 
Classe: Outras compras
Exemplo: Registro de Preços, pelo prazo de até 12 meses, para eventual aquisição de Materiais da marca Sigma para atender as necessidades de Bio Manguinhos. 
Classe: Outras compras
Exemplo: Contratação de serviços de coleta, transporte, tratamento e destinação final de resíduos sólidos de saúde (lixo hospitalar) para o Hospital Geral de Belém. 
Classe: Outras compras
Exemplo: Solução comunicação privada e corporativa com fornecimento de software do ambiente colaborativo e de smartphone com licença de uso do ambiente colaborativo, que garanta a inviolabilidade da comunicação e permita a contribuição entre usuários usando mensagens de texto, voz, vídeo e documentos. 
Classe: Tecnologia da Informação
Exemplo: Pregão destinado para eventual aquisição de material de higiene e limpeza 
Classe: Outras compras
Exemplo: Registro de Preços para eventual aquisição de no-break s destinados às diversas Unidades Administrativas do Banco do Nordeste, incluindo a instalação, configuração e testes, bem como a assistência técnica durante o período de garantia. ITEM 1   No-break 3kVA; ITEM 2   No-break 10kVA. 
Classe: Tecnologia da Informação
Exemplo: {Objeto a ser classificado}
Classe:
```

## Prompt 4

```
Vou apresesentar exemplos de objetos de licitação e você classificará o objeto em 'Tecnologia da Informação', caso o objeto se refira a licitação de itens de Tecnologia da informação ou materiais e equipamentos de informática, ou 'Outras compras' para qualquer outro tipo de compra que não seja TI, como escritório, obras, saúde, entre outras.
A classificação deve ser em apenas uma dessas duas classes, sem qualquer outra palavra adicional.
Objeto: Prestação de serviços de operação e suporte da Central de Atendimento de Telecomunicações do Senado Federal, realizados por equipe técnica residente, nas dependências da Coordenação de Telecomunicações   COOTELE, durante 12 (doze) meses consecutivos, de acordo com as quantidades, periodicidade, especificações, obrigações e demais condições do edital e seus anexos. 
Classificação: Outras compras
Objeto: Aquisição de uma Solução de Gestão Jurídica, com licenças de uso perpétuo, em conjunto com serviços de implementação (planejamento, instalação, implantação da Solução e migração de dados), serviço de captura de processos, andamentos e publicações, manutenção/suporte técnico (incluindo atualização de versão), treinamento, consultoria/ manutenção evolutiva e implantação de funcionalidades adicionais, conforme as especificações do Edital e de seus Anexos 
Classificação: Tecnologia da Informação
Objeto: Registro de preços para eventual aquisição de rastreadores portáteis com tecnologia GPS (Global Positioning System) 
Classificação: Tecnologia da Informação
Objeto: Contratação de serviços continuados de analise, validação e transcrição de dados e eventos atualmente registrados em imagens e microfilmes que integram o acervo de documentos Meteorológicos do Instituto Nacional de Meteorologia - INMET, contemplando a prestação de serviços de digitação, visando a atualização do Banco de Dados Meteorológico conforme condições, quantidades e exigências estabelecidas no presente Edital e seus anexos 
Classificação: Tecnologia da Informação
Objeto: Contratação de serviços continuados de apoio técnico especializado, suporte e atendimento aos usuários dos recursos de Tecnologia da Informação 
Classificação: Tecnologia da Informação
Objeto: Registro de Preço para aquisição de ESPARGIDORES LACRIMOGÊNEOS PIMENTA (OC), na versão para uso coletivo em Operações de Controle de Distúrbios - OCD, e também em outro modelo, de uso individual, para porte do Policial Rodoviário Federal; conforme condições, quantidades, exigências e estimativas, inclusive as encaminhadas pelos órgãos e entidades participantes, estabelecidas no Termo de Referência. 
Classificação: Outras compras
Objeto: Registro de preço para possível aquisição de insumos   pó de brita, brita 0 e brita 1 a serem empregados na Obra de Pavimentação de Logradouros, no município de Araguari-MG e  pó de brita, brita 0, brita 1 e brita 2 e pedra de mão a serem empregados na Obra de Construção de Infraestrutura para Hangares na Base Aérea de São Pedro da Aldeia/RJ 
Classificação: Outras compras
Objeto: Registro de Preços, pelo prazo de até 12 meses, para eventual aquisição de Materiais da marca Sigma para atender as necessidades de Bio Manguinhos. 
Classificação: Outras compras
Objeto: Contratação de serviços de coleta, transporte, tratamento e destinação final de resíduos sólidos de saúde (lixo hospitalar) para o Hospital Geral de Belém. 
Classificação: Outras compras
Objeto: Solução comunicação privada e corporativa com fornecimento de software do ambiente colaborativo e de smartphone com licença de uso do ambiente colaborativo, que garanta a inviolabilidade da comunicação e permita a contribuição entre usuários usando mensagens de texto, voz, vídeo e documentos. 
Classificação: Tecnologia da Informação
Objeto: Pregão destinado para eventual aquisição de material de higiene e limpeza 
Classificação: Outras compras
Objeto: Registro de Preços para eventual aquisição de no-break s destinados às diversas Unidades Administrativas do Banco do Nordeste, incluindo a instalação, configuração e testes, bem como a assistência técnica durante o período de garantia. ITEM 1   No-break 3kVA; ITEM 2   No-break 10kVA. 
Classificação: Tecnologia da Informação
```
