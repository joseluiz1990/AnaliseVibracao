# AnaliseVibracao
# CLASSIFICAÇÃO DE FALHAS EM MÁQUINAS ROTATIVAS UTILIZANDO “K-NEAREST NEIGHBORS” BASEADO EM ATRIBUTOS NO DOMÍNIO DO TEMPO E DA FREQUÊNCIA A PARTIR DE SINAIS DE VIBRAÇÃO TRI-AXIAIS

Este repositório contém o desenvolvimento e a implementação de uma metodologia automatizada para o diagnóstico e classificação de condições de saúde e falha em máquinas rotativas. O projeto é fundamentado na integração entre o processamento digital de sinais de vibração triaxiais e o algoritmo de Aprendizado de Máquina *K-Nearest Neighbors* (KNN), focado em soluções de baixa complexidade computacional e alta confiabilidade para a Indústria 4.0.

## 👥 Autores e Vínculo
* **Autor:** José Luiz Lopes (FATEC Deputado Ari Fossen - Jundiaí) — `jose.lopes5@aluno.cps.sp.gov.br`
* **Orientador:** Argemiro Pentian Junior (Centro Paula Souza) — `argemiro.pentian@cps.sp.gov.br`

## 📊 Visão Geral do Projeto
A transição da Manutenção Baseada em Condição (CBM) tradicional para a Manutenção Preditiva exige diagnósticos precoces e assertivos. Este trabalho propõe um pipeline robusto que realiza a fusão de dados triaxiais, extrai atributos híbridos nos domínios do tempo e da frequência, realiza o tratamento estatístico de outliers e treina um classificador de vizinhos mais próximos otimizado.

### 📐 Pipeline Metodológico
1. **Fusão de Sinais Triaxiais:** Consolidação dos sinais capturados nos eixos 1, 2 e 3 em um único vetor amplitude por meio da **Resultante Euclidiana**, preservando a energia total do sistema independente da orientação da falha.
2. **Engenharia de Atributos (Domínio do Tempo):** Extração de descritores estatísticos a partir da Função Densidade de Probabilidade (PDF):
   * **RMS (Root Mean Square):** Quantificação da energia eficaz e severidade global.
   * **Fator de Crista & Curtose:** Identificação de picos impulsivos e deformações estruturais em estágios iniciais.
3. **Engenharia de Atributos (Domínio da Frequência):** Aplicação da Transformada Rápida de Fourier (FFT) com janelamento Hanning (segmentação fixa de 4096 pontos para otimização $2^{12}$).
   * Monitoramento do trio de harmônicas **1X** (desbalanceamento), **2X** (desalinhamento) e **3X** (folgas estruturais/impactos).
4. **Tratamento de Outliers:** Utilização da Amplitude Interquartílica (IQR) sobre a energia RMS do grupo "Saudável", removendo 7 amostras espúrias para mitigar alarmes falsos.
5. **Padronização:** Escalonamento dos atributos via **Z-Score** para garantir a homogeneidade das grandezas no cálculo das distâncias euclidianas.
6. **Classificação Inteligente:** Ajuste do modelo KNN com otimização hiperparamétrica.

## 📂 Base de Dados
* **Origem:** Repositório Público Kaggle (*Vibration Faults dataset for Rotating Machines*).
* **Características:** Sinais triaxiais coletados a 1000 Hz de motores de indução de 45 kW operando a 2980 RPM.
* **Volume Inicial:** 220 arquivos de 5 segundos (103 Saudáveis / 117 Com Falha).
* **Volume Pós-Limpeza:** 213 amostras balanceadas (96 Saudáveis / 117 Com Falha).

## 🛠️ Tecnologias e Bibliotecas Utilizadas
O pipeline foi totalmente desenvolvido em ambiente **Python (versão 3.10)**, empregando o ecossistema científico:
* **NumPy (v1.24):** Manipulação matricial e cálculo de descritores temporais.
* **SciPy (v1.10):** Processamento espectral (`scipy.fft`).
* **Scikit-Learn (v1.2):** Implementação do `StandardScaler`, `KNeighborsClassifier`, validação cruzada (`StratifiedKFold`) e busca exaustiva (`GridSearchCV`) para blindagem contra vazamento de dados (*data leakage*).
* **Matplotlib (v3.7) & Seaborn (v0.12):** Geração de visualizações gráficas e matrizes de correlação.

## 📈 Resultados Obtidos
A configuração ideal identificada via *Grid Search* definiu os hiperparâmetros: **$k=5$ vizinhos**, métrica de **distância Euclidiana** e pesos ponderados pela distância (`weights='distance'`).

* **Acurácia Final do Teste:** `98,44%`
* **Especificidade:** `100,00%` (Zero alarmes falsos no estado estável, eliminando paradas desnecessárias).
* **Sensibilidade (Recall):** `97,14%`
* **Validação Cruzada (5-Fold):** Acurácia média de `96,72%` com desvio padrão mínimo de `0,0236`, comprovando estabilidade e alta capacidade de generalização do modelo.
* **Destaque Estatístico:** A 3ª harmônica ($3X$) consolidou-se como o descritor espectral de maior severidade e relevância física, registrando uma correlação linear ponto-bisserial de $r = 0,5532$ com o estado de falha.
README.md
Exibindo README.md.
