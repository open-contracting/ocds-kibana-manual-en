# Starting up ELK Platform to Analize OCDS format Contracting

At the end of this section, you will have all you need to check and visualize OCDS format Contracting data.

In order to have a better understanding of this section, it is recommended to be familiar with the command terminal:

* [Mac](https://www.soydemac.com/abrir-terminal-mac/)
* [Windows](https://www.wikihow.com/Open-the-Command-Prompt-in-Windows)
* [Linux/Ubuntu](https://elblogdeliher.com/como-moverse-por-los-directorios-en-la-terminal-de-ubuntu/)

## Goals

1. Starting an ElasticSearch server with Kibana.
1. Uploading the Contracting published data in a format that is easy to check.
1. Checking data.

The current project is developed in order to start the 3 services easily and quickly.


## Prerequisites

1. Open the Operative System's command terminal
1. [Install Docker CE](https://docs.docker.com/install/)
1. [Install Docker Compose](https://docs.docker.com/compose/install/)
1. [Download the file containing the
   tools](https://github.com/ProjectPODER/ManualKibanaOCDS/archive/master.zip)
1. Unzip the downloaded file and access the folder that has just been created
    * In the terminal: `cd ManualKibanaOCDS-master`

## Start *Server Container* with ElasticSearch and Kibana

Now we can start the server by executing the following command in the terminal:
```
docker-compose -f elastic-kibana.yaml up
```
> This command indicates Docker program to create the container shown in the file
> `elastic-kibana.yaml`, here we specify both programs should start.

After some minutes, depending on available resources, we should be able to open this link
[http://localhost:5601/app/kibana](http://localhost:5601/app/kibana) and Kibana should appear as available.

From now on, ElasticSearch and Kibana will be ready to use, even though we still do not have available data.

## Upload OCDS data to ElasticSearch

For this process, we should be placed in the folder `elk-gobmx-csv-master/pipeline`, in the terminal:
```
cd pipeline
```

### Downloading data packages

If we want to work with all the Contracts published in OCDS standard without taking into account the last data published,
we should first be sure we have downloaded **Contracts in OCDS format by package.json**, published in the website
[datos.gob.mx](https://datos.gob.mx/busca/dataset/concentrado-de-contrataciones-abiertas-de-la-apf/resource/ed1ec7e5-61ae-4d00-8adc-67c77844e75c)
> On September 2 2018 , this file's name is `contratacionesabiertas_bulk_paquetes.json.zip` and its size is about 310.5 MB.

It is important to mention that this information is recognized with OCDS standard through the following format
[recordPackages](http://standard.open-contracting.org/latest/en/schema/record_package/) or [Record Package](http://standard.open-contracting.org/latest/es/schema/record_package/); all tools and codes included in this manual use this format. In order to use another one, such as *releases* or *releasePackages*, or another structure non-defined by OCDS standard, it would be required to modify the code.

> As these files can have a big size, it is recommended to have at least 2GB available in
> the hard disk to continue.

Now, we need to unzip `opencontracting_bulk_packages.json.zip` file, which will create multiple
`.json` files inside a folder:
```
folder/opencontracting_bulk_packages.json
folder/opencontracting_bulk_packages2.json
folder/opencontracting_bulk_packages3.json
...
```

**IMPORTANT:** We must know the full file path of .json files to this folder, as it will be necessary for the upload stage.

As an example, suppose that the files were downloaded and unzipped inside the same operative system Download file. Files path to this folder **should be**
[<sup>1</sup>](https://en.wikipedia.org/wiki/Home_directory#Default_home_directory_per_operating_system)

* **Linux/Ubuntu/Mac**: `/home/{username}/Download`
    We can shorten it as `$HOME/Download`
* **Windows**: `C:\Users\{username}\Download`
    We can shorten it as `%HOMEPATH%\Download`

When it is confirmed or we could get the path to the files folder, we can continue.

## Processing and uploading data

### IMPORTANT
**The current process specifically upload the `compiledRelease` part of each OCDS document, based on analyzing the last version available among OCDS *releases*. We recommend to read previous chapters before continuing**

In this same folder, we have another tool designed for data upload. It also uses a Docker container. We will just use two commands: the first one is to get the container ready and the second one to execute it.

```
docker build . -t logstash-sfp-compranet-ocds:latest
```
> With this command, Docker will make the container ready with everything necessary to process and upload data.

Once finished, we can execute the upload process according to the available operating system.

**Linux/Ubuntu o Mac**
```
docker run --net="host" -v $HOME/Download:/input logstash-sfp-compranet-ocds
```
**Windows**
```
docker run --net="host" -v %HOMEPATH%\Download:/input logstash-sfp-compranet-ocds
```
>This command will use the container that is already executing the data processing and upload. For more information, please check the [technical documentation](https://github.com/ProjectPODER/ManualKibanaOCDS/tree/master/pipeline) of this
> process.

The screen now must show the process information. This could take some minutes.

After finishing everything successfully, this caption should pop up: `Upload successfully complete` and `Kibana is ready to use`.

Now, we can visit [Kibana] web page(http://localhost:5601/app/kibana) and see all data uploaded.

---

To know more about technical details on how to upload data, we will explain next how to use LogStash for this process.

### Extra: Download OCDS data directly from datos.gob.mx API

Previously, we explained how to download the OCDS dataset through a single file. In this section, we will introduce an alternative to download only the contracts we are looking for or the most updated contracts that have not been published yet in the final file. For this, we will use datos.gob.mx API, provided by the Mexican government.

> In order to see the full API documentation, please check [Basic guide to use API](http://transparenciapresupuestaria.gob.mx/work/models/PTP/programas/OpenDataDay/Resultados/Guia%20_uso_API_contrataciones%20_abiertas.pdf)
> where you can see in detail the specific filter options.

In order to download and manipulate data, the following tools will be used: [cURL](https://es.wikipedia.org/wiki/CURL)
and [jq](https://es.wikipedia.org/wiki/Jq).

> The `curl` command will enable us to download information automatically and `jq` command will help us to
> give a user-friendly format to JSON data.
> At a lager stage, this manual includes a brief introduction to `jq`.

--- 
Both programs can be installed locally. For example, for Linux Ubuntu, it can be done with an instruction such as:
```
sudo apt-get install -y curl jq
```
For Windows or Mac, we should download the executable files separately, but we can also use the
*docker container* included in the current code. To this effect, we should only execute the following docker command:
```
docker run --rm -it -v $HOME/Download:/input --entrypoint=bash logstash-sfp-compranet-ocds
```
> Remember that `$HOME/` is a shortcut only for Linux and Mac, in Windows we shall use `%HOMEPATH%\`

When this command is executed, we will get a new command line, which must look like this:
```
bash-4.2$
```
In this [command line]((https://es.wikipedia.org/wiki/Bash) we can execute the following commands.

---

If you want to download the last 100 contracting processes and keep them in a `.json`file:
```
curl https://api.datos.gob.mx/v2/contratacionesabiertas | jq -crM ".results" > opencontracting_last_100.json
```

> IF we want to keep the created files inside the container, we should move them or create them in
> the shared files between the computer and the container. Otherwise, these files will be lost
> when the container is "shut down".

If we want to download the contracting processes that involve a certain business unit (unidad compradora)(it is limited to 1000, but it could be changed).
```
curl https://api.datos.gob.mx/v2/contratacionesabiertas?records.compiledRelease.parties.name=Servicio%20de%20Administraci%C3%B3n%20Tributaria&pageSize=1000&page=1 | jq -crM ".results"  > opencontracting_SAT_1000.json
```

In order to understand this last command, we will detail each section:
* Firstly, `curl` command is used
* Secondly, this URL API based is included: [https://api.datos.gob.mx/v2/contratacionesabiertas](https://api.datos.gob.mx/v2/contratacionesabiertas)
* Next, we have the filter parameters:
    - `records.compiledRelease.parties.name`: filters according to that field value, that is, the name of some sections in the contract.
    - `pageSize`: details how many results it will give in each request
    - `page`: it allows asking for the following pages, in case there is more than one.
* Afterwards, we will use the `jq` command to extract only the results.
* Lastly, we indicate that the operation result should be stored in a file,
  it is important that this file's name
  shows the query undertaken to be later filled.

> These files must be stored and treated just as in the previous section,  
> placing them in the download section so we can continue with our next step.
