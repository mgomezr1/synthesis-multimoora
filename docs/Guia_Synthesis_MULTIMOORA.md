# Guía de Sýnthesis MULTIMOORA

Versión 1.0. Autor: Mario Sergio Gómez Rueda. Contacto: mgomezr1@gmail.com

## 1. Descripción

Sýnthesis MULTIMOORA es una aplicación web académica, autocontenida en un solo archivo `index.html`, para aplicar el método MULTIMOORA de decisión multicriterio. La aplicación ordena las alternativas con tres métodos (sistema de razones, punto de referencia y forma multiplicativa completa) y reúne los tres ordenamientos con la teoría de dominancia, usando el conteo de Borda como contraste.

El nombre viene del griego σύνθεσις (sýnthesis), que significa composición: reunir varias partes en un todo.

Decisiones de alcance de esta versión:

- Síntesis: teoría de dominancia como resultado principal y conteo de Borda como contraste.
- Pesos: digitados por el usuario o calculados con la entropía de Shannon.
- Sin análisis de sensibilidad de pesos, igual que Harmonía VIKOR. Los indicadores de acuerdo entre métodos cumplen la función de medir la robustez.

## 2. Cómo guardarlo y ejecutarlo

1. Guarde el archivo `index.html` en cualquier carpeta.
2. Ábralo con doble clic. Funciona en Chrome, Edge, Firefox y Safari recientes.
3. Para publicarlo, súbalo a un repositorio de GitHub y active GitHub Pages sobre la rama principal.

Todos los cálculos ocurren en el navegador. Ningún dato sale del equipo. La única dependencia externa es SheetJS, que se descarga de `cdn.sheetjs.com` y solo se usa para generar el Excel. Sin conexión a internet funcionan los cálculos y el informe PDF, pero no la exportación a Excel.

## 3. Estructura de la interfaz

| Zona | Contenido |
|---|---|
| Portada | Nombre, lema y un explorador interactivo con tres proyectos ficticios. Al mover el peso del VPN se ve si los tres métodos coinciden o discrepan |
| Cómo funciona MULTIMOORA | Seis pasos, diagrama del flujo y supuestos del método |
| 1. Configuración | Objetivo, número de criterios y de alternativas, nombres de las alternativas |
| 2. Criterios y pesos | Origen de los pesos, nombre, unidad, sentido (Maximizar o Minimizar) y peso de cada criterio |
| 3. Matriz de decisión | Valor de cada alternativa en cada criterio |
| 4. Resultados | Diez bloques numerados, gráfico de posiciones e indicadores de calidad |
| 5. Exportación | Libro de Excel e informe ejecutivo en PDF |
| Acerca de Sýnthesis | Nombre, autoría, privacidad, pruebas automáticas, escenarios de prueba, referencias y cita |

El riel lateral marca la sección visible y el estado de cada paso: verde si está listo y ámbar si requiere atención. En tabletas y teléfonos se convierte en una barra superior.

## 4. Identidad visual

- Fondo verde petróleo profundo. Las superficies se separan por contraste de tono y sombras suaves, sin bordes.
- Par de colores con significado: **oro** para los criterios que se maximizan y **rosa** para los que se minimizan.
- Cada método tiene su tono: azul para el sistema de razones, violeta para el punto de referencia y menta para la forma multiplicativa.
- Toda la paleta y las medidas están definidas como variables en `:root`.
- Fuentes del sistema. Hay un bloque `@font-face` comentado por si se quieren cargar fuentes propias.
- Se respeta `prefers-reduced-motion` y la página no se desplaza horizontalmente a 400 px de ancho.

## 5. Fundamento de cálculo

Notación: x_ij es el valor de la alternativa i en el criterio j, con i de 1 a m y j de 1 a n. w_j es el peso del criterio j. Los pesos son no negativos y suman 1.

### 5.1 Normalización vectorial

```
x*_ij = x_ij / raíz( Σ_i x_ij² )
```

### 5.2 Sistema de razones

```
y_i = Σ_{j a maximizar} w_j · x*_ij  −  Σ_{j a minimizar} w_j · x*_ij
```

Mayor y_i es mejor.

### 5.3 Punto de referencia

```
r_j = máx_i x*_ij   si el criterio se maximiza
r_j = mín_i x*_ij   si el criterio se minimiza
z_i = máx_j  w_j · | r_j − x*_ij |
```

Menor z_i es mejor. Es la métrica mín-máx de Tchebycheff.

### 5.4 Forma multiplicativa completa

```
A_i = Π_{j a maximizar} (x*_ij)^w_j
B_i = Π_{j a minimizar} (x*_ij)^w_j
U_i = A_i / B_i
```

Mayor U_i es mejor. La aplicación usa los valores normalizados para que U no tenga unidades. Con los valores originales U cambia de magnitud, pero el orden es idéntico, porque cambiar la escala de una columna multiplica todas las U por la misma constante. Una prueba automática lo verifica.

### 5.5 Posiciones

En cada método la mejor alternativa recibe la posición 1. Dos puntajes empatan si su diferencia relativa es menor que 10⁻⁹. Los empates comparten posición y la siguiente se salta (1, 1, 3).

### 5.6 Síntesis por dominancia

Para cada par de alternativas a y b se cuenta en cuántos de los tres métodos a queda mejor ubicada que b, y en cuántos queda peor.

- a domina a b si queda mejor ubicada en más métodos que b.
- Si ambas ganan en el mismo número de métodos, ninguna domina.
- Puntaje de a = pares ganados − pares perdidos (regla de Copeland).
- La posición final ordena por puntaje de mayor a menor. Los empates comparten posición.
- Un par es circular cuando a domina a b pero no queda por delante en el orden final. Ocurre cuando hay un ciclo del tipo a > b > c > a.

### 5.7 Contraste de Borda

```
Borda_i = posición en razones + posición en referencia + posición en multiplicativa
```

Menor suma es mejor.

### 5.8 Pesos por entropía de Shannon

```
p_ij = x_ij / Σ_i x_ij
E_j  = −(1 / ln m) · Σ_i p_ij · ln p_ij
d_j  = 1 − E_j
w_j  = d_j / Σ_j d_j
```

Si todas las alternativas tienen el mismo valor en todos los criterios, la suma de d es cero y la aplicación pide digitar los pesos.

### 5.9 Indicadores de acuerdo

- Métodos que ubican primera a la ganadora, de 0 a 3.
- Unanimidad: los tres métodos producen exactamente el mismo orden.
- Correlación de Spearman entre cada par de ordenamientos, calculada como la correlación de Pearson de las posiciones.
- Coincidencia entre dominancia y Borda en el primer lugar y número de posiciones que difieren.
- Número de pares con razonamiento circular.

## 6. Funciones principales

| Función | Qué hace |
|---|---|
| `interpretarNumero`, `interpretarCelda` | Leen lo digitado con punto o coma decimal. La segunda exige valores mayores que cero |
| `normalizarVectorial` | Normalización de la matriz |
| `sistemaRazones`, `puntoReferencia`, `formaMultiplicativa` | Los tres métodos |
| `posicionar` | Posiciones con empates |
| `agregarDominancia`, `agregarBorda` | Las dos síntesis |
| `validarPesos`, `pesosEntropia`, `obtenerPesos` | Pesos digitados o por entropía |
| `calcularModelo` | Ejecuta todo el método y devuelve el objeto de resultados |
| `construirModeloDesdeEstado` | Valida lo digitado y arma el modelo numérico |
| `renderizarResultados`, `renderizarGraficoPosiciones` | Tablas y gráfico de pendientes |
| `crearLibroResultados` | Arma el libro de Excel hoja por hoja |
| `DocumentoPDF`, `generarInformePDF` | Generador de PDF propio, sin librerías |
| `ejecutarPruebas` | Batería de 18 pruebas automáticas |

Los datos del aplicativo (nombre, versión, autor, correo y nota de uso) están en la constante `APLICATIVO` al inicio del código. Cambiarlos allí actualiza la interfaz, el Excel y el PDF.

## 7. Contenido del Excel

| Hoja | Contenido |
|---|---|
| Menu | Datos del análisis e índice con enlaces a las demás hojas |
| Criterios | Sentido, signo (1 o −1) y peso de cada criterio |
| Entropia | Solo si los pesos son por entropía: proporciones, términos p × ln p, entropía, diversificación y pesos |
| Matriz | Matriz de decisión en unidades originales |
| Normalizacion | Raíz de la suma de cuadrados y valores normalizados |
| Razones | Aportes con signo, y, posición |
| Referencia | Punto de referencia, desviaciones ponderadas, z, posición |
| Multiplicativa | Factores, U, posición |
| Dominancia | Comparación por pares, pares ganados y perdidos, puntaje y posición final |
| Resultado | Clasificación final, suma y posición de Borda e indicadores de acuerdo |

Las hojas de cálculo llevan fórmulas reales de Excel (`SUMSQ`, `SQRT`, `ABS`, `MAX`, `MIN`, `PRODUCT`, `RANK`, `COUNTIF`, `SIGN`, `LN`), de modo que el estudiante puede seguir el cálculo celda por celda. Si se cambia un dato o un peso, el libro se recalcula. La hoja Resultado conserva el orden de filas del momento de la exportación.

## 8. Contenido del PDF

Informe ejecutivo en tamaño carta, generado con código propio:

1. Objetivo y recomendación.
2. Clasificación final, con la posición en cada método y barras de ubicación.
3. Criterios, sentido y pesos.
4. Resultados de los tres métodos.
5. Calidad y robustez del análisis.
6. Nota metodológica y referencias.

Todas las páginas llevan pie con el nombre del aplicativo, la versión, el autor, la mención de uso académico, el correo y la numeración, además de una marca de agua diagonal translúcida.

## 9. Ejemplo de uso con resultados verificados

Escenario «Prueba 2: selección de proyecto de inversión», con datos ficticios.

| Criterio | Sentido | Peso |
|---|---|---|
| VPN (millones COP) | Maximizar | 0,35 |
| TIR (%) | Maximizar | 0,25 |
| Plazo de recuperación (años) | Minimizar | 0,20 |
| Riesgo (escala de 1 a 10) | Minimizar | 0,20 |

| Alternativa | VPN | TIR | Plazo | Riesgo |
|---|---|---|---|---|
| Proyecto Solar | 850 | 18 | 4,5 | 6 |
| Proyecto Logístico | 620 | 14 | 3,2 | 3 |
| Proyecto Digital | 980 | 23 | 5,5 | 8 |
| Proyecto Inmobiliario | 540 | 11 | 6 | 2 |

Raíz de la suma de cuadrados por columna: 1535,8711; 34,2053; 9,8356; 10,6301.

| Alternativa | y | Pos. | z | Pos. | U | Pos. | Pares ganados | Pares perdidos | Posición final | Suma de Borda | Posición por Borda |
|---|---|---|---|---|---|---|---|---|---|---|---|
| Proyecto Solar | 0,1209 | 3 | 0,0753 | 1 | 0,9077 | 3 | 1 | 2 | 3 | 7 | 2 |
| Proyecto Logístico | 0,1221 | 2 | 0,0820 | 2 | 0,9387 | 1 | 3 | 0 | 1 | 5 | 1 |
| Proyecto Digital | 0,1291 | 1 | 0,1129 | 4 | 0,9200 | 2 | 2 | 1 | 2 | 7 | 2 |
| Proyecto Inmobiliario | 0,0438 | 4 | 0,1003 | 3 | 0,8053 | 4 | 0 | 3 | 4 | 11 | 4 |

Lectura: el Proyecto Logístico gana la síntesis aunque solo es primero en un método, porque nunca baja del segundo lugar. El Proyecto Digital lidera el sistema de razones, pero el punto de referencia lo castiga por su riesgo y su plazo. Borda confirma el primer lugar y difiere en una posición: empata a Solar y Digital, que la dominancia sí separa. Las correlaciones entre ordenamientos son −0,4 (razones y referencia), 0,8 (razones y multiplicativa) y 0 (referencia y multiplicativa), lo que indica una decisión sensible al método y que merece discusión en clase.

Estos valores coinciden con un cálculo independiente hecho en Python con NumPy, con diferencia máxima de 2 × 10⁻¹⁶.

## 10. Pruebas automáticas

Se ejecutan desde «Acerca de Sýnthesis», botón «Ejecutar verificación».

1. Interpretación de números: punto, coma, signo y errores.
2. Celdas de la matriz: se rechazan ceros y negativos.
3. Normalización vectorial: caso 3, 4, 5 y suma de cuadrados igual a 1.
4. Sistema de razones contra la solución analítica (0,04 y 0,24).
5. Punto de referencia contra la solución analítica (0,12 y 0).
6. Forma multiplicativa: valor analítico e invariancia al cambio de escala.
7. Posiciones: sentido, empates y salto de posición.
8. Dominancia: orden transitivo y ciclo detectado como razonamiento circular.
9. Conteo de Borda: sumas, posiciones y correlación.
10. Entropía de Shannon contra la solución analítica.
11. Pesos digitados: suma igual a 1, negativos, vacíos y ceros.
12. Escenario de proyectos completo contra fórmulas escritas aparte.
13. Pesos por entropía en un escenario completo y caso de desacuerdo.
14. Tamaños dinámicos: 54 modelos aleatorios con n de 2 a 12 y m de 2 a 10.
15. Exportación a Excel: hojas, enlaces, fórmulas y valores.
16. Informe PDF: estructura, pie de página y marca de agua en cada página.
17. Formatos numéricos y texto del PDF.
18. Interfaz: el escenario cargado en pantalla coincide con el cálculo directo.

## 11. Verificación realizada

- La aplicación se ejecutó en Chromium. Las 18 pruebas pasan y la consola no muestra errores ni advertencias.
- Los resultados de cuatro escenarios se compararon con un cálculo independiente en Python con NumPy. Diferencia máxima: 1,2 × 10⁻¹⁵.
- Cuatro libros de Excel se recalcularon desde cero con LibreOffice, después de borrar los valores guardados. Entre 155 y 469 fórmulas por libro coinciden con los valores de la aplicación, con diferencia máxima de 4 × 10⁻¹².
- El PDF abre sin errores, tiene 3 páginas en el ejemplo, con pie de página y marca de agua en todas.
- A 400 px de ancho la página no se desplaza horizontalmente.
- Se recorrió el flujo manual: configuración, criterios, matriz, cálculo, cambio a pesos por entropía y mensaje de error ante un valor cero.

No se verificó la apertura del libro en Microsoft Excel, solo en LibreOffice. SheetJS se probó con la versión 0.18.5 del registro de npm, porque el entorno de verificación no tenía acceso a `cdn.sheetjs.com`; la aplicación enlaza la versión 0.20.3, igual que el resto del portafolio.

## 12. Limitaciones y supuestos discutibles

- **Valores positivos.** La forma multiplicativa no admite ceros ni negativos, así que la aplicación exige valores mayores que cero en toda la matriz. Un criterio con ceros o negativos debe reescalarse antes.
- **Pesos.** El MULTIMOORA original no usa pesos. Su incorporación como coeficientes de significancia es una extensión posterior. En la forma multiplicativa actúan como exponentes, porque multiplicar por una constante no cambiaría el orden.
- **Dominancia.** Brauers y Zavadskas describen la teoría de dominancia de forma verbal, con reglas de dominancia, igualdad y transitividad. La implementación por pares con regla de mayoría y puntaje de Copeland es una interpretación operativa; otras implementaciones pueden resolver distinto los casos con empates o ciclos.
- **Tres votantes.** Con solo tres ordenamientos, un cambio pequeño en un método puede alterar la síntesis. Por eso se muestran los indicadores de acuerdo y el contraste de Borda.
- **Entropía.** Los pesos por entropía son objetivos, pero no reflejan las preferencias de quien decide. Además dependen de la escala: un criterio con valores muy dispersos recibe mucho peso aunque sea poco importante.
- **Escalas.** La normalización vectorial supone escalas de razón. Con escalas ordinales el resultado debe leerse con cautela.
- **Sin sensibilidad.** Esta versión no incluye análisis de sensibilidad de pesos.

## 13. Referencias

Brauers, W. K. M., & Zavadskas, E. K. (2006). The MOORA method and its application to privatization in a transition economy. *Control and Cybernetics, 35*(2), 445–469.

Brauers, W. K. M., & Zavadskas, E. K. (2010). Project management by MULTIMOORA as an instrument for transition economies. *Technological and Economic Development of Economy, 16*(1), 5–24. https://doi.org/10.3846/tede.2010.01

Brauers, W. K. M., & Zavadskas, E. K. (2011). MULTIMOORA optimization used to decide on a bank loan to buy property. *Technological and Economic Development of Economy, 17*(1), 174–188. https://doi.org/10.3846/13928619.2011.560632

Brauers, W. K. M., & Zavadskas, E. K. (2012). Robustness of MULTIMOORA: A method for multi-objective optimization. *Informatica, 23*(1), 1–25.

Hafezalkotob, A., Hafezalkotob, A., Liao, H., & Herrera, F. (2019). An overview of MULTIMOORA for multi-criteria decision-making: Theory, developments, applications, and challenges. *Information Fusion, 51*, 145–177. https://doi.org/10.1016/j.inffus.2018.12.002

Shannon, C. E. (1948). A mathematical theory of communication. *Bell System Technical Journal, 27*(3), 379–423. https://doi.org/10.1002/j.1538-7305.1948.tb01338.x
