# PY_SMARTDATA_2026
PY_SMARTDATA_2026 -G18
MARICELA ZULOAGA VALERO

------ CONFIGURACION DEL ENTORNO  -----------------------

**1.GRUPO DE RECURSOS: **rg_smartdatagr18**
**2.AZURE DATA LAKE STORAGE GEN2 :** 
Nombre: **pysmartdata2026**
Region: EAST US 2
Primary Service: Azure Blob Storage or Azure Data Lake Storage
Primary Workload: Big Data Analytics 
Perfomance: Standard 
Redundancy: Locally redundant storage (LRS)
Advanced > Enable Hierarchical namespace : OK, habilitado
  
**- Contenedores**
  raw
  bronze
  silver
  golden
  metastore
En el contenedor raw se almacena los archivos .cvs que corresponde a las fuentes de datos a utilizar en el proyecto
1.Facturacion_Acueducto_202607-202609.csv
2.RecaudosAcueducto_202607en adelante.csv


**3.	AZURE DATABRICKS WORKSPACE**

*AMBIENTE CONSTRUCCIÓN*
Nombre workspace : adbsmartdata2026
grupo de recursos: rg_smartdatagr18
storage account :pysmartdata2026

*AMBIENTE PRODUCCIÓN*
Nombre workspace : adbpysmartdata2026
grupo de recursos: rg_smartdatagr18
storage account :pysmartdata2026
Cluster CLusterSD

Ambos ambientes comparten el mismo grupo de recuros y storage account y el cluster se encuentra en el ambiente de producción

** 4. ACCESS CONECTOR**
Nombre : ac-smartdata
ID Recurso - Identificador para configurar el Storage Credential en Databricks:
/subscriptions/dd9caeb8-47e7-487b-bc7f-8b85d0370d5c/resourceGroups/rg_smartdatagr18/providers/Microsoft.Databricks/accessConnectors/ac-smartdata

En el storage account **pysmartdata2026** se configura la storage credential

>Control Acceso (IAM)  > Add roles > Storage Blob Data Contributor > Manage Identity > **ac-smartdata**

**5.	MANAGED IDENTITY**
Nombre :  miadbspymartdata2026

**6. CREACIÓN DE CREDENCIALES**
Nombre: **credential_smardata**
Access Conector ID:  
/subscriptions/dd9caeb8-47e7-487b-bc7f-8b85d0370d5c/resourceGroups/rg_smartdatagr18/providers/Microsoft.Databricks/accessConnectors/ac-smartdata

** 7. CREACIÓN DE EXTERNAL LOCATION **
Nombre :**extl_pysmartdata2026**
URL : abfss://raw@pysmartdata2026.dfs.core.windows.net/
Storage credential: credential_smardata

**8. CREACION DE CLUSTER**
Nombre : **CLUSTERSD**
Solo en ambiente de produccion

-------------- DESARROLLO DEL PROYECTO -----------------

En la carpeta / procesos se encuentran los notebook para la preparacion de la ETL del proyecto, asi como los procesos de Ingesta y transformacion.

1. Preparacion Ambiente : preparar el ambiente con arquitectura medallion y acceso con external location

2.Ingesta_facturacion_data_bronze: Este notebook tiene la responsabilidad de realizar la ingesta de los datos de facturacion ubicados en /raw e insertarlos en la capa bronze

2.Ingesta_recaudos_data_bronze :  Este notebook tiene la responsabilidad de realizar la ingesta de los datos de recaudos ubicadod en /raw e insertarlos en la capa bronze

3.Transformacion_facturacion_Bronze_a_Silver: 
Este notebook tiene las siguientes responsabilidades:
Leer la tabla facturacion de la capa bronze
Realizar limpieza y validacion de los datos por medio de las siguientes reglas de negocio de Aguas de Manizales
Escribir los registros válidos en silver.facturacion_transformed y los rechazados en silver.facturacion_rechazados

3.Transformacion_recaudos_Bronze_a_Silver:
Este notebook tiene las siguientes responsabilidades:
Leer la tabla de recaudos de la capa bronze
Realizar limpieza y validacion de los datos por medio de las siguientes reglas de negocio de Aguas de Manizales
Escribir los registros válidos en silver.recaudos_transformed y los rechazados en silver.recaudos_rechazados


4.Load_Golden:
Se inicia con una validación de conteos y cuadre de valores contra la capa silver y seguidamente con la carga de informacion a las tablas:

kpi_facturacion_periodo
kpi_recaudo_periodo
kpi_cartera_suscriptor
kpi_indice_recaudo
kpi_calidad_datos
kpi_detalle_suscriptor (cartera + consumo m3 y nombre del cliente por suscriptor y periodo)
Cada tabla se sobrescribe completa en cada ejecución (insertInto(overwrite=True)), así los periodos con pagos tardíos se actualizan sin duplicar registros.

Capa bronze:
Facturacion
Recaudos

Capa silver:
facturacion_transformed
facturacion_rechazados
recaudos_transformed 
recaudos_rechazados

Capa golden:
kpi_facturacion_periodo
kpi_recaudo_periodo
kpi_cartera_suscriptor
kpi_indice_recaudos
kpi_calidad_datos

5. Grant








