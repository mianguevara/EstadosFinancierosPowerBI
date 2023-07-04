## **Guía paso a paso para realizar un análisis de estados financieros con Power BI**
**Introducción:** 

En este laboratorio práctico, aprenderemos a construir un tablero de control financiero utilizando Power BI. El objetivo principal es analizar los estados de resultados y balances generales de los años 2021 y 2022. Utilizaremos Power Query para realizar el proceso de Extracción, Transformación y Carga (ETL) de los datos financieros contenidos en los archivos 2021.xlsx y 2022.xlsx.

**Pasos a seguir:**

1. ETL: Comenzaremos por cargar los archivos de Excel en Power BI y utilizar Power Query para extraer y transformar los datos necesarios. Aplicaremos una serie de pasos para limpiar los datos, remover columnas innecesarias y organizarlos de manera adecuada para su análisis. Una vez finalizado el proceso de ETL, obtendremos los datos financieros necesarios para el análisis.
1. Modelo de datos: En esta etapa, crearemos un modelo de datos en Power BI. Esto implica establecer las relaciones entre las diferentes tablas, como los estados de resultados y balances generales de los años 2021 y 2022. También crearemos una tabla de calendario para facilitar el análisis temporal.
1. Creación de medidas: Para realizar análisis financiero, crearemos medidas para calcular indicadores clave de rendimiento (KPIs). Estas medidas incluirán ingresos, utilidad bruta, utilidad operativa, utilidad neta, patrimonio, activos, pasivos y otros indicadores financieros relevantes. Estas medidas nos permitirán evaluar el desempeño financiero de la empresa en los dos años seleccionados.
1. Creación del tablero de control: Llegamos al punto culminante del laboratorio, donde construiremos un tablero de control interactivo para visualizar y analizar los estados de resultados y balances generales de 2021 y 2022. Utilizaremos gráficos, tablas y KPIs para presentar la información financiera de manera clara y concisa. También incluiremos filtros y opciones de interacción para permitir a los usuarios explorar los datos según sus necesidades.

Con este laboratorio práctico, aprenderemos a utilizar Power BI para construir un tablero de control financiero efectivo. Analizaremos los estados de resultados y balances generales de los años 2021 y 2022, calcularemos indicadores clave de rendimiento y presentaremos la información de manera interactiva. ¡Comencemos con el laboratorio y aprovechemos el poder de Power BI para el análisis financiero!

**Paso 1: ETL**

1. Carga los archivos 2021.xlsx y 2022.xlsx en Excel.
1. Crea una columna personalizada utilizando la fórmula **=Excel.Workbook([Content],true)** en una celda vacía.
1. Elimina las columnas innecesarias.
1. Expande la columna personalizada para obtener los datos.
1. Filtra las columnas para seleccionar únicamente los datos de P&G.
1. Expande la columna "data" para obtener los valores.
1. Desactiva la dinamización de columnas que contienen los meses.
1. Agrega una columna de fecha a los datos.
1. Nomina la tabla resultante como "P&G".
1. Duplica la tabla "P&G" y renómbrala como "BG".
1. Elimina los pasos desde el filtro en adelante.

Repite los pasos anteriores desde el filtrado de columnas para los datos de la tabla "BG".

![](Images/Aspose.Words.f21801ad-dacc-40fa-89c6-f79186ad441d.001.png)

**Paso 2.1 Creación tablas auxiliares:**

1. Duplicar la tabla P&G y conservar solo el campo de la cuenta y dejar solo los campos únicos y renombrar la tabla como Cuenta detalle – P&G.
1. Agregar una columna índice que empiece desde 1.

Repite los pasos anteriores la tabla "BG" y renombrar la tabla como Cuenta detalle - BG.

![Tabla

Descripción generada automáticamente](Aspose.Words.f21801ad-dacc-40fa-89c6-f79186ad441d.002.png)

**Paso 2.2: Modelo de datos**

1. Crea una tabla llamada "calendario" con la información de las fechas.

Calendario = CALENDAR(MIN('P&G'[Fecha]),MAX('P&G'[Fecha]))

1. Crea los modelos de relaciones entre las tablas "P&G", "BG" y "calendario".

Mediante la columna date de Calendario y la columna fecha de "P&G", "BG"

1. Agrega una columna en la tabla "P&G" para indicar el trimestre.

Trimestre = "Trim " & QUARTER('P&G'[Fecha])

1. Crea una tabla resumida de "P&G" por temporalidad trimestral y año.

P&G Trimestre = SUMMARIZE(

`    `'P&G',

`    `'P&G'[Cuenta],

`    `'P&G'[Año],

`    `'P&G'[Trimestre],

`    `"Monto Trimestral", SUM('P&G'[Monto])

)

![Interfaz de usuario gráfica, Aplicación

Descripción generada automáticamente](Images/Aspose.Words.f21801ad-dacc-40fa-89c6-f79186ad441d.003.png)

**Paso 2.3: Creación de medidas**

1. Crea una tabla llamada "medidas" para almacenar las medidas calculadas.
1. En la tabla "medidas", crea las siguientes medidas primarias:
   1. Ingresos (Suma)

Ingresos = CALCULATE(SUM('P&G Trimestre'[Monto Trimestral]), 'P&G Trimestre'[Cuenta] = "Ventas" )

1. Utilidad neta (Suma)

UtilidadNeta = CALCULATE(SUM('P&G Trimestre'[Monto Trimestral]), 'P&G Trimestre'[Cuenta]="Utilidad neta" )

1. Patrimonio (Promedio)

Patrimonio = CALCULATE(AVERAGE(BG[Monto]), BG[Cuenta] = "Patrimonio" )

1. Activos (Promedio)

Activos = CALCULATE(AVERAGE(BG[Monto]), BG[Cuenta] = "Activo" )

1. Pasivo (Promedio)

Pasivo = CALCULATE(AVERAGE(BG[Monto]), BG[Cuenta] = "Pasivo" )

1. Crea las siguientes medidas secundarias (Ratios):
   1. Margen de utilidad neta

Margen = [UtilidadNeta] / [Ingresos]

1. Rentabilidad sobre el patrimonio

ROE = DIVIDE([UtilidadNeta],[Patrimonio])

1. Ratio de solvencia total

Ratio de Solvencia Total = [Activos] / [Pasivos]

1. Crea las siguientes medidas para el periodo anterior utilizando la función DATEADD:
   1. Ingresos periodo anterior trimestral (Suma)

Ingresos PA = CALCULATE(Ingresos], DATEADD(Calendario[Date],-1,QUARTER))

1. Utilidad neta periodo anterior trimestral (Suma)

UtilidadNeta PA = CALCULATE ([UtilidadNeta], DATEADD (Calendario[Date],-1, QUARTER))

1. Patrimonio periodo anterior trimestral (Promedio)

Patrimonio PA = CALCULATE ([Patrimonio], DATEADD (Calendario[Date], -1, QUARTER))

1. Activos periodo anterior trimestral (Promedio)

Activos PA = CALCULATE ([Activos], DATEADD (Calendario[Date], -1, QUARTER))

1. Pasivo periodo anterior trimestral (Promedio)

Pasivo PA = CALCULATE ([Pasivos], DATEADD (Calendario[Date], -1, QUARTER))

1. Calcular los mismos ratios del punto 3 pero esta vez utilizando los datos que contenga la información del periodo anterior (PA).

![Imagen de la pantalla de un celular con letras

Descripción generada automáticamente](Images/Aspose.Words.f21801ad-dacc-40fa-89c6-f79186ad441d.004.png)

**Paso 3: Creación del dashboard**

1. Crea 4 KPIs en el dashboard utilizando las medidas calculadas (Utilidad neta, Margen de utilidad neta, ROE, Ratio de solvencia total).
1. Crea filtros de temporalidad anual y trimestral para filtrar los datos.
1. Crea un gráfico de líneas para visualizar la evolución de las medidas a lo largo del tiempo de los Ingresos e Ingresos PA.
1. Crea un gráfico de anillo para comparar las diferentes categorías Activos, Pasivos y Patrimonio.
1. Crea un gráfico de barras para mostrar comparaciones entre diferentes elementos.

![Interfaz de usuario gráfica, Aplicación

Descripción generada automáticamente](Images/Aspose.Words.f21801ad-dacc-40fa-89c6-f79186ad441d.005.png)

¡Felicitaciones! Has completado un análisis de estados financieros en Power BI. Ahora puedes utilizar estos datos para análisis, visualizaciones o cualquier otro propósito necesario.

