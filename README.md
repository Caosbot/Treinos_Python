# Estudos e projetros de Machine Learning.
## No arquivo yoloSeg.ipynb:
 ### 🔬 Metodologia de Previsão: Visão Híbrida (YOLO + CNN)
Esta etapa detalha o pipeline de inferência utilizado para analisar uma imagem de esfregaço sanguíneo. O procedimento combina um modelo de detecção de objetos (YOLO) para isolar células e um modelo de classificação (CNN) para categorizar cada célula individualmente.

### 1. Detecção e Segmentação com YOLOv8 (ultralytics)
O primeiro estágio utiliza um modelo YOLOv8 treinado (best.pt) para identificar e localizar as células relevantes na imagem de entrada.

Ações:
Carregamento: O modelo YOLO treinado (best.pt) é carregado.

Detecção: O modelo processa a imagem do esfregaço sanguíneo e retorna as coordenadas das caixas delimitadoras (bounding boxes) para as classes:

white_cell (Célula Branca)

malaria (Glóbulo Vermelho Infectado)

Contagem e Visualização: As células são contadas por tipo (e.g., 26 células brancas, 214 malárias). A imagem original é então marcada com as caixas delimitadoras (vermelho para células brancas e verde para malária) e os resultados são salvos.

Resultado:
A imagem de entrada é transformada em uma coleção de 240 imagens recortadas (images_yolo), onde cada recorte isola uma única célula e é redimensionado para 50×50 pixels.

### 2. Classificação Pós-Segmentação com CNN
O segundo estágio utiliza um modelo de Rede Neural Convolucional (CNN), treinado previamente para a classificação fina de células, para analisar cada imagem recortada individualmente.

Ações:
Pré-processamento de Entrada: O conjunto de imagens recortadas do YOLO (images_yolo) é convertido em um tensor do TensorFlow (yolo_tensor).

Inferência: O modelo CNN (ModelCNN.h5), que alcançou 96.63% de acurácia no conjunto de teste, executa a previsão em cada célula. O modelo está treinado para distinguir entre três categorias principais:

0: White (Célula Branca)

1: Vivax (Malária P. vivax)

2: Falciparum (Malária P. falciparum)

Interpretação: As previsões são interpretadas, e a categoria com a maior probabilidade (maior tf.argmax) é definida como a classe final da célula.

Resultado:
O procedimento fornece uma contagem final detalhada da imagem, discriminando as células infectadas por subespécie, o que é crucial para o diagnóstico:

Categoria	Contagem Final
White (Células Brancas)	26
Vivax	148
Falciparum	4
Total Classificado	178

Exportar para as Planilhas
(Observação: Os totais de White, Vivax e Falciparum foram ajustados para refletir a saída completa do seu script e demonstrar o resultado final do pipeline.)
