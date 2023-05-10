# Few-Shot-Editais-TI

Estes notebooks comparam o few-shot do GPT3 (ada, babbage e currie), GPT3.5 (Davinci) e [SetFit](https://github.com/huggingface/setfit) utilizando o modelo [sentence-transformers-ult5-pt-small](tgsc/sentence-transformer-ult5-pt-small) e [paraphrase-multilingual-mpnet-base-v2](sentence-transformers/paraphrase-multilingual-mpnet-base-v2). O modelo [ult5-pt-small](tgsc/ult5-pt-small) com fine-tune.

Os modelos utilizados da OpenAI foram: text-ada-001; text-babbage-001; text-curie-001; text-davinci-003; e gpt-3.5-turbo. Foi utilizado 'temperature' = 0 para usarmos *greedy decoding* e remover a aleatoriedade das respostas. Os modelos 'ada', 'babbage' e 'curie' não retornaram classificações direito.

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
| Ada - 12 shot                                                                                                                           | -          | 0.55     | 0.78      | 0.534        | 0.32                  | 0.592                    |
| Ada (2o prompt) - 12 shot                                                                                                               | -          | 0.56     | 0.6       | 0.555        | 0.52                  | 0.565                    |
| Baggage - 12 shot                                                                                                                       | 1B         | 0.6      | 0.96      | 0.558        | 0.24                  | 0.857                    |
| Baggage - (2o prompt) - 12 shot                                                                                                         | 1B         | 0.62     | 0.96      | 0.571        | 0.28                  | 0.875                    |
| Curie - 12 shot                                                                                                                         | 6.7B       | 0.55     | 0.96      | 0.527        | 0.14                  | 0.77                     |
| Curie (2o prompt) - 12 shot                                                                                                             | 6.7B       | 0.52     | 0.9       | 0.511        | 0.14                  | 0.583                    |
| [sentence-transformer-ult5-pt-small](tgsc/sentence-transformer-ult5-pt-small)   - 12 shot                                               | 51M        | 0.84     | 0.8       | 0.869        | 0.88                  | 0.814                    |
| [paraphrase-multilingual-mpnet-base-v2](https://huggingface.co/sentence-transformers/paraphrase-multilingual-mpnet-base-v2)   - 12 shot | 278M       | 0.86     | 0.8       | 0.909        | 0.92                  | 0.821                    |
| ChatGPT - zero shot                                                                                                                     | 175B       | 0.91     | 0.86      | 0.955        | 0.96                  | 0.872                    |
| Davinci-003 - 12 shot                                                                                                                   | 175B       | 0.95     | 0.96      | 0.941        | 0.94                  | 0.959                    |
| ult5-pt-small - 6535 fine-tune                                                                                                          | 82.4M      | 0.97     | 0.96      | 0.979        | 0.98                  | 0.96                     |



## Observações e comentários

- Seria interessante saber a partir de qual tamanho de rede ultrapassria a performance do SetFit, mas não existe modelo da OpenAI disponível intermediário entre 6.7B e 175B. 

- Após a realização dos testes, como o set de validação foi selecionado aleatoriamente, um dos exemplos está classificando errado: 'Intenção de Registrar Preços para eventual aquisição de material - CONSUMO - UNIFORME ESCOLAR - Processo Original nº 23305.003766.2018-54.' = 'Tecnologia da informação'. Na prática, todos os modelos ganham 1% de acurácia e há um pequeno aumento em todas as métricas. Vou deixar da forma como está, sem efetuar os recálculos, para manter os mesmos resultados dos notebooks.

- O modelo Curie, apesar de ser 6 vezes o tamanho do baggage, possui desempenho pior nesse dataset específico. Mesmo com algumas tentativas de mudança de prompt, o desempenho piorou. Além disso, tentar o prompt com letras minúsculas piorou o resultado nos 3 modelos GPT3 (ada, babage e curie). Os outros prompts testados tiveram pior performance, inclusive substituir as expressões 'Tecnologia da informação' por 'informática'. Como cada tentativa é paga, manteve-se os mesmos prompts do notebook que funcionaram com o DaVinci, Ada e Babbage. 

- Infelizmente não sei quem criou o dataset para os devidos créditos.

- Agradecimentos ao Ricardo Akl, Sylvio e [Edans](https://github.com/edanssandes), por quem tive conhecimento do dataset, e ao Edans também pela criação do [notebook de classificação zero-shot do ChatGPT]((https://github.com/edanssandes/LicitacoesChatGPT/blob/main/Classificacao.ipynb) no qual o notebook Classificacao-ChatGPT-Zero-Shot.ipynb foi inteiramente baseado.

- Os prompts do GPT3 foram feitos da forma tradicional:
```
Detalhamento da instrução:
Exemplo => Classe
Exemplo => Classe
Exemplo a ser classificado =>
```
