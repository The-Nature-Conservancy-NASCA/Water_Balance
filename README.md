# Water_Balance
Como partes de los componentes de modelación que integra la WaterFunds App se encuentra la solución de balances de masa entre los diferentes componentes de los sistemas de captación, distribución y tratamiento del agua potable. En este sentido, en este repositorio se encuentra consolidado el algoritmo que permite solucionar dichos balances.
Para ejemplificar su funcionamiento, observemos el siguiente esquema de captación:

Figura 2.1 Esquema de Captación
![Sin titulo](https://github.com/The-Nature-Conservancy-NASCA/Water_Balance/blob/main/FIGURES/Dummy_Grapho.jpg)
Fuente: Elaboración propia

En el esquema anterior, cada elemento se encuentra identificado por un código numérico único. En la App, este código, debe ser un valor positivo mayor o igual a 1. El cero se encuentra reservado al interior del código, por lo que no se puede utilizar como identificador.
Para efectos de facilidad, en la WaterFunds App, los esquemas de captación siempre deben iniciar con un elemento denominado río, luego a este, se conectan los demás elementos que componene el sistema. Dado que cada elemento contiene un código único, la topología (conexiones entre elementos) puede ser representada como el conjunto de pares de elementos contiguos ordenados en un arreglo matricial. La topología, se ingresa en el archivo de nombre **0_WI_Topology.csv**. A continuación, se presenta la topología del esquema anterior.

**Tabla 1**. Topología
|From_Element|To_Element|
| -- | -- |
|100|1|
|1|2|
|2|3|
|3|4|
|4|5|
|5|6|
|6|7|
|7|8|
|4|9|
|9|10|
|10|11|
|11|12|

Cada elemento que compone el sistema tiene ciertas propiedades que son importantes para el balance. A continuación, se listan las propiedades que puede tener cada elemento.
|Propiedad | Descripción|
| -- | -- |
|PWater| Es el porcentaje de caudal entregado por el elemento anterior que pasa por elemento|
|RetSed| Es el porcentaje de retención de sedimentos que posee el elemento |
|RetN| Es el porcentaje de retención de sedimentos que posee el elemento|
|RetP| Es el porcentaje de retención de sedimentos que posee el elemento|

En el esquema, los valores de las propiedades para cada elemento se presentan en un recuadro. Sin embargo, en la aplicación, estas se introducen en el archivo **1_WI_Elements_Param.csv** como un arreglo matricial tal como se presenta a continuación.

**Tabla 2**. Propiedades de elementos
|From_Element|PWater|RetSed|RetN|RetP|
| -- | -- | -- | -- | -- |
|1|100|0|0|0|
|2|100|5|1|1|
|3|100|25|15|20|
|4|100|10|5|10|
|5|50|5|1|1|
|6|100|20|5|15|
|7|100|5|1|1|
|8|100|0|0|0|
|9|50|5|1|1|
|10|100|20|5|15|
|11|100|5|1|1|
|12|100|0|0|0|
|100|100|0|0|0|

Los valores de caudal, carga de sedimentos, nitrógeno y fosforo con los cuales se realiza el análisis (estos valores son el resultado de las modelaciones de InVEST), se introducen como series de tiempo, sin indexación Juliana. Es decir, los años se presentan como una secuencia numérica continua, mas no con una fecha fija. Los archivos en los cuales introducen estos datos son:

**Tabla 3**. Archivos
|Archivo | Descripción| unidad |
| -- | -- | -- |
|2_WI_AWYInputs.csv|Volumen de agua de la fuente|m3|
|2_WI_WNInputs.csv| Carga de nitrógeno de la fuente | Kg |
|2_WI_WPInputs.csv| Carga de fosforo de la fuente | Kg |
|2_WI_WSedInputs.csv | Carga de sedimentos de la fuente | Ton|
|2_WI_WSedInputs.csv | Caudal extraído| l/s|

Conceptualmente, el código tiene la posibilidad de entender estas tres entradas, para cada uno de los elementos, no obstante, en la App, las entradas se permiten algunos puntos específicos como lo son los embalses. A continuación, se muestran las series temporales de entrada para cada elemento, en un periodo de 5 años. Es muy importante que no use notación científica en los archivos o el código arrojara error.

**Tabla 4**. Volumen de agua en la fuente
|0|1|2|3|4|5|6|7|8|9|10|11|12|100|
| -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- |
|0|1000000000|0|0|0|0|0|0|0|0|0|0|0|1000000000|
|1|1000000000|0|0|0|0|0|0|0|0|0|0|0|1000000000|
|2|1000000000|0|0|0|0|0|0|0|0|0|0|0|1000000000|
|3|1000000000|0|0|0|0|0|0|0|0|0|0|0|1000000000|
|4|1000000000|0|0|0|0|0|0|0|0|0|0|0|1000000000|
|5|1000000000|0|0|0|0|0|0|0|0|0|0|0|1000000000|

**Tabla 5**. Carga de nitrógeno en la fuente
|0|1|2|3|4|5|6|7|8|9|10|11|12|100|
| -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- |
|0|0|0|0|0|0|0|0|0|0|0|0|0|1000|
|1|0|0|0|0|0|0|0|0|0|0|0|0|1000|
|2|0|0|0|0|0|0|0|0|0|0|0|0|1000|
|3|0|0|0|0|0|0|0|0|0|0|0|0|1000|
|4|0|0|0|0|0|0|0|0|0|0|0|0|1000|
|5|0|0|0|0|0|0|0|0|0|0|0|0|1000|

**Tabla 6**. Carga de fosforo en la fuente
|0|1|2|3|4|5|6|7|8|9|10|11|12|100|
| -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- |
|0|0|0|0|0|0|0|0|0|0|0|0|0|1000|
|1|0|0|0|0|0|0|0|0|0|0|0|0|1000|
|2|0|0|0|0|0|0|0|0|0|0|0|0|1000|
|3|0|0|0|0|0|0|0|0|0|0|0|0|1000|
|4|0|0|0|0|0|0|0|0|0|0|0|0|1000|
|5|0|0|0|0|0|0|0|0|0|0|0|0|1000|

**Tabla 7**. Carga de sedimentos en la fuente
|0|1|2|3|4|5|6|7|8|9|10|11|12|100|
| -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- |
|0|0|0|0|0|0|0|0|0|0|0|0|0|1000000|
|1|0|0|0|0|0|0|0|0|0|0|0|0|1000000|
|2|0|0|0|0|0|0|0|0|0|0|0|0|1000000|
|3|0|0|0|0|0|0|0|0|0|0|0|0|1000000|
|4|0|0|0|0|0|0|0|0|0|0|0|0|1000000|
|5|0|0|0|0|0|0|0|0|0|0|0|0|1000000|

**Tabla 8**. Caudal extradido de la fuente
|0|1|
| -- | -- |
|0|10|
|1|20|
|2|30|
|3|40|
|4|50|
|5|60|

A continuación, se listan los resultados que arroja el código.

**Tabla 9**. Archivos de salida
|Archivo | Descripción| unidad |
| -- | -- | -- |
| CN_Results.csv | Concentración de nitrógeno que entra a cada elemento | mg/l |
| CP_Results.csv| Concentración de fosforo que entra a cada elemento | mg/l |
| CSed_Results.csv| Concentración de sedimentos que entra a cada elemento | mg/l |
| Q_Results.csv| Caudal que pasa por cada elemento | m^3 |
| Results_Order.csv| Orden de la topología resuelta| Ad|
| WN_Results.csv| Carga de nitrógeno que entra cada elemento | Kg |
| WN_Ret_Results.csv | Carga de nitrógeno que retiene cada elemento | Kg |
| WP_Results.csv | Carga de nitrógeno que entra cada elemento | Kg |
| WP_Ret_Results.csv | Carga de fosforo que retiene cada elemento | Kg |
| WSed_Results.csv | Carga de nitrógeno que entra cada elemento Caudal extraído| Ton|
| WSed_Ret_Results.csv | Carga de sedimentos que retiene cada elemento | Ton |

