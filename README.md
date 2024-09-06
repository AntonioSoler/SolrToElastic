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
       * **Aspect**: "ConfidentialAspect [Libson:ConfidentialAspect]"
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
27. Scroll to Aspects and verify that the _Confidental Aspect [Lisbon:ConfidentialAspect]_ is there. (_Note:_ Scroll to the top and notice that the Type is cm:content)

28. Click the Parent to go up, then click the Parent again to get to the Shared folder.

29. Scroll to _Children_ and select **cm:Type**.
30. Scroll to _Children_ and select **cm:mediation-filter.yml**.
31. Scroll to _Aspects_ and verify that the _Confidental Aspect [Lisbon:ConfidentialAspect]_ is there. (_Note:_ Scroll to the top and notice that the Type is _LisbonType [Lisbon:CustomType_)
32. To verify that both yml files have been uploaded, go to Query, select fts-alfresco, and execute a search for yml.
33. In Left Navigation Panel, select Search Service.
34. Verify that the system is configured to use Solr 6.
  


  [here]:https://github.com/GBHyland/Alfresco-TechQuest-Class-Preparation?tab=readme-ov-file#alfresco-migrations-solr-to-elasticsearch--acs-embedded-workflows-to-aps
