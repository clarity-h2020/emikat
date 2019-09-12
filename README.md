# emikat

"EMIKAT_ImpactCalculationModel.eap": current version = “<DataModel> Clarity Data Model (Version 2)”


## Overview
In this issue we will describe the flow of individual REST calls between Emikat and CSIS, how to access the endpoints on Drupal-side and what content(-fields) should be expected in each response.

## General procedure
1.  CSIS needs to trigger Emikat (send all necessary information and receive Emikat's internal ID of our Study) once a new Study is created and sufficient data has been added, so that Emikat can start the calculations. **TODO**: We need to define how much data is "sufficient" here: #6.
2.  Emikat requests the data it needs from the Drupal REST endpoint (going to be either special views or the complete nodes exported as json or hal_json)
3. Emikat needs to trigger Drupal once results are available and Drupal needs to trigger Emikat once changes to the Study are made.

## Individual REST endpoints
### 1) Initial Study information
**Link**: csis.myclimateservice.eu/rest/emikat/study/[**StudyID**]

This link will be given to Emikat in the initial trigger event from the CSIS. BasicAuth is used as authentication. Emikat can use the "Emikat" user in the CSIS, which was created for this purpose.
This endpoint uses the [REST Group View](https://csis.myclimateservice.eu/admin/structure/views/view/rest_group/edit/rest_export_3) and displays the following properties:
- **study_title**: Name of the Study
- **study_description**: Description of the Study
- **study_area_left**: position of the left side of the bounding box
- **study_area_top**: position of the top side of the bounding box
- **study_area_right**: position of the right side of the bounding box
- **study_area_bottom**: position of the bottom side of the bounding box
- **path_data_package**: Link to the endpoint with additional information about the used data package
- **path_resources**: Link to View endpoint showing all resources in that Study/datapackage
- **path_selected_adaptation_options**: Link to View endpoint showing all selected Adaptation options in that Study

### 2) Data package information
**Link**: csis.myclimateservice.eu/node/[**DatapackageID**]?_format=hal_json

This endpoint is currently not implemented as a view, so the complete data package is displayed. This can be changed if necessary, so that only relevant fields are displayed.

### 3) Resources information
**Link**: csis.myclimateservice.eu/rest/emikat/dp[**DatapackageID**]/resources

This endpoint uses the [REST Node View](https://csis.myclimateservice.eu/admin/structure/views/view/rest_node/edit/rest_export_6) and displays the following properties:
- **resource_description**: Description of resource (plain text)
- **resource_format**: format of resource (Geojson, jpeg, Shapefile, ...)
- **resource_url**: array of URLs to external source(s) of resource

### 4) Selected Adaptation options overview
**Link**: /rest/emikat/study/[**StudyID**]/selected_adaptation_options

This endpoint uses the [REST Node View](https://csis.myclimateservice.eu/admin/structure/views/view/rest_node/edit/rest_export_7) and displays the following properties:
- **path_selected_adaptation_option**: path to endpoint for the selected Adaptation option

### 5) Adaptation option information
**Link**: https://csis.myclimateservice.eu/node/**[AdaptationOptionID]**?_format=hal_json

This endpoint is currently not implemented as a view, so the complete adaptation option is displayed. This can be changed if necessary, so that only relevant fields are displayed.

<hr>

### additional REST endpoints ???  
