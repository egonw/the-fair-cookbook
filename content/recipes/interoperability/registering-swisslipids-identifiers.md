# Registering SwissLipids identifiers in Wikidata

 ````{panels_fairplus}
:identifier_text: FCBxxx
:identifier_link: 'https://w3id.org/faircookbook/FCBxxx'
:difficulty_level: 5
:recipe_type: hands_on
:reading_time_minutes: 15
:intended_audience: bioinformatician, data_scientist, data_engineer
:has_executable_code: nope
:recipe_name: Registering SwissLipids identifiers in Wikidata
```` 

## Main Objectives

The objective of this recipe is to describe how [Wikidata](https://www.wikidata.org) can be augmented by adding `external identifiers`, that is identifiers minted by other authorities for an entity described in Wikidata,  in an automated way. This is to make the content more findable and more interoperable.

___


## Requirements

- Familiarity with the process of requesting a new property from [Wikidata](https://www.wikidata.org/).
That process is described [here](https://www.wikidata.org/wiki/Wikidata:Property_proposal).
- In order to record identifiers of some resource, first a  property needs to be requested.

---


## Main Content

Making datasets more findable and databases more interoperable, it is recommended to share not just top-level metadata, but also record-level
metadata. For example, for chemical databases, you can share your chemical compound identifiers as open data, while keeping the data itself
in your own database. That allows other databases, like Wikidata, to link to your database. First, that makes your data more findable,
but it also makes your data easier to integrate and therefore more interoperable.

This recipe follows this idea and starts with a mapping file that links InChIKeys and database identifiers and which is shared under a [CC0 license](https://creativecommons.org/share-your-work/public-domain/cc0/).
The FAIR cookbook Recipe *[InChI and SMILES identifiers for chemical structures (fcb:FCB007)](https://w3id.org/faircookbook/FCB007)*  explains how InChIKeys
can be generated for chemical compounds in your database.

To illustrate our point, this recipe demonstrates the approach of listing [SwissLipid](http://www.swisslipids.org/#/) {footcite}`Aimo2015SwissLipids`
identifiers in Wikidata, developed at the 2021 BioHackathon Europe.
The work relied on the fact that a [`Swiss Lipids property` for Wikidata](https://www.wikidata.org/wiki/Wikidata:Property_proposal/SwissLipids_identifier)
was already proposed, later approved, and created in Wikidata [just before the 2020 BioHackathon Europe](https://www.wikidata.org/w/index.php?title=Property:P8691&oldid=1287579005).

```{warning}
An existing Wikidata property is a requirement to adding the external identifiers.
```

The recipe uses [Bacting](https://bio.tools/bacting) {footcite}`Willighagen2021` which can be used in Groovy and Python. We
will use the first here.

The starting point is mappings (tuples) that link the `SwissLipids` identifiers to `InChIKeys`.
The latter
is used to find the matching Wikidata items.

### Step 1: getting the data

While the SwissLipids data is available under [CC-BY license](https://creativecommons.org/licenses/by/4.0/), it was agreed that adding the SwissLipids identifiers to Wikidata resource, the data of which is available under [CC0 license](https://creativecommons.org/publicdomain/zero/1.0/) is legitimate.

* Download `lipids.tsv` from the [Downloads](https://www.swisslipids.org/#/downloads) page
* Gunzip the file

### Step 2: extract Swiss Lipid ID <> InChIKey tuples

For this step, I use `csvtool` (`apt get install csvtool`):

```shell
csvtool -t TAB col 1,11 swisslipids.tsv
```

The output needs some further clean up, like removing lines without InChIKeys or "none" and "-" as value. Also,
the "InChIKey=" prefix is removed in preparation for the next step. The full used code is:

```shell
csvtool -t TAB col 1,11 swisslipids.tsv  | sed 's/InChIKey=//' | grep -v "none" | grep -v ",-$" | grep -v ",$" | tee swisslipids_ids.tsv
```

This results in almost 600k tuples:

```shell
$ wc -l swiss*tsv
   592412 swisslipids_ids.tsv
   777957 swisslipids.tsv
```

### Step 3: creating a ShEx model

Skipping this step for later this week, but here the task is to create a shape expression for Wikidata, to model how
the identifiers will be added to Wikidata. See the recept publication by Waagmeester et al. about _A protocol for adding knowledge to Wikidata: aligning resources on human coronaviruses_
{footcite}`Waagmeester2021`.

### Step 4: creating QuickStatements

Now we have the mappings and the data model in Wikidata, we can create [QuickStatements](https://www.wikidata.org/wiki/Help:QuickStatements) to allow us to enter the
data into Wikidata. This is not the only approach, and the process can be further automated using "Wikidata bots".
For this, see BioHackathon Europe [Project 32](https://github.com/elixir-europe/biohackathon-projects-2021/tree/main/projects/32):
Connecting ELIXIR-related open data on Wikidata via [WikiProject ELIXIR](https://www.wikidata.org/wiki/Wikidata:WikiProject_Elixir).

Based on existing Bacting scripts, a script is created to take the `swisslipids_ids.tsv` file as input and create
QuickStatements: [https://github.com/egonw/ons-wikidata/blob/master/ExtIdentifiers/swisslipids.groovy](https://github.com/egonw/ons-wikidata/blob/master/ExtIdentifiers/swisslipids.groovy) This script is using [Apache Groovy](http://www.groovy-lang.org/)
but Bacting can also be using in Python, see [https://github.com/cthoyt/pybacting](https://github.com/cthoyt/pybacting).

This script uses a federated query against https://beta.sparql.swisslipids.org/sparql/ after a suggestion by Dr [Jerven Bolleman](https://orcid.org/0000-0002-7449-1266)
who indicated that the [RDF4J](https://rdf4j.org/) backing this SPARQL endpoint will automatically batch the query against Wikidata, overcoming
limitations of the Wikidata Query Service:

```sparql
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
SELECT ?wd ?key ?value WHERE {
  SERVICE <https://query.wikidata.org/sparql> {
    SELECT (substr(str(?compound),32) as ?wd) ?key ?value WHERE {
      ?compound wdt:P235 ?key .
      OPTIONAL { ?compound wdt:P8691 ?value . }
    }
  }
}
```

This creates a file that looks like:

```shell
Q76738581       P8691   "SLM:000163948" S248    Q41165322       S854    "https://www.swisslipids.org/#/downloads"       S813    +2021-11-06T00:00:00Z/11
Q76865359       P8691   "SLM:000163954" S248    Q41165322       S854    "https://www.swisslipids.org/#/downloads"       S813    +2021-11-06T00:00:00Z/11
Q76865370       P8691   "SLM:000163964" S248    Q41165322       S854    "https://www.swisslipids.org/#/downloads"       S813    +2021-11-06T00:00:00Z/11
Q76866423       P8691   "SLM:000163966" S248    Q41165322       S854    "https://www.swisslipids.org/#/downloads"       S813    +2021-11-06T00:00:00Z/11
Q76865004       P8691   "SLM:000163968" S248    Q41165322       S854    "https://www.swisslipids.org/#/downloads"       S813    +2021-11-06T00:00:00Z/11
Q76733356       P8691   "SLM:000164019" S248    Q41165322       S854    "https://www.swisslipids.org/#/downloads"       S813    +2021-11-06T00:00:00Z/11
Q76733312       P8691   "SLM:000164023" S248    Q41165322       S854    "https://www.swisslipids.org/#/downloads"       S813    +2021-11-06T00:00:00Z/11
Q76737210       P8691   "SLM:000164026" S248    Q41165322       S854    "https://www.swisslipids.org/#/downloads"       S813    +2021-11-06T00:00:00Z/11
Q76736881       P8691   "SLM:000164032" S248    Q41165322       S854    "https://www.swisslipids.org/#/downloads"       S813    +2021-11-06T00:00:00Z/11
Q76735022       P8691   "SLM:000164034" S248    Q41165322       S854    "https://www.swisslipids.org/#/downloads"       S813    +2021-11-06T00:00:00Z/11
```

This resulted in about 17.5 thousand mappings. These are based on exact InChIKey match. 

### Step 5: running the QuickStatements

The resulting mappings are being added via the [QuickStatements website](https://quickstatements.toolforge.org/).

## Conclusion

The main conclusion is that it is possible to automate adding external / third party identifiers routinely to Wikidata using InChIKey matching.
The tool `Bacting` used in the example can obviously be replaced without too much effort by any other tools performing the same steps.

What this protocol does not do, however, is create Wikidata items for lipids in the SwissLipids database that
are not yet in Wikidata. This is the topic of a future recipe.

## References

```{footbibliography}
```

---

## Authors

````{authors_fairplus}
Egon: Writing - Original Draft
Philippe: Review & Editing
````


---

## License

````{license_fairplus}
CC-BY-4.0
````
