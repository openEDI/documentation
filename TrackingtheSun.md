# Tracking the Sun

The Tracking the Sun dataset is provided in Parquet format partitioned by city, this allows data queries using Glue/Athena. However, with the development of Tracking the Sun dataset, the dataset 
schema/columns may change each year.

/2018

  `data_provider`       	string              	                    
  `system_id_from_data_provider`	string              	                    
  `system_id_tracking_the_sun`	string              	                    
  `installation_date`   	string              	                    
  `system_size`         	string              	                    
  `total_installed_price`	string              	                    
  `appraised_value_flag`	string              	                    
  `sales_tax_cost`      	string              	                    
  `rebate_or_grant`     	string              	                    
  `performance_based_incentive_annual_payment`	string              	                    
  `performance_based_incentives_duration`	string              	                    
  `feed_in_tariff_annual_payment`	string              	                    
  `feed_in_tariff_duration`	string              	                    
  `customer_segment`    	string              	                    
  `new_construction`    	string              	                    
  `tracking`            	string              	                    
  `tracking_type`       	string              	                    
  `ground_mounted`      	string              	                    
  `battery_system`      	string              	                    
  `zip_code`            	string              	                    
  `city`                	string              	                    
  `county`              	string              	                    
  `utility_service_territory`	string              	                    
  `third_party_owned`   	string              	                    
  `installer_name`      	string              	                    
  `self_installed`      	string              	                    
  `azimuth_1`           	string              	                    
  `azimuth_2`           	string              	                    
  `azimuth_3`           	string              	                    
  `tilt_1`              	string              	                    
  `tilt_2`              	string              	                    
  `tilt_3`              	string              	                    
  `module_manufacturer_1`	string              	                    
  `module_model_1`      	string              	                    
  `module_manufacturer_2`	string              	                    
  `module_model_2`      	string              	                    
  `module_manufacturer_3`	string              	                    
  `module_model_3`      	string              	                    
  `additional_module_model`	string              	                    
  `module_technology_1` 	string              	                    
  `module_technology_2` 	string              	                    
  `module_technology_3` 	string              	                    
  `bipv_module_1`       	string              	                    
  `bipv_module_2`       	string              	                    
  `bipv_module_3`       	string              	                    
  `module_efficiency_1` 	string              	                    
  `module_efficiency_2` 	string              	                    
  `module_efficiency_3` 	string              	                    
  `inverter_manufacturer_1`	string              	                    
  `inverter_model_1`    	string              	                    
  `inverter_quantity_1` 	string              	                    
  `inverter_manufacturer_2`	string              	                    
  `inverter_model_2`    	string              	                    
  `inverter_quantity_2` 	string              	                    
  `inverter_manufacturer_3`	string              	                    
  `inverter_model_3`    	string              	                    
  `inverter_quantity_3` 	string              	                    
  `additional_inverter_model`	string              	                    
  `microinverter_1`     	string              	                    
  `microinverter_2`     	string              	                    
  `microinverter_3`     	string              	                    
  `dc_optimizer`        	string              	                    
  `state`               	string              	                    
	 	 
    Partition Information	 	 
    col_name            	data_type           	comment             
	 	 
    `state`               	string   

    Available states: 

    1.  AR
    2.  AZ
    3.  CA
    4.  CO
    5.  CT
    6.  DC
    7.  DE
    8.  FL
    9.  KS
    10. TX
    11. OH
    12. IL
    13. MA
    14. MD
    15. ME
    16. MN
    17. MO
    18. NH
    19. NM
    20. NY
    21. OR
    22. PA
    23. UT
    24. VT
    25. WI


/2019

  `data_provider`       	string              	                    
  `system_id_from_first_data_provider`	string              	                    
  `system_id_from_second_data_provider_if_applicable`	string              	                    
  `system_id_tracking_the_sun`	string              	                    
  `installation_date`   	date                	                    
  `system_size`         	double              	                    
  `total_installed_price`	double              	                    
  `appraised_value_flag`	boolean             	                    
  `sales_tax_cost`      	double              	                    
  `rebate_or_grant`     	double              	                    
  `performance_based_incentive_annual_payment`	double              	                    
  `performance_based_incentives_duration`	int                 	                    
  `feed_in_tariff_annual_payment`	double              	                    
  `feed_in_tariff_duration`	int                 	                    
  `customer_segment`    	string              	                    
  `new_construction`    	int                 	                    
  `tracking`            	int                 	                    
  `ground_mounted`      	int                 	                    
  `battery_system`      	int                 	                    
  `zip_code`            	string              	                    
  `city`                	string              	                    
  `utility_service_territory`	string              	                    
  `third_party_owned`   	int                 	                    
  `installer_name`      	string              	                    
  `self_installed`      	int                 	                    
  `azimuth_1`           	double              	                    
  `azimuth_2`           	double              	                    
  `azimuth_3`           	double              	                    
  `tilt_1`              	double              	                    
  `tilt_2`              	double              	                    
  `tilt_3`              	double              	                    
  `module_manufacturer_1`	string              	                    
  `module_model_1`      	string              	                    
  `module_manufacturer_2`	string              	                    
  `module_model_2`      	string              	                    
  `module_manufacturer_3`	string              	                    
  `module_model_3`      	string              	                    
  `additional_module_model`	int                 	                    
  `module_technology_1` 	string              	                    
  `module_technology_2` 	string              	                    
  `module_technology_3` 	string              	                    
  `bipv_module_1`       	int                 	                    
  `bipv_module_2`       	int                 	                    
  `bipv_module_3`       	int                 	                    
  `module_efficiency_1` 	double              	                    
  `module_efficiency_2` 	double              	                    
  `module_efficiency_3` 	double              	                    
  `inverter_manufacturer_1`	string              	                    
  `inverter_manufacturer_2`	string              	                    
  `inverter_manufacturer_3`	string              	                    
  `inverter_model_1`    	string              	                    
  `inverter_model_2`    	string              	                    
  `inverter_model_3`    	string              	                    
  `microinverter_1`     	int                 	                    
  `microinverter_2`     	int                 	                    
  `microinverter_3`     	int                 	                    
  `system_inverter_capacity`	double              	                    
  `dc_optimizer`        	int                 	                    
  `inverter_loading_ratio`	double              	                    
  `state`               	string              	                    
	 	 
    # Partition Information	 	 
    # col_name            	data_type           	comment             
            
    `state`               	string   

    Available States
    1. AR
    2. AZ
    3. CA
    4. CO
    5. CT
    6. DC
    7. DE
    8. FL
    9. IL
    10. KS
    11. MA
    12. MD
    13. ME
    14. MN
    15. MO
    16. NH
    17. NJ
    18. NM
    19. NY
    20. OH
    21. OR
    22. PA
    23. RI
    24. TX
    25. UT
    26. VT
    27. WI
    28. WA