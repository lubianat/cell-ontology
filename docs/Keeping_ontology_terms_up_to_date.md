## Keeping Cell Ontology annotation up to date

Cell Ontology identifiers (IRIs) are never lost, but they might be deprecated.
When this happens, all logical links to other ontology terms (e.g. recording classification or partonomy) are removed and term is tagged with the annotation `owl:deprecated True`. 
To aid migration of annotations to the latest standard,  these terms are also annotated with either a `term_replaced_by` or a `consider` tag.
A `term_replaced_by` annotation is used to record the ID of a term it is safe to auto-migrate annotations to.  More rarely,  `consider` is used to record multiple potential replacement terms requiring human consideration to map.  
In these cases, a comment will be present to provide guidance for mapping.

Due to legacy issues, the values of these tags are either a CURIE (`"CL:0000123"`) or short_form ID (`"CL_0000123"`) rather than a complete IRI.  Handling code needs to deal with both of these formats

### Checking for deprecated terms

The [Ontology Lookup Service API](https://www.ebi.ac.uk/ols/docs/api#Term) provides a convenient way to check for deprecated terms & find replacements.


For example, the term <http://purl.obolibrary.org/obo/CL_0000375> ("obsolete osteoprogenitor cell") has been deprecated and has that tag **term_replaced_by**

It is possible to query the Ontology Lookup Service API for this by concatenating the base API URL for cl (https://www.ebi.ac.uk/ols/api/ontologies/cl/terms/) with the URL for the class in CL:

<https://www.ebi.ac.uk/ols/api/ontologies/cl/terms/http%253A%252F%252Fpurl.obolibrary.org%252Fobo%252FCL_0000375>. 

(Note the query IRI must be double encoded)
<!-- What is double encoding? -->

That API query will return a JSON object containing the detailed information for the class. 
The key `"term_replaced_by"` points to the ID of a safe replacement term: `"CL:0007010"`.  This is a Compact URI (CURIE) for <http://purl.obolibrary.org/obo/CL_0007010>  ("preosteoblast").

<details>
  <summary> Ontology Lookup Service API data on CL_0000375 (click to expand) </summary>

  ```json
  {
    "iri" : "http://purl.obolibrary.org/obo/CL_0000375",
    "label" : "obsolete osteoprogenitor cell",
    "description" : null,
    "annotation" : {
      "database_cross_reference" : [ "BTO:0002051" ],
      "has_obo_namespace" : [ "cell" ],
      "term replaced by" : [ "CL:0007010" ]
    },
    "synonyms" : null,
    "ontology_name" : "cl",
    "ontology_prefix" : "CL",
    "ontology_iri" : "http://purl.obolibrary.org/obo/cl.owl",
    "is_obsolete" : true,
    "term_replaced_by" : "CL:0007010",
    "is_defining_ontology" : true,
    "has_children" : false,
    "is_root" : true,
    "short_form" : "CL_0000375",
    "obo_id" : "CL:0000375",
    "in_subset" : null,
    "obo_definition_citation" : null,
    "obo_xref" : [{"database":"BTO","id":"0002051","description":null,"url":"http://purl.obolibrary.org/obo/BTO_0002051"}],
    "obo_synonym" : null,
    "is_preferred_root" : false,
    "_links" : {
      "self" : {
        "href" : "https://www.ebi.ac.uk/ols/api/ontologies/cl/terms/http%253A%252F%252Fpurl.obolibrary.org%252Fobo%252FCL_0000375"
      },
      "graph" : {
        "href" : "https://www.ebi.ac.uk/ols/api/ontologies/cl/terms/http%253A%252F%252Fpurl.obolibrary.org%252Fobo%252FCL_0000375/graph"
      }
    }
  }
  ```
  
</details>

### The `consider` tag

The term <http://purl.obolibrary.org/obo/CL_0000144> ("obsolete cell by function") has been deprecated and has a **consider** tag pointing to multiple possible replacement terms, along with a comment for guidance.

Querying the [OLS API for this term](https://www.ebi.ac.uk/ols/api/ontologies/cl/terms/http%253A%252F%252Fpurl.obolibrary.org%252Fobo%252FCL_0000144), you will find under the `"annotation"` key the subkeys  `"comment"` and `"consider"`. The  first will contain a coment: `[ "This term was made obsolete because there is no difference in meaning between it and 'cell', as any cell can be classified by its function or behavior. If you have used this term in annotation, please replace it with cell (CL:0000000), native cell (CL:0000003), or cell in vitro (CL:0001034) as appropriate."]`
The second contain key holds a list of a list of  three CURIES : `[ "CL:0001034", "CL:0000000", "CL:0000003" ]`, pointing to "cell in vitro", "cell", and "native cell", respectively


<details>
  <summary> Ontology Lookup Service API data on CL_0000144 (click to expand) </summary>

```json
{
  "iri" : "http://purl.obolibrary.org/obo/CL_0000144",
  "label" : "obsolete cell by function",
  "description" : [ "OBSOLETE: A classification of cells by their primary end goal or behavior." ],
  "annotation" : {
    "comment" : [ "This term was made obsolete because there is no difference in meaning between it and 'cell', as any cell can be classified by its function or behavior. If you have used this term in annotation, please replace it with cell (CL:0000000), native cell (CL:0000003), or cell in vitro (CL:0001034) as appropriate." ],
    "consider" : [ "CL:0001034", "CL:0000000", "CL:0000003" ],
    "has_obo_namespace" : [ "cell" ]
  },
  "synonyms" : null,
  "ontology_name" : "cl",
  "ontology_prefix" : "CL",
  "ontology_iri" : "http://purl.obolibrary.org/obo/cl.owl",
  "is_obsolete" : true,
  "term_replaced_by" : null,
  "is_defining_ontology" : true,
  "has_children" : false,
  "is_root" : true,
  "short_form" : "CL_0000144",
  "obo_id" : "CL:0000144",
  "in_subset" : null,
  "obo_definition_citation" : [{"definition":"OBSOLETE: A classification of cells by their primary end goal or behavior.","oboXrefs":[{"database":"FB","id":"ma","description":null,"url":"http://flybase.org/reports/ma.html"}]}],
  "obo_xref" : null,
  "obo_synonym" : null,
  "is_preferred_root" : false,
  "_links" : {
    "self" : {
      "href" : "https://www.ebi.ac.uk/ols/api/ontologies/cl/terms/http%253A%252F%252Fpurl.obolibrary.org%252Fobo%252FCL_0000144"
    },
    "graph" : {
      "href" : "https://www.ebi.ac.uk/ols/api/ontologies/cl/terms/http%253A%252F%252Fpurl.obolibrary.org%252Fobo%252FCL_0000144/graph"
    }
  }
}
```
<\details>


