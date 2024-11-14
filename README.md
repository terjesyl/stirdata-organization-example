# Organization/Unit from Enhetsregisteret as Linked Data

> [!IMPORTANT]  
> For testing purposes only

Fetch a given organization/unit from Enhetsregisteret (i.e. Digdir):

```bash
curl -H "accept: application/json" -v https://data.brreg.no/enhetsregisteret/api/enheter/991825827
```

## Introduce "@context"-object

Defines which properties should be mapped to the JSON keys in the JSON response.
Follows the STIRData specification.

## Description of properties/keys

For conformance with the STIRData specification,

The following properties have been added:

|  key                     | RDF property             |
| ------------------------ | ------------------------ |
| legalIdentifier          | legal:legalIdentifier    |
| stirdata:abbreviatedName | stirdata:abbreviatedName |
| parentOrganization       | org:hasUnit (reversed)   |
| telefonInternasjonal     |  schema:telephone        |
|                          |  org:hasSite             |

The following keys have been given a semantic context without changes:

|  key  | RDF property |
| ----- | ------------ |
| epost | schema:email |

The following objects have extra properties (in addition to semantic context) added without further changes:

|  key/property | RDF property          |
| ------------- | --------------------- |
| naeringskode1 | legal:companyActivity |

The following objects have been modified:

|  key/property      | RDF property           |
| ------------------ | ---------------------- |
| postadresse        | stirdata:postalAddress |
| forretningsadresse | m8g:registeredAddress  |

## Relevant properties currently omitted

Oppløsningsdata/dissolution date, `schema:dissolutionDate`

## Turtle representation

The JSON-LD representation results in the Turtle serialization below. There is some information loss since not all JSON keys are given a semantic definition.

```turtle
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .
@prefix skos: <http://www.w3.org/2004/02/skos/core#> .
@prefix dcterms: <http://purl.org/dc/terms/> .
@prefix schema: <https://schema.org/> .
@prefix locn: <http://www.w3.org/ns/locn#> .
@prefix org: <http://www.w3.org/ns/org#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix legal: <http://www.w3.org/ns/legal#> .
@prefix adms: <http://www.w3.org/ns/adms#> .
@prefix stirdata: <https://w3id.org/stirdata/vocabulary/> .
@prefix m8g: <http://data.europa.eu/m8g/> .

<https://data.brreg.no/enhetsregisteret/api/enheter/932384469> org:hasUnit <https://data.brreg.no/enhetsregisteret/api/enheter/991825827> .

<https://data.brreg.no/enhetsregisteret/api/enheter/991825827> m8g:registeredAddress [
  locn:addressArea "OSLO" ;
  locn:adminUnitL1 "Norge" ;
  locn:adminUnitL2 "OSLO" ;
  locn:locatorDesignator "1C" ;
  locn:postCode "0585" ;
  locn:thoroughfare "Lørenfaret" ;
 ] ;
 dcterms:identifier "991825827" ;
 a legal:LegalEntity ;
 legal:companyActivity [
  skos:broader "http://data.europa.eu/ux2/nace2/8411" ;
  skos:notation "84110" ;
  skos:prefLabel "Generell offentlig administrasjon"@no ;
  stirdata:level "5" ;
 ] ;
 legal:legalIdentifier [
  dcterms:creator <https://data.brreg.no/enhetsregisteret/api/enheter/974760673> ;
  dcterms:issued "2007-10-15" ;
  rdf:type adms:Identifier ;
  skos:notation "991825827" ;
 ] ;
 legal:legalName "DIGITALISERINGSDIREKTORATET"@no ;
 org:hasSite [
  rdf:type org:Site ;
  org:siteAddress "https://www.digdir.no" ;
 ] ;
 schema:email "postmottak@digdir.no" ;
 schema:telephone "+4722451000" ;
 stirdata:abbreviatedName "Digdir" ;
 stirdata:postalAddress [
  locn:addressArea "OSLO" ;
  locn:adminUnitL2 "OSLO" ;
  locn:country "Norge" ;
  locn:locatorDesignator "Postboks 1382" ;
  locn:postCode "0114" ;
 ] .
```
