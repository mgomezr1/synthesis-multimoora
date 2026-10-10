# Sýnthesis MULTIMOORA

Aplicación web académica para tomar decisiones con el método **MULTIMOORA** de Brauers y Zavadskas: ordene sus alternativas con tres métodos distintos y obtenga una sola clasificación, respaldada por el acuerdo entre ellos.

**Abrir la aplicación:** https://mgomezr1.github.io/synthesis-multimoora/

No requiere instalación ni registro. Funciona en cualquier navegador moderno, en computador, tableta o teléfono.

## Qué permite hacer

- Definir un objetivo, los criterios y las alternativas, en la cantidad que necesite.
- Indicar si cada criterio se maximiza o se minimiza.
- Digitar los pesos o calcularlos con la entropía de Shannon a partir de los datos.
- Aplicar el sistema de razones, el punto de referencia y la forma multiplicativa completa.
- Reunir los tres ordenamientos con la teoría de dominancia y contrastarlos con el conteo de Borda.
- Revisar el acuerdo entre métodos: correlaciones, coincidencia con Borda y razonamiento circular.
- Descargar los resultados en Excel, con fórmulas reales, y un informe ejecutivo en PDF.

## Cómo usarla

1. Lea la sección «Cómo funciona MULTIMOORA» y mueva el control del ejemplo de la portada.
2. En **Configuración**, escriba el objetivo, el número de criterios y de alternativas, y genere la configuración.
3. En **Criterios y pesos**, registre nombre, unidad, sentido y peso de cada criterio, y confirme.
4. En **Matriz de decisión**, digite el valor de cada alternativa en cada criterio. Todos deben ser mayores que cero.
5. Pulse **Calcular resultados** y revise los indicadores de calidad.
6. Descargue el **Excel** o el **informe PDF**.

Los **escenarios de prueba** de la sección «Acerca de Sýnthesis» cargan ejemplos con datos ficticios para practicar.

## Privacidad

Todos los cálculos se hacen en su navegador. Los datos no se envían a ningún servidor ni se guardan: al cerrar o recargar la página se pierden, así que descargue el Excel o el PDF antes de salir.

## Requisitos

- Navegador actualizado (Chrome, Edge, Firefox o Safari) con JavaScript activo.
- Conexión a internet para exportar a Excel, porque la aplicación descarga la biblioteca SheetJS. El informe PDF funciona sin conexión.

## Documentación

La guía completa (método, fórmulas, funciones, exportaciones y limitaciones) está en [docs/Guia_Synthesis_MULTIMOORA.md](docs/Guia_Synthesis_MULTIMOORA.md). En la carpeta [ejemplos](ejemplos) hay un libro de Excel y un informe PDF generados con datos de prueba.

## Cómo citar

Gómez Rueda, M. S. (2026). *Sýnthesis MULTIMOORA* (Versión 1.0) [Software]. https://mgomezr1.github.io/synthesis-multimoora/

## Autoría y uso

Este aplicativo fue desarrollado por Mario Sergio Gómez Rueda. Su uso es de carácter académico y cualquier otro uso se regirá por el derecho de la propiedad intelectual. Consulte los términos en [LICENSE.md](LICENSE.md). Sugerencias o dudas: mgomezr1@gmail.com

## Fundamento metodológico

- Brauers, W. K. M., & Zavadskas, E. K. (2006). The MOORA method and its application to privatization in a transition economy. *Control and Cybernetics, 35*(2), 445–469.
- Brauers, W. K. M., & Zavadskas, E. K. (2010). Project management by MULTIMOORA as an instrument for transition economies. *Technological and Economic Development of Economy, 16*(1), 5–24. https://doi.org/10.3846/tede.2010.01
- Brauers, W. K. M., & Zavadskas, E. K. (2011). MULTIMOORA optimization used to decide on a bank loan to buy property. *Technological and Economic Development of Economy, 17*(1), 174–188. https://doi.org/10.3846/13928619.2011.560632
- Brauers, W. K. M., & Zavadskas, E. K. (2012). Robustness of MULTIMOORA: A method for multi-objective optimization. *Informatica, 23*(1), 1–25.
- Hafezalkotob, A., Hafezalkotob, A., Liao, H., & Herrera, F. (2019). An overview of MULTIMOORA for multi-criteria decision-making: Theory, developments, applications, and challenges. *Information Fusion, 51*, 145–177. https://doi.org/10.1016/j.inffus.2018.12.002
- Shannon, C. E. (1948). A mathematical theory of communication. *Bell System Technical Journal, 27*(3), 379–423. https://doi.org/10.1002/j.1538-7305.1948.tb01338.x

## Componentes de terceros

La exportación a Excel usa [SheetJS Community Edition](https://sheetjs.com), distribuida bajo la licencia Apache 2.0 y cargada desde cdn.sheetjs.com.
