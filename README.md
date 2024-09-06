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
