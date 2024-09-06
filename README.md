# Alfresco Content Services: Solr to Elasticsearch Migration

## License

The code in this repository is released under the Apache License, see the [LICENSE](./LICENSE) file for details.

## Instructions

### Preliminary

#### Quay.io

You should have already submitted the form for temporary Quay.io credentials. If not, you can find the instructions [here]. 

Ensure that you have Docker and Docker Compose installed. You may need to run these with the `sudo` command.


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

1. Test the Docker installation with `docker run hello-world`.
   * You may be prompted to enter the password for Administator. This is **password**.
  
2. Enter `docker login quay.io`. You will need your Quay.io credentials from earlier.
   * You will be prompted for a Username and Password. 
    
3. If this runs successfully, it's time to deploy the `01-Solr` environment.
   * Ensure you are in `solrToEs/01-Solr` directory.

```
docker compose up
```
The images have already been downloaded to the VMs - the containers will spin up. This may take a few minutes.

### Uploading the Trial License

1. Navigate to **localhost:8080/alfresco**.
2. Select **Alfresco Administration Console (admin only)**.
   * The User/Password is admin/admin
3. Select **License**
4. At the bottom of the page, click _Upload License_.
5. Select the Paper/Green Arrow icon.
6. The file explorer will appear. Select **alf23-5user-1000doc-allenabled-till-30-09-24.lic**.
7. Click _Select_ in the upper right corner of the file explorer.
8. Click _Upload_.
9. You will know the license was successfully uploaded when you see the text, "The license was successfully uploaded and applied to the repository."
10. Click _Close_. You have now uploaded the license.

### Uploading the Custom Content Model

1. With the Solr environment running, open a web browser and navigate to **localhost:8080/workspace**.
   * The User/Password is admin/admin
2. Select **Custom Personal Files**.
3. Select **Data Dictionary**.
4. Select **Models**.
5. Click _Upload_.
   * Open the **solrToEs** folder.
   * Open the **01-Solr** folder.
   * Select the **customContentModel.xml** file.
6. Select the file in Workspace and open the _Properties_.
7. Open **Dictionary Model** and click the _pencil icon_ to make edits.
8. Select **Model Active** and click the _checkmark icon_ to save.

### Applying Types and Aspects

1. Go to **Custom Personal Files**.
2. Select **Shared**.
3. Create two folders: _Aspects_ and _Types_.
4. Right click on the **Aspects** folder.
   * Select **Manage Rules**, then **Create Rule**.
   * Name the Rule **Aspect Rule**.
   * Set the following specifications:
       * **When**: Items are created or enter this folder
       * **Perform Actions**: Add aspect
       * **Aspect**: "ConfidentialAspect [Libson:ConfidentialAspect"
       * **Other Options**: Rule applies to subfolders
5. Click _Create_.




  [here]:https://github.com/GBHyland/Alfresco-TechQuest-Class-Preparation?tab=readme-ov-file#alfresco-migrations-solr-to-elasticsearch--acs-embedded-workflows-to-aps
