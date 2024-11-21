# Grunner til å bruke framing

Datakonsumering og integrasjon blir uavhengig av JSON-strukturen.
Man trenger kun å forholde seg til spesifikasjonen som RDF-dataen (JSON-LD) følger.

Gjør at integrasjonen takler endringer i JSON-API-et, og endringer som for et vanlig JSON-API ville vært en breaking change.

![Diagram over ulike flyter](../media/diagram-framing.drawio.svg)

## Eksempler på endring

- Legger til engelsk navn på Digdir.
- Endre nøkkel fra `navn` til `name`
- Endrer nøkkel fra `epostadresse` til `email`

## Opprinnelig JSON-LD

```json
{
  "@context": {
    "legal": "http://www.w3.org/ns/legal#",
    "schema": "https://schema.org/",
    "LegalEntity": "legal:LegalEntity",
    "type": "@type",
    "id": "@id",
    "navn": {
      "@id": "legal:legalName",
      "@language": "no"
    },
    "epostadresse": "schema:email"
  },
  "type": "LegalEntity",
  "id": "https://data.brreg.no/enhetsregisteret/api/enheter/991825827",
  "navn": "DIGITALISERINGSDIREKTORATET",
  "epostadresse": "postmottak@digdir.no"
}
```

**JSON-nøkler**:

```json
  "type": "LegalEntity",
  "id": "https://data.brreg.no/enhetsregisteret/api/enheter/991825827",
  "navn": "DIGITALISERINGSDIREKTORATET",
  "epostadresse": "postmottak@digdir.no"
```

## Endret JSON men med kontekst som overholder STIRData

```json
{
  "@context": {
    "legal": "http://www.w3.org/ns/legal#",
    "schema": "https://schema.org/",
    "LegalEntity": "legal:LegalEntity",
    "type": "@type",
    "id": "@id",
    "name": {
      "@id": "legal:legalName",
      "@container": "@language"
    },
    "email": "schema:email"
  },
  "type": "LegalEntity",
  "id": "https://data.brreg.no/enhetsregisteret/api/enheter/991825827",
  "name": {
    "no": "DIGITALISERINGSDIREKTORATET",
    "en": "The Norwegian Digitalisation Agency"
  },
  "email": "postmottak@digdir.no"
}
```

**JSON-nøkler**:

```json
  "type": "LegalEntity",
  "id": "https://data.brreg.no/enhetsregisteret/api/enheter/991825827",
  "name": {                                <-------------- endret fra string til objekt
    "no": "DIGITALISERINGSDIREKTORATET",
    "en": "The Norwegian Digitalisation Agency"
  },
  "email": "postmottak@digdir.no"          <-------------- endret nøkkelnavn
```

## Framing

Framing gir konsument samme JSON-struktur uavhengig av endringene, siden den kun forholder seg til STIRData-modellen, og ikke JSON-utformingen.

**Frame**:

```json
{
  "@context": {
    "legal": "http://www.w3.org/ns/legal#",
    "schema": "https://schema.org/",
    "name": {
      "@id": "legal:legalName",
      "@container": "@language"
    },
    "email": "schema:email"
  },
  "@type": "legal:LegalEntity",
  "email": {},
  "legal:legalName": {},
  "@explicit": true
}
```

RDF-resultat er lik etter framing av begge JSON-dokumentene:

```json
{
  "@context": {
    "legal": "http://www.w3.org/ns/legal#",
    "schema": "https://schema.org/",
    "name": {
      "@id": "legal:legalName",
      "@container": "@language"
    },
    "email": "schema:email"
  },
  "@id": "https://data.brreg.no/enhetsregisteret/api/enheter/991825827",
  "@type": "legal:LegalEntity",
  "name": {
    "no": "DIGITALISERINGSDIREKTORATET",
    "en": "The Norwegian Digitalisation Agency",    <----- eneste tillegg, struktur endrer seg ikke
  },
  "email": "postmottak@digdir.no"
}
```

I Turtle-syntaks:

```turtle
@prefix legal: <http://www.w3.org/ns/legal#> .
@prefix schema: <https://schema.org/> .

<https://data.brreg.no/enhetsregisteret/api/enheter/991825827>
  a <http://www.w3.org/ns/legal#LegalEntity> ;
  legal:legalName "DIGITALISERINGSDIREKTORATET"@no, "The Norwegian Digitalisation Agency"@en ;
  schema:email "postmottak@digdir.no" .
```
