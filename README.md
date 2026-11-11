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












