# Alfresco Content Services: Solr to Elasticsearch Migration

## License

The code in this repository is released under the Apache License, see the [LICENSE](./LICENSE) file for details.

## Instructions

### Downloading the Custom Aspects File

Currently, within the `solrToEs/01-Solr` directory, there is a file `customContentModel.xml`. Right click on this file and **Move To Trash**.

Now that you've moved the file to the trash, we need to get an updated version. 

1. Open Terminal
2. Change directories to `solrToEs/01-Solr`. This can be done by typing two separate commands:
```
cd solrToEs
cd 01-Solr
```
Alternatively, you can also press **Tab** to auto populate the filepath. 

3. Now that we're in the apropriate directory, we can pull the file from GitHub.  
```
wget https://github.com/AntonioSoler/SolrToElastic/raw/main/solrToEs/01-Solr/customContentModel.xml
```
This will download the new XML file into your 01-Solr folder. If you have issues, you can download the file directly from the GitHub project.

### Starting the Solr Environment

1. Test the Docker installation with `sudo docker run hello-world`.
   * You may be prompted to enter the password for Administator. This is **password**.
    
2. If this runs successfully, it's time to deploy the `01-Solr` environment.
   * Ensure you are in `solrToEs/01-Solr` directory.

```
sudo docker compose up --force-recreate
```
The images have already been downloaded to the VMs - the containers will spin up. This may take a few minutes.

### Uploading the Custom Content Model

1. With the Solr environment running, open a web browser and navigate to **localhost:8080/workspace**.
2. Select **Custom Personal Files**.
3. Select **Data Dictionary**.
4. Select **Models**.
5. Click _Upload_.
   * Open the **solrToEs** folder.
   * Open the **01-Solr** folder.
   * Select the **customContentModel.xml file.**
6. Select the file in Workspace and open the _Properties_
7. Open the 


# Extra Steps - to be deleted if VMs are already populated.

### Quay.io

You should have already submitted the form for temporary Quay.io credentials. If not, you can find the instructions [here]. 

Ensure that you have Docker and Docker Compose installed. You may need to run these with the `sudo` command.

### Docker

* Docker
  ```
  apt-get update
  apt-get install docker
  ```

* Docker Compose
  ```
  apt-get update
  apt-get install docker-compose
  ```


  [here]:https://github.com/GBHyland/Alfresco-TechQuest-Class-Preparation
