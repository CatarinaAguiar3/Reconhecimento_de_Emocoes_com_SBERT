# Reconhecimento de Emoções em Relatórios Financeiros usando Sentence BERT

# Introdução
Este projeto usa técnicas de Processamento de Linguagem Natural (NLP) para investigar como empresas brasileiras alteram a linguagem de seus relatórios financeiros após a divulgação de más notícias. O objetivo foi identificar a emoção predominante em notas explicativas de 50 empresas de capital aberto.

Para isso, foi aplicada uma abordagem lexical em que o reconhecimento das emoções é feito a partir de uma lista de palavras-chaves associadas a cada emoção. Os textos foram representados por meio de embeddings gerados pela arquitetura Sentence-BERT e a emoção predominante foi determinada a partir da similaridade cosseno entre os vetores.

> **OBS:**
> Este é um resumo da minha iniciação científica e TCC

Esse tipo de análise pode contribuir para estudos sobre transparência corporativa, gerenciamento de impressão e comunicação financeira.

# Por que utilizar o Sentence-BERT?
A maioria dos trabalhos em análise textual financeira utiliza modelos baseados em frequência de palavras (Bag-of-Words ou TF-IDF). Neste projeto foi utilizado o Sentence-BERT, um modelo baseado em Transformers capaz de gerar embeddings contextuais.

Isso permite representar uma mesma palavra de maneiras diferentes dependendo do contexto em que aparece, tornando a comparação entre textos muito mais robusta do que abordagens tradicionais como Word2Vec ou contagem simples de palavras.

Por exemplo, em duas frases contendo a palavra "capital", o SBERT gera vetores distintos de acordo com o contexto, enquanto o Word2Vec atribui um único vetor, independentemente do significado.

<small>Exemplo de Embeddings: SBERT (contextual) vs WordVec (Semântico)</small>
<img src="Figuras\Exemplo de Embeddings -  contextual e semântico.png">

# Dataset
O conjunto de dados é composto por 149 notas explicativas de 50 empresas de capital aberto, divididos em 3 períodos:
- Ano da má notícia
- Ano anterior a má notícia
- 2 anos após a má notícia
  
> OBS: As notas explicativas são uma secção dos relatórios financeiros
> 
O dicionário de emoções (léxico) contém 1.004 termos distribuídos em seis categorias emocionais: Alegria (301), Medo (155), Raiva (136), Repugnância (109), Surpresa (70) e Tristeza (233).

# Tecnologia e Bibliotecas
- Python 
- Regex
- uniecode
- SentenceTransformers 
- Nltk
- spacy
- re
- Pandas 
- NumPy 
- Matplotlib 
- OpenPyXL

# Pipeline

<img src="Figuras\Pipeline.png">

## Etapa 1: Coleta e Limpeza
Os relatórios foram coletados nos sites das empresas e da B3, convertidos para texto e submetidos a uma etapa de limpeza que removeu tabelas, cabeçalhos, rodapés e outros elementos não textuais antes da análise.

<small>Antes e depois da limpeza dos relatórios</small>
<img src="Figuras\Antes e Depois da Limpeza dos Relatórios.png">

## Etapa 2: Análise Descritiva
Usando as bibliotecas NLTK e Spacy, descobriu-se a quantidade de alguns elementos textuais, como número de palavras e sentenças.

## Etapa 3: Criação dos embeddings
A representação vetorial dos textos foi realizada com a arquitetura SBERT. O modelo adotado foi o paraphrase-multilingual MiniLM-L12-v2 da biblioteca SentenceTransformers. Ao aplicar o modelo, cada relatório foi transformado em um embedding e os termos de cada emoção foram convertidos em um único embending.

<small>Transformação de texto em embedding, de forma ilustrada</small>
<img src="Figuras\Transformação de texto em embedding, de forma ilustrada.png">

## Etapa 4: Similaridade Cosseno
A similaridade cosseno foi utilizada para medir a proximidade entre o embedding de cada relatório e os embeddings das seis emoções. A emoção com maior similaridade foi considerada predominante.

<small>Ilustração do cálculo da similaridade cosseno</small>
<img src="Figuras\Ilustração do cálculo da similaridade cosseno.png">

# Resultado

### Predominância de emoções negativas
Foram identificadas quatro emoções predominantes: Repugnância, Raiva, Medo e Surpresa, indicando predominância de emoções negativas nos relatórios analisados. 

<small>Emoções Predominantes nos Relatórios</small>
<img src="Figuras\Gráfico com o Resultado Principal.png">

### Mudanças no discurso corporativo
- 88% das empresas alteraram a emoção predominante entre os períodos analisados. 
- As maiores mudanças ocorreram entre empresas inicialmente classificadas com medo e raiva. 
- A emoção Repugnância apresentou maior estabilidade, sugerindo um padrão consistente de comunicação negativa.

<small> Variação na Contagem das emoções em t e t+2, em relação a t-1 </small>
<img src="Figuras\Variação na Contagem das emoções em t e t+2, em relação a t-1.png">

### Relatórios mais difíceis de serem lidos
Após as más notícias, os relatórios passaram a apresentar:
- maior quantidade de palavras 
- sentenças mais longas 
- maior complexidade textual 
  
<small>Gráficos da Análise Descritiva </small>
<img src="Figuras\Gráficos - Análise Descritiva.png">

Esse padrão aumenta o esforço congnitivo para extrair informações, dificultando a interpretação e a tomada de decisão. E segunda a literatura, essa seria uma maneira de ofuscar as informações relacionadas a má notícia (BLOOMFIELD, 2002, p. 7- 8).

# Interpretação do Resultado
Os resultados sugerem que empresas não modificam apenas o conteúdo divulgado após uma má notícia, mas também a forma como comunicam essas informações. Sendo uma estratégia compatível com o gerenciamento de impressão, no qual organizações ajustam a linguagem utilizada em seus relatórios para influenciar a percepção dos leitores (GINER-SOROLLA ET AL., 2018).

# Limitações e trabalhos futuros
- A amostra contempla apenas 149 relatórios financeiros. 
- Estudos futuros podem ampliar o conjunto de empresas e períodos analisados. 
- Com uma base maior, seria possível aplicar modelos estatísticos e de aprendizado de máquina para investigar a relação entre emoções, tipo de má notícia e gerenciamento de impressões.

# Apêndice: Como funciona o SBERT?
O SBERT é um modelo usado para pesquisa semântica e tarefas não supervisionadas, como agrupamento. Ele é uma modificação do modelo BERT que emprega estruturas de rede siamesas para derivar embeddings de frases (ou vetores de frases). Isso reduz o tempo de cálculo da similaridade de cosseno (REIMERS; GUREVYCH, 2019). 

Uma rede siamesa é um tipo especial de rede neural na qual duas redes idênticas tem os mesmos pesos (REIMERS; GUREVYCH, 2019). Elas são usadas lado a lado para comparar dois elementos.

<small>Arquitetura SBERT para calcular pontuações de similaridade</small>
<img src="Figuras\Arquitetura SBERT.png" width="70%">

Onde, 
- u: embedding da sentença A
- v:  embedding da sentença B
O SBERT opera em cinco etapas principais (REIMERS; GUREVYCH, 2019):
1.	As sentenças A e B são inseridas em dois modelos BERT idênticos com pesos compartilhados. 
2.	Cada cópia do modelo BERT gera uma sequência de embeddings contextuais, um para cada token. Ou seja, ele não produz um único vetor para a frase inteira, mas sim um vetor (embedding) para cada token da sentença. 
3.	Em seguida, uma camada de pooling transforma os vetores de saída do BERT em um único vetor de tamanho fixo. Isso pode ser feito calculando a média (mean pooling) ou o valor máximo (max pooling) dos vetores de todos os tokens da sentença.
4.	Esses vetores de tamanho fixo são os embeddings u e v (REIMERS; GUREVYCH, 2019).
5.	Os vetores resultantes são utilizados para calcular a similaridade de cosseno das sentenças A e B.  A arquitetura usa uma função objetiva de regressão que resulta em uma saída entre -1 e 1 (REIMERS; GUREVYCH, 2019)

# Referências
BLOOMFIELD, R. J. The “Incomplete Revelation Hypothesis” and Financial Reporting. SSRN Electronic Journal, 2002. Disponível em: https://papers.ssrn.com/sol3/papers.cfm?abstract_id=312671 Acesso em: 20 mai. 2023

GINER-SOROLLA, R. et al. What Makes Moral Disgust Special? An Integrative Functional Review. Advances in Experimental Social Psychology, v. 57, p. 223–289, 2018. Disponível em: https://www.sciencedirect.com/science/article/pii/S0065260117300357# . Acesso em: 16 abr. 2025.

REIMERS, N.; GUREVYCH, I. Sentence-BERT: Sentence embeddings using Siamese BERT-networks. arXiv preprint arXiv:1908.10084, 2019. Disponível em: https://arxiv.org/abs/1908.10084 . Acesso em 06 mar. 2025
