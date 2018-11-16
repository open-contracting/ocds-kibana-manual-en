Although most of this manual's information can be used to analyze any data set, our efforts concentrate on analyzing Open Contracting Data published by the Ministry of Finance and Public Credit.

## Open Contracting Data
Open Contracting Data has existed for many years in Mexico. Our records are available at the [CompraNet] web page(http://compranet.funcionpublica.gob.mx/), which has an Excel file with information since 2002. This data has been analyzed thoroughly throughout time, and is available in a variety of civic tech that makes it simpler to evaluate. These are some that are currently accessible:
* [Who's Who Wiki](https://www.quienesquien.wiki)
* [ContratoBook](http://contratobook.org/#/contratos)
* [CompranNetFacil](http://compranetfacil.com/)
* [Data Analytics for Procurement](http://mexico.procurement-analytics.org/#/analysis/summary)

## OCDS Mexican Data
Since 2015 different Mexican Public administration projects have emerged in order to publish data in OCDS. The first one available was [Mexico City] website(http://www.contratosabiertos.cdmx.gob.mx/contratos). It is published by the government database source in OCDS [Ministry of Finance and Public Credit](https://www.gob.mx/contratacionesabiertas/home) , and this is the data used in this manual.

Furthermore, it is worth noting the effort made by [Open Contracting Partnership in Mexico](https://www.contratacionesabiertas.mx/) to translate OCDS into Spanish; other organizations that publish in OCDS are [National Institute for Transparency, Access to Information and Personal Data Protection](http://contratacionesabiertas.inai.org.mx), Mexico City Airport Group that publishes [contracts to build the New Airport](https://datos.gob.mx/nuevoaeropuerto/), the Secretariat of Communications and Transportation publishes [contracts related to Shared Network project](https://datos.gob.mx/redcompartida/) and at a subnational level, we can find [Secretariat of Planning, Administration and Finance of the Government of Jalisco](https://contratacionesabiertas.jalisco.gob.mx/contratosabiertos/). 

## OCDS Data in Latin America

Along with Mexico, five more countries in Latin America have six OCDS datasets:
* [ChileBuys](http://www.chilecompra.cl/)
* [Colombia Buys Efficient](https://www.colombiacompra.gov.co/transparencia/gestion-documental/datos-abiertos)
* [The Public-Private Partnerships In Honduras](http://app.sisocs.org/index.php?r=Ciudadano/index)
* [Open Contracting in Paraguay](https://www.contrataciones.gov.py/datos/api/v2/#!/ocds) 
* [Open Data Portal of the Paraguay's Ministry of Finance](https://datos.hacienda.gov.py/)
* [State Purchases of Uruguay's Public Procurement and Contracting Agency](https://www.comprasestatales.gub.uy/inicio/datos-abiertos/)

In the manual [OCDS Kingfisher tool](https://ocdskingfisher.readthedocs.io/en/master/index.html), it is explained how to download and import them.

## Data for this manual
As we have mentioned in the previous section, we will be using data published by the Ministry of Finance and Public Credit's Budget Transparency area, which is available over here: [https://www.gob.mx/contratacionesabiertas](https://www.gob.mx/contratacionesabiertas)

More specifically, we will be working with the Open Contract section of the Federal Public Administration in JSON format, obtained through this URL: [https://datos.gob.mx/busca/dataset/concentrado-de-contrataciones-abiertas-de-la-apf/resource/0252e19f-bdd6-43de-af7b-106d4c7a82c8](https://datos.gob.mx/busca/dataset/concentrado-de-contrataciones-abiertas-de-la-apf/resource/0252e19f-bdd6-43de-af7b-106d4c7a82c8)

This dataset includes contracting data conducted by each federal government department, autonomous organizations, State-owned companies and all those state and citywide contracts with a contribution from the government. Data in this format is available since 2017 and is constantly updated. Even though the file update frequency is not indicated in the data portal, and the last version is from 2018, you can find more recent data with the API described in this document: [http://transparenciapresupuestaria.gob.mx/work/models/PTP/programas/OpenDataDay/Resultados/Guia%20_uso_API_contrataciones%20_abiertas.pdf](http://transparenciapresupuestaria.gob.mx/work/models/PTP/programas/OpenDataDay/Resultados/Guia%20_uso_API_contrataciones%20_abiertas.pdf)

In addition to that, we could get a full data dump in OCDS, by requiring directly to the responsible area.

In the following chapter, we will explore in depth the tools needed to analyze this data and import it.
