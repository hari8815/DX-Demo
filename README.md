# SFDX  Deployment Issue.
In this repo, we have created a sample DX project to reproduce the field dependancy deployment issue. 

## Prerequisites 
1. Salesfore CLI 
2. Salesforce DX org and DevHub enabled
3. Any Managed package from Appexchange.
    For ex: **Lightning Carousel and Banner package** [Link](https://appexchange.salesforce.com/appxListingDetail?listingId=a0N3A00000EFp50UAD)

## Steps to reproduce the issue:
1. Create New DX Project.
  ```
  sfdx force:project:create --projectname DemoApp
  ```
2. Connect your DevHub Org and set alias as dx.
  ```
  sfdx force:auth:web:login -d -a dx
  ```
3. Create new scracth Org and set alias as s1
  ```
  sfdx force:org:create -f config/project-scratch-def.json -d 30 --setalias
  ```
4. Install **Lightning Carousel and Banner package** on DevHub and Scratch Org.
  ```
  sfdx force:package:install --package 04tB00000009XuZIAU -u dx -w 10
  sfdx force:package:install --package 04tB00000009XuZIAU -u s1
  ```
5. Now open the DevHub org and create field dependancy in the managed package object.
  - Open salesforce with following command
  `sfdx force:org:open -u dx`
  - Go to setup, naviagte to objects.
  - Open Carousel Slide object.
  - Click Field dependancy buttton in Custom Fields section.
  - Click New  
  - Select Caption Position as Controlling Field and Text Alignment as Dependent Field.
  - Include random values and save the record.
6. Now get back to the DX project folder and Create Package.xml file with only these dependant fields.
7. Run the follwoing command to retreive source data based on the Package.xml
  ```
  sfdx force:mdapi:retrieve -r ./mdapipkg  -k ./package.xml -u dx
  ```
8. Unzip the downloaded metadata and convert into dx format using following command.
  ```
  sfdx force:mdapi:convert -r mdapipkg
  ```
9. Now push this changes to scratch org using following command.
  ```
  sfdx force:source:push -u s1
  ```
10. You may get the follwoing deployment error due the dependant fields.

**force-app/main/default/objects/cloudx_cms__SS_Carousel_Slide__c/fields/cloudx_cms__Text_Alignment__c.field-meta.xml  Cannot modify managed object: entity=CustomFieldDefinition, component=00NN0000004IH8j, field=PicklistControllerEnumOrId, state=installed**

## Temporary fix for this issue:
Create the dependancy manually in the scratch org and deploy your changes.
