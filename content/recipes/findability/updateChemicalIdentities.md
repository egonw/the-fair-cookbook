# Update Chemical Identities

 ````{panels_fairplus}
:difficulty_level: 2
:recipe_type: hands_on
:reading_time_minutes: 10
:intended_audience: bioinformatician, data_scientist, data_engineer, chemoinformatician
:has_executable_code: yes
:recipe_name: How to update chemical identities
```` 

## Main Objectives

The primary purpose of this recipe is:

* Determine chemical identifiers, given a compound.
* Updating chemical compounds in Wikidata.
* Create global unique identifiers for a chemical compound.


## Requirements

1) Bash Knowledge
2) Groovy Knowledge
3) WikiStatement Knowledge (Optional)
4) General knowledge of SMILES and InChI
   * SMILES (FairSharing doi:[10.25504/fairsharing.qv4b3c](https://doi.org/10.25504/fairsharing.qv4b3c))
   * InChI (FairSharing doi:[10.25504/fairsharing.ddk9t9](https://doi.org/10.25504/fairsharing.ddk9t9))

---


## Main Content



### Generating Compound Identifiers
Updating chemical identifiers requires the existing knowledge of at least one chemical identifier. 

From this identifier, other compound identifiers can be determined using the following script:

```
groovy -identifier <Identifier> -identifierType <IdentifierType>
```
This script is available from the chemistry development kit/


### Updating Wikidata
With the available identifiers of a compound the compound can be updated within Wikidata using any of the following:

- Direct edits to the compound page from the compound's Wikidata page using the 'edit' functionality 
- Quick statements at https://quickstatements.toolforge.org/



## Conclusion
This recipe discussed several aspects of updating chemical compounds
- How to retrieve chemical identifiers given an original identifier
- How to update chemical identification properties in Wikidata 


## Authors
````{authors_fairplus}
Zachary: Writing - Original Draft
````


## License

````{license_fairplus}
CC-BY-4.0
````