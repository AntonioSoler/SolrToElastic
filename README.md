# Alfresco Content Services: Solr to Elasticsearch Migration

## License

The code in this repository is released under the Apache License, see the [LICENSE](./LICENSE) file for details.

## Preparing for Migration

### Preliminary

#### Quay.io

You should have already submitted the form for temporary Quay.io credentials. If not, you can find the instructions [here]. 

Ensure that you have Docker and Docker Compose installed. You may need to run these with the `sudo` command.


### Downloading the Custom Aspects File

Currently, within the `solrToEs/01-Solr` directory, there is a file `customContentModel.xml`. Right click on this file and **Move To Trash**.

Now that you've moved the file to the trash, we need to get an updated version. 

1. Open Terminal
2. we can pull the file from GitHub.  
```
wget https://github.com/AntonioSoler/SolrToElastic/raw/main/solrToEs.zip
```
This will download the new zip file into your folder, then use:

```
unzip solrToEs.zip
```

If you have issues, you can download the entire repository from GitHub project.

https://github.com/AntonioSoler/SolrToElastic

### Starting the Solr Environment

1. Test the Docker installation with `docker run hello-world`.
   * You may be prompted to enter the password for Administator. This is **password**.
  
2. Enter `docker login quay.io`. You will need your Quay.io credentials from earlier.
   * You will be prompted for a Username and Password. 
    
3. If this runs successfully, it's time to deploy the `01-Solr` environment.
   * Ensure you are in `solrToEs/01-Solr` directory.
  
We need to create in advance an external network to connect later Elastic's docker container with this one

```
docker network create myNetwork 
```

Then bootstrap the environment with the command:
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
       * **Aspect**: "DossierAspect [Libson:DossierAspect]"
       * **Other Options**: Rule applies to subfolders
5. Click _Create_.

6. Migrate back to the **Shared** folder.

7. Right click on the **Types** folder.
   * Select **Manage Rules**, then **Create Rule**.
   * Name the Rule **Types Rule**.
   * Set the following specifications:
       * **When**: Items are created or enter this folder
       * **Perform Actions**: Specialise Type
       * **Aspect**: "LisbonType [Libson:CustomType]"
       * **Other Options**: Rule applies to subfolders
8. Click _Create_.

9. Migrate back to the **Shared** folder.

10. Select the **Types** folder
11. Click _Upload_.
12. Open **solrToEs** and then **02-ES**.
13. Select and _Upload_ **mediation-filter.yml**. This will act as a test file for re-indexing.

14. Migrate back to the **Shared** folder.

15. 10. Select the **Aspects** folder
16. Click _Upload_.
17. Open **solrToEs** and then **01-Solr**.
18. Select and _Upload_ **docker-compose.yml**. This will act as a test file for re-indexing.

19. Navigate to **localhost:8080/alfresco**.
20. Select **Alfresco Administration Console (admin only)**.
   * The User/Password is admin/admin
21. On the Left Navigation Panel, scroll down to _Support Tools_, then select **Node Browser**.
22. Click _Root List_.
23. Scroll to Children and select **app:company_home**.
24. Scroll to Children and select **app:shared**.
25. Scroll to Children and select **cm:Aspect**.
26. Scroll to Children and select **cm:docker-compose.yml**.
27. Scroll to Aspects and verify that the _DossierAspect Aspect [Lisbon:DossierAspect]_ is there. (_Note:_ Scroll to the top and notice that the Type is cm:content)

28. Click the Parent to go up, then click the Parent again to get to the Shared folder.

29. Scroll to _Children_ and select **cm:Type**.
30. Scroll to _Children_ and select **cm:mediation-filter.yml**.
31. Scroll to _Aspects_ and verify that the _Confidental Aspect [Lisbon:ConfidentialAspect]_ is there. (_Note:_ Scroll to the top and notice that the Type is _LisbonType [Lisbon:CustomType_)
32. To verify that both yml files have been uploaded, go to Query, select fts-alfresco, and execute a search for yml.
33. In Left Navigation Panel, select Search Service.
34. Verify that the system is configured to use Solr 6.
  
## Starting the Migration

1. Navigate to **localhost:8080/alfresco**.
2. Select **Alfresco WebScripts Home (admin only – INTERNAL)**.
3. Select **Browse by Web Script Package**.
4. Select **/alfresco/model**.
5. Select **GET alfresco/s/model/ns-prefix-map**.
6. Highlight and copy this text.
   * Open the _Text Editor_ and paste this text here or from the browser select "Save as"
   * Save the document as **ModelPrefixes.json** to the `solrToEs/02-ES/re-indexing` folder. (

7. In the file explorer, navigate to the `solrToEs/02-ES` folder.
8. Open the **docker-compose.yml** file.
9. Edit line 69 as shown below:
   * ./re-indexing/**ModelPrefixes.json**:/opt/reindex.prefixes.json
10. Save the **docker-compose.yml** file.
11. edit the **solrToEs/02-ES/re-indexing/mediation-filter.yml**
12. we want to exclude the Confidential files from the indexing so we need to add the confidential Aspect after line 6 and put the lisbon:ConfidentialAspect

### Deploying Elasticsearch

1. Open a new tab in Terminal
   * The new tab will probably open in the 'solrToEs/01-Solr` directory. You need to be in the `solrToEs/02-ES` directory. You can do this by entering these two lines separately:
     ```
     cd ..
     cd 02-ES
     ```
2. Run the command `docker compose up elasticsearch`.
3. In the Web Browser, navigate to **localhost:9200/_cat/indices**. We can confirm that Elasticsearch is running.
4. the web page should show only one line as the default database is the only one present

### Connecting to Elasticsearch

1. Navigate to **localhost:8080/alfresco**.
2. Select **Alfresco Administration Console (admin only)**.
   * The User/Password is admin/admin
3. In the Left Navigation Panel, select **Search Service**.
4. Go to the _Search Service in User_ drop-down menu and slect **Elasticsearch**.
5. Go to _Elasticsearch Hostname_ and name it **elasticsearch**.
6. Click _Save_.
7. Refresh the page and select **Test Connection**. This should be successful.
8. Navigate to **localhost:9200/_cat/indices**. There will be a new index created "Alfresco" value (2 lines shown).

### Re-Indexing

1. Open a new tab in Terminal. This should still be in the `solrToEs/02-ES` directory.
2. Run the command `docker compose up re-indexing`.
3. This will exit with **Code 0**. Nothing to worry about - it means it was successful.
4. Navigate to **localhost:8080/alfresco**.
5. Select **Alfresco Administration Console (admin only)**.
   * The User/Password is admin/admin
6. In the Left Navigation Panel, select **Node Browser**.
7. Select _Query_.
8. Select **fts.alfresco** from the drop-down menu.
9. Execute a search for **yml**.

### Live Indexing

1. In the terminal, ensure you are in the `solrToEs/02-ES`.
2. Run the command `docker compose up live-indexing`.
3. Open the Web Broswer and navigate to **localhost:8080/workspace**.
4. Select **Custom Personal Files**.
5. Select **Shared**.
6. Select the **Aspects** folder.
7. Select **Upload**.
8. Within the File Explorer, upload **solrToEs.zip**.

### Testing Elasticsearch Live Indexing

1. Navigate to **localhost:8080/alfresco**.
2. Select **Alfresco Administration Console (admin only)**.
   * The User/Password is admin/admin
3. In the Left Navigation Panel, select **Node Browser**.
4. Go to _Query_
5. Select **fts-alfresco** from the drop-down menu.
6. Execute a search for **zip**. You should see the **solrToEs.zip** file - confirming that the Solr to Elasticsearch migration has occured successfully.

We can do one more test to confirm that Solr is no longer indexing.


### tail-Indexing

In the event that some unexpected event happens during the server operation there is the option to reindex a range of dates using the reindexing component with different parameters

You can see the configuration in **the docker-compose.yml** file in the section  **tail-re-rendexing**

1. Open a new tab in Terminal. This should still be in the `solrToEs/02-ES` directory.
2. Run the command `docker compose up tail-re-indexing`.
3. This will exit with **Code 0**. Nothing to worry about - it means it was successful.

Follow steps 1-3, but switch to Elasticsearch to move back to your newly migrated and configured search service! Congratulations!

  [here]:https://github.com/GBHyland/Alfresco-TechQuest-Class-Preparation?tab=readme-ov-file#alfresco-migrations-solr-to-elasticsearch--acs-embedded-workflows-to-aps
