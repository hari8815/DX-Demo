# SFDX  App

## Dev, Build and Test


## Resources


## Description of Files and Directories


## Issues

sfdx force:project:create --projectname

sfdx force:auth:web:login -d -a dx

sfdx force:org:create -f config/project-scratch-def.json -d 30 --setalias

sfdx force:org:open -u dx

sfdx force:mdapi:retrieve -r ./mdapipkg  -k ./package.xml -u dx

sfdx force:mdapi:convert -r mdapipkg

sfdx force:source:push -u s1

sfdx force:package:install --package 04tB00000009XuZIAU -u s1

force-app/main/default/objects/cloudx_cms__SS_Carousel_Slide__c/fields/cloudx_cms__Text_Alignment__c.field-meta.xml  Cannot modify managed object: entity=CustomFieldDefinition, component=00NN0000004IH8j, field=PicklistControllerEnumOrId, state=installed
