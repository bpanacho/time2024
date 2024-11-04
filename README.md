
### Autores: Bianca Panacho Ferreira, Pedro Henrique Campos Moreira e Marcus Vinicius Diniz dos Reis
<img src="/images/titulo.png">

<div align="center">
  
# Hackathon 2024 
</div>

# Visão Geral
Este projeto desenvolve um modelo para classificar jogadores em vídeos esportivos. Utiliza YOLO11n para detectar jogadores no campo/ringue e K-Means para classificar os jogadores em diferentes times com base na cor dos uniformes. Esse processo é aplicado em três esportes: boxe, futebol e lacrosse.

## Resumo
+ Frames são extraídos dos vídeos com base nas coordenadas dos JSON, que definem as áreas dos jogadores.
+ Bounding boxes são visualizadas para conferir a correspondência entre frames e JSON.
+ Uma máscara verde é aplicada ao fundo, destacando apenas os jogadores, com ajustes de zoom, saturação e brilho.
+ K-Means classifica os jogadores em times de acordo com as cores dos uniformes:
  - **Futebol**: camisetas.
  - **Boxe**: luvas.
  - **Lacrosse**: uniformes.
+ Os JSON são atualizados com novas classes e convertidos para o formato .txt para o YOLO11n, que é então treinado para inferir em imagens e vídeos de teste.

## Arquivos Classificados

+ Os arquivos JSON classificados estão localizados na pasta **json_retorno**, organizados em subpastas para cada modalidade de esporte: *boxe*, *futebol* e *lacrosse*. Cada subpasta contém as labels correspondentes. Por questão de espaço, fizemos o upload de amostras parciais com arquivos JSON para cada esporte, em vez de toda a quantidade disponível.
<br>

## Execução do Projeto

A execução completa do projeto é opcional. Caso deseje realizá-la, siga alguma das opções abaixo:

**Execução Completa** 
+ Execute todas as etapas do arquivo Jupyter Notebook, localizado na pasta **códigos do projeto**. Há um notebook específico para cada modalidade de esporte, facilitando a execução individual por esporte.
   
**Teste de Precisão**
+ Se preferir, você pode realizar apenas o teste de precisão dos modelos. Para isso, disponibilizamos os arquivos `best.pt` no drive, Pasta - [Hackathon](https://drive.google.com/drive/folders/1ot3yJXcx7wLfLhZ0Y03kuhl6a3TPY7aL?usp=sharing) treinados para teste de inferência, ao final de cada jupyter notebook há um código de inferência que permite operar sobre vídeos ou imagens.
<br>

## Tecnologias Utilizadas
<table> 
  <tr>
    <td>LINGUAGEM</td>
    <td>BIBLIOTECAS</td>
    <td>AMBIENTES DE DESENVOLVIMENTO </td>
  </tr>
  <tr>
    <td>Python 3.8+</td>
    <td>Ultralytics 8.3.27, PyTorch 1.8+, OpenCV 4.10.0, Pandas, NumPy, Scikit-Learn, Matplotlib, PyYAML</td>
    <td>Jupyter Notebook</td>
  </tr>
</table>
<br>

# Configuração do Ambiente

## 1. Instalação 

Clone o repositório e instale as dependências:
```
git clone https://github.com/bpanacho/team2024.git
cd "./team2024"
```

## 2. Ativação do Ambiente Virtual

Para garantir o isolamento das dependências e facilitar o controle de versões, é recomendável utilizar um ambiente virtual específico para o projeto.

**Passo 1: Ativar o Ambiente Virtual**

Ative o ambiente virtual de acordo com o seu sistema operacional:

+ No Windows:

```
.\env\Scripts\activate
```

+ No macOS/Linux:

```
source env/bin/activate
```

<br>

## Personalização do Ambiente Virtual

**Passo 1: Crie o ambiente virtual**: 
<br>
No terminal, dentro da pasta raiz do projeto, execute o comando abaixo para criar o ambiente virtual chamado `env`:

```
python -m venv env
```

**Passo 2: Instalar as Dependências:**
Com o ambiente virtual ativo, instale a biblioteca principal para execução do YOLO:
```
pip install pip install ultralytics
```

**Para desativar o ambiente virtual, use o comando:**
```
deactivate
```

<br>

# Modelo Escolhido

+ **YOLO (You Only Look Once)** foi escolhido por sua eficiência em tarefas de detecção de objetos em tempo real. Baseado em Redes Neurais Convolucionais, YOLO é uma abordagem poderosa e popular na área de Deep Learning para a detecção de objetos. <br> 
+ **YOLO11n** foi escolhido especificamente pela sua leveza e velocidade, o que o torna ideal para aplicações em dispositivos com recursos limitados e para análises rápidas de eventos esportivos.
  + A versão **"n"** (nano) é otimizada para operar com baixo consumo de memória e processamento, sem comprometer significativamente a precisão, o que é fundamental para detectar jogadores e suas interações durante o jogo.<br>
+ Neste projeto, utilizamos YOLO11n para detectar objetos em eventos esportivos, auxiliando na identificação de jogadores e suas posições.

<br>

## Estrutura de Pastas do Dataset 
A estrutura de pastas é organizada para separar dados de treino, teste e validação. Essa divisão permite uma avaliação precisa do modelo, evitando sobreposição de dados. A separação foi realizada nas seguintes proporções:

**60% para treino:** Dados destinados ao aprendizado do modelo.  <br>
**20% para teste:** Dados utilizados para avaliar o modelo de forma independente após o treino.  <br>
**20% para validação:** Dados usados durante o treino para ajustar o modelo e prevenir overfitting.  <br>

Visualização da estrutura do *projeto final*: 
 <br>
 
├── dataset <br>
│   ├── test <br>
│   │   └── imagens/vídeos <br>
│   ├── train <br>
│   │   ├── images <br>
│   │   └── labels <br>
│   ├── val <br>
│   │   ├── images <br>
│   │   └── labels <br>
├── env <br>
├── main.ipynb  
├── inferencia.py <br>
├── json_retorno <br>
│   ├── boxe <br>
│   ├── futebol <br>
│   ├── lacrosse <br>
└── data.yaml <br>

<br>


# Fluxograma do Projeto

**1. Submissão de Vídeo e Arquivos JSON de Entrada:**

+ O projeto começa com a submissão de vídeos e arquivos JSON contendo as coordenadas dos jogadores.

**2. Extração de Frames e Recorte:**

+ Frames são extraídos dos vídeos, e cada frame é recortado com base nas coordenadas dos JSON para capturar apenas as áreas onde os jogadores estão presentes.

**3. Visualização e Conferência de Bounding Boxes:**

+ Os frames recortados são exibidos com bounding boxes sobrepostas para conferir a correspondência com os arquivos JSON e garantir que as detecções estejam corretas.
<img src="/images/fut_detection.jpeg">
<br>

**4. Aplicação de Máscara Verde e Inversão do Foco:**

+ Após a conferência, é aplicada uma máscara verde para o fundo.
<img src="/images/grama.jpeg">

+  Em seguida, o foco é invertido para destacar apenas os jogadores.
<img src="/images/inversao.jpeg">

**5. Ajuste de Saturação, Brilho e Zoom:**
<table>
  <tr>
    <td valign="top">
      <center>As áreas dos jogadores recebem ajustes adicionais de zoom, saturação e brilho.</center>
      <br>
      <img src="/images/kit_1.jpeg" height="400"/>
    </td>
    <td valign="top">
      <center>As informações de cor são armazenadas para posterior análise.</center>
      <br>
      <img src="/images/kit_2.jpeg" height="400"/>
    </td>
  </tr>
</table>


**6. Análise de Cores e Classificação de Times com K-Means:**

+ As cores dos uniformes dos jogadores são extraídas e agrupadas com K-Means para classificar os jogadores em diferentes times com base nas cores dominantes:
  + Futebol: Cores das camisetas são analisadas para classificar cada jogador no time correspondente.
  + Boxe: A cor da luva diferencia os lutadores entre si.
  + Lacrosse: As cores dos uniformes são usadas para identificar jogadores em times distintos.

+ **Kit de Cores**: Capturamos as cores principais presentes na camiseta do jogador, tiramos a média e associamos cada jogador ao seu time com base nas características visuais.
+ **Agrupamento com K-Means:** As cores são agrupadas em clusters para representar cada time, facilitando a diferenciação visual entre jogadores.
<br>
<div align="center">
  <img src="/images/kit_resized.gif">
</div>

**7. Atualização dos Arquivos JSON com Classes:**

+ Os arquivos JSON são atualizados para incluir novas classes que distinguem os jogadores com base nos clusters de cor, gerando um JSON final com as classes diferenciadas.

**8. Conversão dos JSON para o Formato YOLO:**

 + Após a classificação e atualização, os JSON são convertidos para o formato .txt exigido pelo YOLO para treinamento.

**9. Treinamento do Modelo YOLO11n:**

+ Com os dados organizados e rotulados, fazemos um novo Dataset e o YOLO11n é treinado para realizar a detecção dos jogadores em imagens e vídeos, permitindo análise em eventos esportivos.

**10. Inferência em Imagens e Vídeos de Teste:**

+ O modelo treinado é aplicado em imagens e vídeos de teste para verificar a inferência e avaliar o desempenho da classificação.
<br>

# Fluxograma
<br>
<img src="/images/fluxo.jpeg">
<br>
