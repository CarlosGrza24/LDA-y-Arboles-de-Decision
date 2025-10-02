# Análisis de LDA y Árboles de Decisión

Este proyecto analiza la relación entre distintos sistemas de riego y la probabilidad de que un municipio presente **alta vs baja** cantidad de unidades de producción (UP) agropecuarias activas.  
El objetivo es comparar dos metodologías de clasificación: **Linear Discriminant Analysis (LDA)** y **Árboles de Decisión con poda**, evaluando su capacidad predictiva y sus diferencias en interpretabilidad.  

La hipótesis es que la tecnificación del riego (aspersores, pivote central, goteo, etc.) se asocia positivamente con una mayor probabilidad de alta actividad agropecuaria.

Los datos provienen del **Censo Agropecuario 2022 (INEGI)**, disponibles públicamente en la plataforma oficial de descargas:  
- [INEGI – Censo Agropecuario 2022](https://www.inegi.org.mx/app/descarga/ficha.html?tit=1795268&ag=0&f=csv)

Archivo utilizado: [CA2022_Cultivos_Superficie_Produccion.csv](CA2022_Cultivos_Superficie_Produccion.csv)  

## Datos utilizados

- **Total UP agropecuaria activas** → Total de unidades de producción activas (objetivo, luego binarizado por mediana: 1=alta, 0=baja).  
- **UP Agrícolas** → Total de UP agrícolas.  
- **UP Argri Riego (R)** → UP agrícolas con riego.  
- **UP Riego Gravedad (RG)** → UP con riego por gravedad.  
- **UP R Por Microaspersión** → UP con riego por microaspersión.  
- **UP R Por Aspersión** → UP con riego por aspersión general.  
- **UP R Por Aspersión Pivote Central** → UP con riego por pivote central.  
- **UP R Por Aspersión Avance Frontal** → UP con riego por avance frontal.  
- **UP R Por Aspersión Cañón** → UP con riego por aspersión de cañón.  
- **UP R Por Aspersores** → UP con riego por aspersores.  
- **UP R Sistema De Goteo** → UP con riego por goteo.  
- Identificadores como **NOM_MUN**, **ENTIDAD**, **NOMBRE** se eliminaron en el preprocesamiento.  

## Metodología

1. **Exploración inicial**  
   - Revisión de tipos de datos, valores faltantes y distribución de la variable objetivo.

2. **Limpieza y preprocesamiento**  
   - Eliminación de columnas identificadoras.  
   - Filtro de outliers (Tukey) sobre la variable objetivo.  
   - Control de colinealidad: eliminación de predictores con |r| > 0.85.

3. **Construcción del objetivo**  
   - Binarización de *Total UP agropecuaria activas* con la mediana: 1 = alta, 0 = baja.  
   - Clases balanceadas aproximadamente 50/50.

4. **Partición de datos (70/30)**  
   - Uso de `train_test_split(..., stratify=y)` para conservar la proporción de clases en Train y Test.

5. **Selección de variables con GLM**  
   - Ajuste de un modelo logístico con `statsmodels` (GLM Binomial).  
   - Selección de las dos variables con menor p-value para el análisis posterior.

6. **Modelado con LDA**  
   - Estandarización de las variables seleccionadas.  
   - Entrenamiento de un **Linear Discriminant Analysis (LDA)**.  
   - Visualización de la frontera de decisión y evaluación de métricas.

7. **Modelado con Árbol de Decisión**  
   - Poda por complejidad de costo (`ccp_alpha`).  
   - Selección del alpha óptimo mediante validación cruzada K-Fold (cv=5).  
   - Entrenamiento del árbol podado y visualización de sus particiones.

8. **Evaluación y métricas**  
   - Cálculo de Accuracy, Precision, Recall, F1 Score y AUC.  
   - Generación de matrices de confusión para cada modelo.  
   - Comparación de desempeño y discusión sobre interpretabilidad.


## Archivos del proyecto

- [Reporte en ipynb](A2.2_648241.ipynb)
- [Reporte en html](A2.2_648241.html)  
- [CA2022_Cultivos_Superficie_Produccion.csv](CA2022_Cultivos_Superficie_Produccion.csv)  
- [DiccionarioDeDatos.csv](DiccionarioDeDatos.csv)
