# Finding FAIR Experimental Datasets for Chemical Compounds
 "Using Wikidata to find experimental data related chemical compounds in a FAIR dataset."


 ````{panels_fairplus}
:difficulty_level: 2
:recipe_type: hands_on
:reading_time_minutes: 15
:intended_audience: bioinformatician, data_curator, data_scientist, data_engineer
:has_executable_code: nope
:recipe_name: How to find FAIR Datasets
```` 

## Main Objectives

The goal of this recipe is to find compounds from scientific sources to add to Wikidata. Many different trusted sources and repositories contain research surrounding chemical compounds, and, given the proper licenses, these compounds can be added to Wikidata.
___

## Dataset Extensions and Identifiers

Here are some datasets types which are simple to add to Wikidata. Each has a well-structured format. 

* SDF file (FairSharing doi:[10.25504/fairsharing.ew26v7](https://doi.org/10.25504/fairsharing.ew26v7))

* csv file (FairSharing [bsg-s001546/](https://fairsharing.org/bsg-s001546/))

## Identifiers
To add other related chemical identifiers to each compound at least universal identifier must be present. For example: 

* SMILES (FairSharing doi:[10.25504/fairsharing.qv4b3c](https://doi.org/10.25504/fairsharing.qv4b3c))
* InChI (FairSharing doi:[10.25504/fairsharing.ddk9t9](https://doi.org/10.25504/fairsharing.ddk9t9))

For a more detailed understanding of the chemical structures available refer to (FAIR Chemical Structures`fairchemicalstructures`)



## Requirements:
* Public license knowledge
* Data format knowledge
---


## Main Content
Adding compounds to Wikidata is a multi-step procedure. This process begins with identifying data with the correct licensing. There are many different resources available that have open-source research. Moreover, much of this research has datasets accompanying it: these datasets, or other various supplementary material, may be unstructured, semi-structured, or structured. Ideally, datasets in a standardized format and has some meta-information to help with identification. 

### Process
1) Searching for chemistry papers on Figshare
2) Filter papers via the CCO license search criteria
3) Search for datasets or other supplementary material
4) View the dataset contents to decern the quality


## Conclusion

> Summerize Key Learnings of the recipe.
> * Searching for datasets based on license
> * Identifying data quality
> Suggest further reading using the following:
> ### What should I read next?
> * [Registering Datasets](./.md)
> * [Unique Keys](./.md)

---


## Authors

````{authors_fairplus}
Zachary: Writing - Conceptualization, Original Draft
````

---

## License

````{license_fairplus}
CC-BY-4.0
````