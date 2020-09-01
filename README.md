# emikat

"EMIKAT_ImpactCalculationModel.eap": current version = “<DataModel> Clarity Data Model (Version 2)”


## Overview
Emikat is used by the CSIS to perform a wide range of different calculations for the Studies created in the CSIS. Communication between Emikat and CSIS is established by a flow of individual REST calls between the endpoints of the two systems, which were created specifically for that purpose.

## General procedure
1. CSIS needs to trigger Emikat (by sending some initial information and receiving an Emikat's internal ID of the Study, which is later used to identify the Study in consecutive REST calls.) once the user has provided sufficient data for the Study and executed the Study calculation request.
2. Emikat requests additional data it needs from the Drupal REST endpoints, which were provided in the initial trigger request from the CSIS.
3. The CSIS then pulls information about the status of the current calculation progress in regular intervals until all calculations are completed. During the calculation the user cannot request a second triggering of the calculation.

## Individual REST endpoints
### 1) Initial Study information
**Link**: https://csis.myclimateservice.eu/rest/emikat/study/[**StudyID**]

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
- **ap_id**: ID of the adapted project
- **ap_title**: Title of the adapted project
- **ap_coefficient**: coefficient factor of the adapted project
- **ap_build_open_space**: Link to the selected adaptation strategy in the category build open space
- **ap_buildings**: Link to the selected adaptation strategy in the category buildings
- **ap_roads**: Link to the selected adaptation strategy in the category roads
- **ap_vegetated_area**: Link to the selected adaptation strategy in the category vegetated area
- **ap_area_left**: position of the left side of the bounding box for the adaptation project
- **ap_area_top**: position of the top side of the bounding box for the adaptation project
- **ap_area_right**: position of the right side of the bounding box for the adaptation project
- **ap_area_bottom**: position of the bottom side of the bounding box for the adaptation project
- **ap_area_wkt**: area of the adaptation project in the WKT (well-known-text) format

### 2) Data package information
**Link**: https://csis.myclimateservice.eu/node/[**DatapackageID**]?_format=hal_json

This endpoint is currently not implemented as a Drupal View, so the complete data package content is displayed instead using the default REST endpoint provided by Drupal.

### 3) Resources information
**Link**: https://csis.myclimateservice.eu/rest/emikat/dp/[**DatapackageID**]/resources

This endpoint uses the [REST Node View](https://csis.myclimateservice.eu/admin/structure/views/view/rest_node/edit/rest_export_6) and displays the following properties:
- **resource_description**: Description of resource (plain text)
- **resource_format**: format of resource (Geojson, jpeg, Shapefile, ...)
- **resource_url**: array of URLs to external source(s) of resource

### 4) Included Adaptation options in the selected Adaptation Strategy
**Link**: https://csis.myclimateservice.eu/rest/emikat/as[**AdaptationStrategyID**]

This endpoint uses the [REST Node View](https://csis.myclimateservice.eu/admin/structure/views/view/rest_node/edit/rest_export_8) and displays the following properties:
- **ao_name**: Name of the adaptation option
- **ao_layer_title**: The title of the layer
- **ao_partition_factor**: the partition factor of the adaptation option
- **ao_land_layer**: Boolean indication whether or not it is a land usage layer
- **ao_path_parameters**: Link to the individual parameters of the adaptation option
- **ao_cost_development**: the development cost of the adaptation option
- **ao_cost_maintenance**: the maintenance cost of the adaptation option
- **ao_cost_retrofitting**: the retrofitting cost of the adaptation option

### 5) Adaptation option information
**Link**: https://csis.myclimateservice.eu/rest/emikat/ao[**AdaptationOptionID**]

This endpoint uses the [REST Node View](https://csis.myclimateservice.eu/admin/structure/views/view/rest_node/edit/rest_export_9) and displays the following properties:
- **name**: Name of the adaptation option
- **field_le_parameter**: The parameter that is being modified by this adaptation option
- **field_operation**: The operation used in this adaptation option
- **field_unit**: The type of unit used
- **field_value**: The value used by this adaptation option

<hr>

### Additional REST endpoints

The complete description of REST endpoints provided by Emikat and their further specifications is available on [CSIS Wiki](https://github.com/clarity-h2020/csis/wiki/Services-endpoints-(used-by-CSIS)).

## Relevant batch jobs for checking calculation results
CSIS regularly checks the status of running calculations by requesting the list of batch jobs for a particular Study and analysing their current states.
Only batch jobs from a list of "productive batch jobs" should be tested and analysed. Others might have been just added temporarily by Emikat and are only used for testing or debugging and have no influence on the actual calculation results expected by the CSIS. Their status should therefore not be taken into account in CSIS. Therefore only a list of predefined batch jobs is analysed by the CSIS.

This list contains the following batch jobs (This list should be expanded, if new functionality is added.):

- Rebuild all views...
- Rebuild Table CLY_IMPACT_RESULT_HW#1838
- Rebuild Table CLY_HW_T_MRT#1856
- Rebuild Table CLY_HW_FLUXES#1856
- Rebuild Table CLY_HW_GRID_DETAILS_PROJ#1856
- Rebuild Table CLY_HW_GRID_DETAILS#1856
- Rebuild Table CLY_URBAN_ATLAS#1776
- Rebuild Table CLY_AO_LAYER_PARAMS#1796
- Rebuild Table CLY_EUROSTAT_CITIES_MORTALITY#2056
- Rebuild Table CLY_EL_POPULATION_INTERPOLATED#2016
- Rebuild Table CLY_UA_FRACTION#2016
- Rebuild Table CLY_ADAPTATION_OPT_ITEM#1837
- Rebuild Table CLY_ADAPTATION_OPTION#1837
- Rebuild Table CLY_PROJECT#1837
- Rebuild Table CLY_HAZARD_EVENTS_STUDY#2036
- Rebuild Table CLY_GRID_ETRS89_1K#1757
- Rebuild Table CLY_PARAMETER#1976
