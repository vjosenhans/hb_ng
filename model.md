# Introduction

We will create the following application profiles:

* simple: `ap-simple`
* FRBRoo/RDA/GND: `ap-internal`
* schema.org: `ap-schema`
* CERIF: `ap-cerif`
* Kerndatensatz Forschung: `ap-kdsf`

The following ontologies are used:

	# general
	@prefix xsd:      <http://www.w3.org/2001/XMLSchema#> .
	@prefix rdf:      <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
	@prefix rdfs:     <http://www.w3.org/2000/01/rdf-schema#> .
	@prefix owl: 	  <http://www.w3.org/2002/07/owl#> . 
	@prefix foaf:     <http://xmlns.com/foaf/0.1/#> .
	@prefix dsp:      <http://dublincore.org/dc-dsp#> .
	@prefix dcterms:  <http://purl.org/dc/terms#> .
	@prefix skos:     <http://www.w3.org/2004/02/skos/core#> .
	@prefix schema:   <http://schema.org/> .
	@prefix powder:   <http://www.w3.org/2007/05/powder-s#> .
	@prefix prov:     <http://www.w3.org/ns/prov#> .
	# cidoc crm universe
	@prefix ecrm:     <http://erlangen-crm.org/120111/> .
	@prefix efrbroo:  <http://erlangen-crm.org/efrbroo/121016/> .
	# resource discovery and access (rda)
	@prefix rdac:     <http://rdaregistry.info/Elements/c/> .
	@prefix rdaw:     <http://rdaregistry.info/Elements/w/> .
	@prefix rdae:     <http://rdaregistry.info/Elements/e/> .
	@prefix rdam:     <http://rdaregistry.info/Elements/m/> .
	@prefix rdai:     <http://rdaregistry.info/Elements/i/> .
	# cris
	@prefix cerif:    <http://www.eurocris.org/ontologies/semcerif/1.3#> .
	# authority data
	@prefix gnd:      <http://d-nb.info/standards/elementset/gnd#> .

For each profile we describe a mapping from current resources.

`dcterms:type` links to a controlled vocabulary (e.g. a skos:ConceptScheme); registerNamespace('xlink', 'http://www.w3.org/1999/xlink')

# Properties and Classes for Authority Data

| Source | Target Entities | Target Property | Target Value Type | Application Profile | Search | Display | Facet | Index |
| ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ |
| //name[@type="personal" and not(@authority)] | foaf:Person, cerif:cfPers, schema:Person, efrbroo:F10_Person, rdac:Person, prov:Person | rdf:about | rdfs:Literal [1] | * | 1 | 1 | | person |
| //name[@type="personal" and not(@authority)]/namePart[@type="family"] | foaf:Person, cerif:cfPers, schema:Person, efrbroo:F10_Person, rdac:Person, prov:Person | foaf:familyName | rdfs:Literal | ap-simple | [2] | | | |
| //name[@type="personal" and not(@authority)]/namePart[@type="given"] | foaf:Person, cerif:cfPers, schema:Person, efrbroo:F10_Person, rdac:Person, prov:Person | foaf:givenName | rdfs:Literal | ap-simple | | | | |
| //name[@type="personal" and starts-with(@valueURI, "http://d-nb.info/gnd/") and @authority="gnd"]/@valueURI | foaf:Person, cerif:cfPers, schema:Person, efrbroo:F10_Person, rdac:Person, prov:Person | powder:describedby | gnd:Person | * | 1 | 1 | ua_member [3] | person |
| //name[@type="personal" and starts-with(@valueURI, "http://d-nb.info/gnd/") and @authority="gnd"]/@valueURI | foaf:Person, cerif:cfPers, schema:Person, efrbroo:F10_Person, rdac:Person, prov:Person | cerif:cfPersID | rdfs:Literal | ap-cerif | 1 | 1 | ua_member | person |
| //name[@type="personal"]/role/roleTerm[@type="code" and @authorityURI="http://www.loc.gov/marc/relators/"] | must be defined at the publication level | | | | 1 | 0 | role | role |
| //name[@type="corporate" and not(@authority)] | foaf:Organization, cerif:cfOrgUnit, schema:Organization, efrbroo:F11_Corporate_Body, prov:Organization | rdf:about | rdfs:Literal [4] | * | 1 | 1 | institution | institution |
| //name[@type="corporate" and not(@authority)]/namePart | foaf:Organization, cerif:cfOrgUnit, schema:Organization, efrbroo:F11_Corporate_Body, prov:Organization | foaf:name | rdfs:Literal | ap-simple | 1 | 1 | institution | institution |
| //name[@type="corporate" and starts-with(@valueURI, "http://d-nb.info/gnd/") and @authority="gnd"]/@valueURI | foaf:Organization, cerif:cfOrgUnit, schema:Organization, efrbroo:F11_Corporate_Body, prov:Organization | powder:describedby | gnd:Organisation | * | 1 | 1 | f_ua_unit [5] | ua_unit |
| //name[@type="corporate" and starts-with(@valueURI, "http://d-nb.info/gnd/") and @authority="gnd"]/@valueURI |foaf:Organization, cerif:cfOrgUnit, schema:Organization, efrbroo:F11_Corporate_Body, prov:Organization | cerif:cfOrgUnitID | rdfs:Literal | ap-cerif | 1 | 1 | f_ua_unit | ua_unit |
| //name[@type="corporate"]/role/roleTerm[@type="code" and @authorityURI="http://www.loc.gov/marc/relators/"] | must be defined at the publication level | | | | 1 | 0 | role | role |

[1]: if exists(//name[@type="personal" and starts-with(@valueURI, "http://d-nb.info/gnd/") and @authority="gnd"]/@valueURI) then create an URI based on an UUID for the entity in first migration.
[2]: For indexing family and given names have to be combined and stored in both the normal and the inverted form.
[3]: As we don't have affiliation data in the MODS we need to make sure that persons with GND identifiers in fact are university members.
[4]: if exists(//name[@type="corporate" and starts-with(@valueURI, "http://d-nb.info/gnd/") and @authority="gnd"]/@valueURI) then create an URI based on an UUID for the entity in first migration.
[5]: We can be pretty certain that organizations with GND identifiers in the MODS data are university units.

# Properties and Classes for Bibliographic Description


## List of source paths to map from the current system 

| Source | Target Entity | Target Property | Target Value Type | Application Profile | Search | Display | Facet | Index |
| ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ |
| //mods/typeOfResource | | | | | 1 | 1 | type | type |
| //mods/identifier[@type="isbn"] | | | | | 1 | 1 | | isbn |
| //mods/identifier[@type="issn"] | | | | | 1 | 1 | | issn |
| //mods/relatedItem/identifier[@type="isbn"] | | | | | 1 | 1 | | isbn |
| //mods/relatedItem/identifier[@type="issn"] | | | | | 1 | 1 | | issn |
| //mods/identifier[@type="doi"] | | | | | 1 | 1 | | doi |
| //mods/identifier[@type="pm"] | | | | | 1 | 1 | | pmid |
| //mods/identifier[@type="isi"] | | | | | 1 | 1 | | wos_id |
| //mods/tableOfContents[@xlink:href] | | | | | 0 | 1 | | |
| //mods/accessCondition[@type="use and reproduction"] | | | | | 0 | 1 | | |
| //mods/accessCondition[@type="restriction on access"] | | | | | 0 | 1 | | |
| //mods/note | | | | | 1 | 1 | | note |
| //mods/note[@type="publication status"] | | | | | 0 | 1 | | |
| //mods/note[@displayLabel="Preis"] | | | | | 0 | 0 | | |
| //mods/note[@displayLabel="Titelzusätze"] | | | | | 1 | 1 | | remainder |
| //mods/subject[@authority="mesh"]/topic | | | | | 1 | 1 | fsubject | subject |
| //mods/subject[@authority="thesoz"]/topic | | | | | 1 | 1 | fsubject | subject |
| //mods/subject[@authority="stw"]/topic | | | | | 1 | 1 | fsubject | subject |
| //mods/subject[@authority="lcsh"]/topic | | | | | 1 | 1 | fsubject | subject |
| //mods/language/languageTerm | | | | | 1 | 1 | lang | lang |
| //mods/location/physicalLocation | | | | | | | | |
| //mods/location/shelfLocator | | | | | 1 | 1 | | locator |
| //mods/location/url | | | | | 0 | 1 | | |
| //mods/physicalDescription/extent | | | | | 0 | 1 | | |
| //mods/physicalDescription/internetMediaType | | | | | 0 | 1 | | |
| //mods/abstract[@sharable="no"] | | | | | 1 | 0 | | abstract |
| //mods/abstract | | | | | 1 | 1 | | abstract |
| //mods/recordInfo/recordCreationDate | | | | | 0 | 0 | | |
| //mods/recordInfo/recordChangeDate | | | | | 0 | 0 | | |
| //mods/extension/dcterms:bibliographicCitation | | | | | 0 | 1 | | |
| //mods/genre[@authority="dct" and @valueURI="http://purl.org/dc/dcmitype/Text"] | | | | | | | | |
| //mods/genre[starts-with(@valueURI, "http://purl.org/info:eu-repo/semantics/")] | | | | | | | | |
| //mods/genre[@authority="dct" starts-with(@valueURI="http://purl.org/dc/dcmitype/")] |  | dcterms:type | skos:Concept | | | | | |
| //mods/genre[not(@authority) and starts-with(@valueURI="http://purl.org/info:eu-repo/semantics/")] | | dcterms:type | skos:Concept | | | | | |
| //mods/genre[@authority="local"] | | dcterms:type | skos:Concept | | | | | |
| //mods/genre[@authority="marcgt"] | | dcterms:type | skos:Concept | | | | | |
| //mods/relatedItem[@type="..."] | | ... | rdfs:Resource (depends on @type and/or //mods/relatedItem/genre) | | | | | |



## Publications 

[The CERIF XML Publication Entity in the OpenAIRE context](https://guidelines.openaire.eu/wiki/OpenAIRE_Guidelines:_For_CRIS#The_CERIF_XML_Publication_Entity_in_the_OpenAIRE_context):
Book, Book Review, Book Chapter Abstract, Book Chapter Review, Inbook, Anthology, Monograph, Referencebook, Textbook, Encyclopedia, Manual, Otherbook, Journal, Journal Issue, Journal Article, Journal Article Abstract, Journal Article Review, Conference Proceedings, Conference Proceedings Article, Conference Abstract, Conference Poster, Letter, Letter to Editor, PhD Thesis, Doctoral Thesis, Supervised Student Publications, Report, Short Communication, Poster, Presentation, Newsclipping, Commentary, Annotation, Transliteration, Translation, Authored Book, Edited Book, Chapter in Book, Scholarly Edition, Conference Contribution, Working Paper, Research Report for external body, Confidential Report (for external body), Encyclopedia Entry, Magazine Article, Dictionary Entry, Online Resource, Standard and Policy 


**independent publication**

depends on `//mods/genre`: {TODO list of relevant values}

* use static FRBRoo model: 
	* `efrbroo:F1_Work`, `rdac:Work`  
	* `efrbroo:R3_is_realised_in (realises)`, `rdaw:expressionOfWork (rdae:workExpressed)`
	* `efrbroo:F22_Self-Contained_Expression`, `rdac:Expression`
	* `efrbroo:R4_carriers_provided_by (comprises_carriers_of)`, `rdae:manifestationOfExpression (rdae:manifestationOfExpression)`
	* `efrbroo:F3_Manifestation_Product_Type`, `rdac:Manifestation`
	* `efrbroo:R7_has_example (is_example_of)`, `rdam:exemplarOfManifestation (rdai:manifestationExemplified)`
	* `efrbroo:F5_Item`, `rdac:Item`
* roles for contributors in RDA: ["has contributor" and its subproperties](http://www.rdaregistry.info/Elements/e/#P20053)

| Source | Target Entities | Target Property | Target Value Type | Application Profile | Search | Display | Facet | Index |
| ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ |
| //mods | efrbroo:F1_Work, rdac:Work | | | ap-internal |
| //mods | efrbroo:F1_Work, rdac:Work | efrbroo:R3_is_realised_in | cerif:cfResPubl, efrbroo:F22_Self-Contained_Expression, rdac:Expression | ap-internal |
| //mods | efrbroo:F1_Work, rdac:Work | rdaw:expressionOfWork | cerif:cfResPubl, efrbroo:F22_Self-Contained_Expression, rdac:Expression | ap-internal |
| //mods | cerif:cfResPubl, efrbroo:F22_Self-Contained_Expression, rdac:Expression | | | * |
| //mods | cerif:cfResPubl, efrbroo:F22_Self-Contained_Expression, rdac:Expression | rdae:workExpressed | efrbroo:F1_Work, rdac:Work | ap-internal |
| //mods | cerif:cfResPubl, efrbroo:F22_Self-Contained_Expression, rdac:Expression | efrbroo:R3_realises | efrbroo:F1_Work, rdac:Work | ap-internal |
| //mods | cerif:cfResPubl, efrbroo:F22_Self-Contained_Expression, rdac:Expression | rdae:manifestationOfExpression | efrbroo:F3_Manifestation_Product_Type, rdac:Manifestation | ap-internal |
| //mods | cerif:cfResPubl, efrbroo:F22_Self-Contained_Expression, rdac:Expression | efrbroo:R4_carriers_provided_by | efrbroo:F3_Manifestation_Product_Type, rdac:Manifestation | ap-internal |
| //mods | efrbroo:F3_Manifestation_Product_Type, rdac:Manifestation | | | ap-internal |
| //mods | efrbroo:F3_Manifestation_Product_Type, rdac:Manifestation | rdam:expressionManifested | cerif:cfResPubl, efrbroo:F22_Self-Contained_Expression, rdac:Expression | ap-internal |
| //mods | efrbroo:F3_Manifestation_Product_Type, rdac:Manifestation | efrbroo:R4_comprises_carriers_of | cerif:cfResPubl, efrbroo:F22_Self-Contained_Expression, rdac:Expression | ap-internal |
| //mods/typeOfResource | cerif:cfResPubl, efrbroo:F22_Self-Contained_Expression, rdac:Expression | | | | 1 | 1 | type | type |
| concat(//mods/titleInfo/title, " : ", //mods/titleInfo/subTitle) | cerif:cfResPubl, efrbroo:F22_Self-Contained_Expression, rdac:Expression | dc:title | rdfs:Literal | ap-simple | 1 | 1 | | title |
| concat(//mods/titleInfo/title[@type="abbreviated"], " : ", //mods/titleInfo/subTitle[@type="abbreviated"]) | cerif:cfResPubl, efrbroo:F22_Self-Contained_Expression, rdac:Expression | dcterms:title | rdfs:Literal | ap-simple | 1 | 1 | | abbr_title |
| concat(//mods/titleInfo/title[@type="translated"], " : ", //mods/titleInfo/subTitle[@type="translated"]) | cerif:cfResPubl, efrbroo:F22_Self-Contained_Expression, rdac:Expression | dcterms:title | rdfs:Literal | ap-simple | 1 | 1 | | trans_title |
| concat(//mods/titleInfo/title[@type="uniform"], " : ", //mods/titleInfo/subTitle[@type="uniform"]) | cerif:cfResPubl, efrbroo:F22_Self-Contained_Expression, rdac:Expression | dcterms:title | rdfs:Literal | ap-simple | 1 | 1 | | uniform_title |
| concat(//mods/titleInfo/title[@type="alternative"], " : ", //mods/titleInfo/subTitle[@type="alternative"]) | cerif:cfResPubl, efrbroo:F22_Self-Contained_Expression, rdac:Expression | dcterms:title | rdfs:Literal | ap-simple | 1 | 0 | | alt_title
| concat(//mods/titleInfo/title[@type="enumerated"], " : ", //mods/titleInfo/subTitle[@type="enumerated"]) | cerif:cfResPubl, efrbroo:F22_Self-Contained_Expression, rdac:Expression | dcterms:title | rdfs:Literal | ap-simple | 1 | 0 | | enum_title
| //mods/language/languageTerm[@type="code" and @authority="iso639-2b"] | cerif:cfResPubl, efrbroo:F22_Self-Contained_Expression, rdac:Expression | dc:language | rdfs:Literal (ISO 639) | ap-simple | 1 | 1 | lang | lang |
| //mods/language/languageTerm[@type="code" and @authority="iso639-2b"] | cerif:cfResPubl, efrbroo:F22_Self-Contained_Expression, rdac:Expression | rdae:languageOfExpression | rdfs:Literal (ISO 639) | ap-internal |
| //mods/abstract[@sharable="no"] | cerif:cfResPubl, efrbroo:F22_Self-Contained_Expression, rdac:Expression | dc:abstract | rdfs:Literal | ap-simple | 1 | 0 | | abstract |
| //mods/abstract | cerif:cfResPubl, efrbroo:F22_Self-Contained_Expression, rdac:Expression | dc:abstract | rdfs:Literal |ap-simple | 1 | 1 | | abstract |
| //mods/tableOfContents[@xlink:href] | cerif:cfResPubl, efrbroo:F22_Self-Contained_Expression, rdac:Expression | dc:tableOfContents | rdfs:Literal | ap-simple | 0 | 0 | | |
| //mods/subject[@authority="mesh"]/topic | cerif:cfResPubl, efrbroo:F22_Self-Contained_Expression, rdac:Expression | dc:subject | skos:Concept | ap-simple | 1 | 1 | fsubject | subject |
| //mods/subject[@authority="thesoz"]/topic | cerif:cfResPubl, efrbroo:F22_Self-Contained_Expression, rdac:Expression | dc:subject | skos:Concept | ap-simple | 1 | 1 | fsubject | subject |
| //mods/subject[@authority="stw"]/topic | cerif:cfResPubl, efrbroo:F22_Self-Contained_Expression, rdac:Expression | dc:subject | skos:Concept | ap-simple | 1 | 1 | fsubject | subject |
| //mods/subject[@authority="lcsh"]/topic | cerif:cfResPubl, efrbroo:F22_Self-Contained_Expression, rdac:Expression | dc:subject | skos:Concept | ap-simple | 1 | 1 | fsubject | subject |
| //mods/identifier[@type="isbn"] | cerif:cfResPubl, efrbroo:F22_Self-Contained_Expression, rdac:Expression | dc:identifier | rdfs:Literal |ap-simple | 1 | 1 | | isbn |
| //mods/identifier[@type="issn"] | cerif:cfResPubl, efrbroo:F22_Self-Contained_Expression, rdac:Expression | dc:identifier | rdfs:Literal |ap-simple | 1 | 1 | | issn |
| //mods/identifier[@type="doi"] | cerif:cfResPubl, efrbroo:F22_Self-Contained_Expression, rdac:Expression | dc:identifier | rdfs:Literal |ap-simple | 1 | 1 | | doi |
| //mods/identifier[@type="pm"] | cerif:cfResPubl, efrbroo:F22_Self-Contained_Expression, rdac:Expression | dc:identifier | rdfs:Literal |ap-simple | 1 | 1 | | pmid |
| //mods/identifier[@type="isi"] | cerif:cfResPubl, efrbroo:F22_Self-Contained_Expression, rdac:Expression | dc:identifier | rdfs:Literal |ap-simple | 1 | 1 | | wos_id |
| //mods/tableOfContents | cerif:cfResPubl, efrbroo:F22_Self-Contained_Expression, rdac:Expression | rdae:containerOfExpression | efrbroo:F23_Expression_Fragment, rdac:Expression | ap-internal | 1 | 1 | | toc |
| //mods/tableOfContents | cerif:cfResPubl, efrbroo:F22_Self-Contained_Expression, rdac:Expression | efrbroo:R15_has_fragment | efrbroo:F23_Expression_Fragment, rdac:Expression | ap-internal | 1 | 1 | | toc |
| //name[@type="personal" and starts-with(@valueURI, "http://d-nb.info/gnd/") and @authority="gnd"]/@valueURI **AND** //name[@type="personal" and starts-with(@valueURI, "http://d-nb.info/gnd/") and @authority="gnd"]/role/roleTerm = "edt" | cerif:cfResPubl, efrbroo:F22_Self-Contained_Expression, rdac:Expression | rdae:editor.en | foaf:Person, cerif:cfPers, schema:Person, efrbroo:F10_Person, rdac:Person | ap-internal |
| //mods/tableOfContents | cerif:cfResPubl, efrbroo:F23_Expression_Fragment, rdac:Expression | | | ap-internal |
| //mods/tableOfContents | cerif:cfResPubl, efrbroo:F23_Expression_Fragment, rdac:Expression | efrbroo:R4_carriers_provided_by | efrbroo:F3_Manifestation_Product_Type, rdac:Manifestation | ap-internal |
| //mods/tableOfContents | efrbroo:F3_Manifestation_Product_Type, rdac:Manifestation | | | ap-internal |
| //mods/tableOfContents[@xlink:href] | efrbroo:F3_Manifestation_Product_Type, rdac:Manifestation | rdam:uniformResourceLocator | rdfs:Literal | ap-internal |
| //name[@type="personal" and starts-with(@valueURI, "http://d-nb.info/gnd/") and @authority="gnd"]/@valueURI **AND** //name[@type="personal" and starts-with(@valueURI, "http://d-nb.info/gnd/") and @authority="gnd"]/role/roleTerm = "aut" | efrbroo:F1_Work, rdac:Work | rdaw:hasAuthor | foaf:Person, cerif:cfPers, schema:Person, efrbroo:F10_Person, rdac:Person | ap-internal |
| concat(//mods/titleInfo/title, " : ", //mods/titleInfo/subTitle) | efrbroo:F1_Work, rdac:Work | rdaw:preferredTitleForTheWork | rdfs:Literal | ap-internal |
| concat(//mods/titleInfo/title[@type="abbreviated"], " : ", //mods/titleInfo/subTitle[@type="abbreviated"]) | efrbroo:F1_Work, rdac:Work | rdaw:titleOfTheWork | rdfs:Literal | ap-internal |
| concat(//mods/titleInfo/title[@type="translated"], " : ", //mods/titleInfo/subTitle[@type="translated"]) | efrbroo:F1_Work, rdac:Work | rdaw:titleOfTheWork | rdfs:Literal | ap-internal |
| concat(//mods/titleInfo/title[@type="uniform"], " : ", //mods/titleInfo/subTitle[@type="uniform"]) | efrbroo:F1_Work, rdac:Work | rdaw:titleOfTheWork | rdfs:Literal | ap-internal |
| concat(//mods/titleInfo/title[@type="alternative"], " : ", //mods/titleInfo/subTitle[@type="alternative"]) | efrbroo:F1_Work, rdac:Work | rdaw:variantTitleForTheWork | rdfs:Literal | ap-internal |
| concat(//mods/titleInfo/title[@type="enumerated"], " : ", //mods/titleInfo/subTitle[@type="enumerated"]) | efrbroo:F1_Work, rdac:Work | rdaw:titleOfTheWork | rdfs:Literal | ap-internal |
| //mods/subject[@authority="mesh"]/topic | efrbroo:F1_Work, rdac:Work | rdaw:subjectRelationship | skos:Concept | ap-internal |
| //mods/subject[@authority="thesoz"]/topic | efrbroo:F1_Work, rdac:Work | rdaw:subjectRelationship | skos:Concept | ap-internal |
| //mods/subject[@authority="stw"]/topic | efrbroo:F1_Work, rdac:Work | rdaw:subjectRelationship | skos:Concept | ap-internal |
| //mods/subject[@authority="lcsh"]/topic | efrbroo:F1_Work, rdac:Work | rdaw:subjectRelationship | skos:Concept | ap-internal |
| //mods/abstract | efrbroo:F1_Work, rdac:Work | | | ap-internal |
| //mods/abstract | efrbroo:F1_Work, rdac:Work | rdaw:abstractedAsWork | efrbroo:F1_Work, rdac:Work | ap-internal |
| //mods/abstract[@sharable="no"] | efrbroo:F1_Work, rdac:Work | rdaw:abstractedAsWork | efrbroo:F1_Work, rdac:Work | ap-internal |
| //mods/identifier[@type="isbn"] | efrbroo:F3_Manifestation_Product_Type, rdac:Manifestation | rdam:identifierForTheManifestation | rdfs:Literal | ap-internal |
| //mods/identifier[@type="issn"] | efrbroo:F3_Manifestation_Product_Type, rdac:Manifestation | rdam:identifierForTheManifestation | rdfs:Literal | ap-internal |
| //mods/identifier[@type="doi"] | efrbroo:F3_Manifestation_Product_Type, rdac:Manifestation | rdam:identifierForTheManifestation | rdfs:Literal | ap-internal |
| //mods/identifier[@type="pm"] | efrbroo:F3_Manifestation_Product_Type, rdac:Manifestation | rdam:identifierForTheManifestation | rdfs:Literal | ap-internal |
| //mods/identifier[@type="isi"] | efrbroo:F3_Manifestation_Product_Type, rdac:Manifestation | rdam:identifierForTheManifestation | rdfs:Literal | ap-internal |
| //mods/originInfo/publisher | efrbroo:F3_Manifestation_Product_Type, rdac:Manifestation | rdam:publisher | gnd:Organisation | ap-internal |
| //mods/originInfo/publisher | efrbroo:F3_Manifestation_Product_Type, rdac:Manifestation | rdam:publishersName | rdfs:Literal | ap-internal |
| //mods/originInfo/dateIssued | efrbroo:F3_Manifestation_Product_Type, rdac:Manifestation | rdam:dateOfPublication | rdfs:Literal | ap-internal |
| //mods/originInfo/place/placeTerm[@type='text'] | efrbroo:F3_Manifestation_Product_Type, rdac:Manifestation | rdam:placeOfPublication | rdfs:Literal | ap-internal |
| //mods/originInfo/physicalDescription/extent | efrbroo:F3_Manifestation_Product_Type, rdac:Manifestation | rdam:extent | rdfs:Literal | ap-internal |
| //mods/relatedItem | depends on type attribute and on contained current()/genre | | | |

Examples on GitHubGist: 
* Elliptische Differentialgleichungen zweiter Ordnung / Wienholtz, Ernst; Kalf, Hubert; Kriecherbauer, Thomas ([Dataset in Ruhr-Universität Bochum, Research Bibliography & Repository](https://bibliographie.ub.rub.de/export/xml/mods?q=id%3Ac8ab7d97-47a6-4647-acc6-f5f0fe05a5fe)): 
	* [book description in terms of the application profile 'ap-internal-public'](https://gist.github.com/hagbeck/bb08ae55556bbda1314c) 
	* [book description in terms of the application profile 'ap-internal-nonpublic'](https://gist.github.com/hagbeck/8f108a8889cb12a2dedb) 



**non-independent publication**

depends on `//mods/genre`: {TODO list of relevant values}

* use static FRBRoo model: 
	* `efrbroo:F1_Work`, `rdac:Work` 
	* `efrbroo:R3_is_realised_in (realises)`, `rdaw:expressionOfWork (rdae:workExpressed)`
	* `efrbroo:F22_Self-Contained_Expression`, `rdac:Expression`
		* `efrbroo:F1_Work`, `rdac:Work` 
		* `efrbroo:R3_is_realised_in (realises)`, `rdaw:expressionOfWork (rdae:workExpressed)`
		* `efrbroo:R15_has_fragment (is_fragment_of)`, `rdae:containerOfExpression (rdae:containedInExpression)`
		* `efrbroo:F23_Expression_Fragment`, `rdac:Expression`
		* (`efrbroo:R4_carriers_provided_by (comprises_carriers_of)`, `rdae:manifestationOfExpression (rdae:manifestationOfExpression)`)* 
		* (`efrbroo:F3_Manifestation_Product_Type`, `rdac:Manifestation`)* 
		* (`efrbroo:R7_has_example (is_example_of)`, `rdam:exemplarOfManifestation (rdai:manifestationExemplified)`)*
		* (`efrbroo:F5_Item`, `rdac:Item`)*
	* `efrbroo:R4_carriers_provided_by (comprises_carriers_of)`, `rdae:manifestationOfExpression (rdae:manifestationOfExpression)` 
	* `efrbroo:F3_Manifestation_Product_Type`, `rdac:Manifestation`
	* `efrbroo:R7_has_example (is_example_of)`, `rdam:exemplarOfManifestation (rdai:manifestationExemplified)`
	* `efrbroo:F5_Item`, `rdac:Item`
* roles for contributors in RDA: ["has contributor" and its subproperties](http://www.rdaregistry.info/Elements/e/#P20053)

\* if the Expression Fragement is realised in an independent publication using its own persistent identifier


| Source | Target Entities | Target Property | Target Value Type | Application Profile | Search | Display | Facet | Index |
| ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ |
| //mods/relatedItem[@type="host"] | efrbroo:F17_Aggregation_Work, rdac:Work | | | ap-internal |
| //mods/relatedItem[@type="host"] | efrbroo:F17_Aggregation_Work, rdac:Work | efrbroo:R3_is_realised_in | efrbroo:F22_Self-Contained_Expression, rdac:Expression | ap-internal |
| //mods/relatedItem[@type="host"] | efrbroo:F17_Aggregation_Work, rdac:Work | rdaw:expressionOfWork | efrbroo:F22_Self-Contained_Expression, rdac:Expression | ap-internal |
| //mods/relatedItem[@type="host"] | efrbroo:F22_Self-Contained_Expression, rdac:Expression | | | ap-internal |
| //mods/relatedItem[@type="host"] | efrbroo:F22_Self-Contained_Expression, rdac:Expression | rdae:workExpressed | efrbroo:F17_Aggregation_Work, rdac:Work | ap-internal |
| //mods/relatedItem[@type="host"] | efrbroo:F22_Self-Contained_Expression, rdac:Expression | efrbroo:R3_realises | efrbroo:F17_Aggregation_Work, rdac:Work | ap-internal |
| //mods/relatedItem[@type="host"] | efrbroo:F22_Self-Contained_Expression, rdac:Expression | rdae:manifestationOfExpression | efrbroo:F3_Manifestation_Product_Type, rdac:Manifestation | ap-internal |
| //mods/relatedItem[@type="host"] | efrbroo:F22_Self-Contained_Expression, rdac:Expression | efrbroo:R4_carriers_provided_by | efrbroo:F3_Manifestation_Product_Type, rdac:Manifestation | ap-internal |
| //mods/relatedItem[@type="host"] | efrbroo:F22_Self-Contained_Expression, rdac:Expression | rdae:containerOfExpression | cerif:cfResPubl, efrbroo:F23_Expression_Fragment, rdac:Expression | ap-internal |
| //mods/relatedItem[@type="host"] | efrbroo:F22_Self-Contained_Expression, rdac:Expression | efrbroo:R15_has_fragment | cerif:cfResPubl, efrbroo:F23_Expression_Fragment, rdac:Expression | ap-internal |
| //mods/relatedItem[@type="host"] | efrbroo:F3_Manifestation_Product_Type, rdac:Manifestation | | | ap-internal |
| //mods/relatedItem[@type="host"] | efrbroo:F3_Manifestation_Product_Type, rdac:Manifestation | rdam:expressionManifested | efrbroo:F22_Self-Contained_Expression, rdac:Expression | ap-internal |
| //mods/relatedItem[@type="host"] | efrbroo:F3_Manifestation_Product_Type, rdac:Manifestation | efrbroo:R4_comprises_carriers_of | efrbroo:F22_Self-Contained_Expression, rdac:Expression | ap-internal |
| //mods | efrbroo:F14_Individual_Work, rdac:Work | | | ap-internal |
| //mods | efrbroo:F14_Individual_Work, rdac:Work | efrbroo:R3_is_realised_in | cerif:cfResPubl, efrbroo:F23_Expression_Fragment, rdac:Expression | ap-internal |
| //mods | efrbroo:F14_Individual_Work, rdac:Work | rdaw:expressionOfWork | cerif:cfResPubl, efrbroo:F23_Expression_Fragment, rdac:Expression | ap-internal |
| //mods | cerif:cfResPubl, efrbroo:F23_Expression_Fragment, rdac:Expression | | | * |
| //mods | cerif:cfResPubl, efrbroo:F23_Expression_Fragment, rdac:Expression | rdae:workExpressed | efrbroo:F14_Individual_Work, rdac:Work | ap-internal |
| //mods | cerif:cfResPubl, efrbroo:F23_Expression_Fragment, rdac:Expression | efrbroo:R3_realises | efrbroo:F14_Individual_Work, rdac:Work | ap-internal |
| //mods | cerif:cfResPubl, efrbroo:F23_Expression_Fragment, rdac:Expression | rdae:containedInExpression | efrbroo:F22_Self-Contained_Expression, rdac:Expression | ap-internal |

Examples on GitHubGist: 
* Uniform asymptotics for polynomials orthogonal with respect to a general class of discrete weights and universality results for associated ensembles / Baik, Jinho; Kriecherbauer, Thomas; McLaughlin, Kenneth T-R; Miller, Peter David ([Dataset in Ruhr-Universität Bochum, Research Bibliography & Repository](https://bibliographie.ub.rub.de/export/xml/mods?q=id%3Ae0007d60-6426-4273-bfca-561b87ca8036)): 
	* [journal article description in terms of the application profile 'ap-internal-public'](https://gist.github.com/hagbeck/b6e7058ca257c0d2fbb7) 



## Patent 

depends on `//mods/genre`: {TODO list of relevant values}

| Source | Target Entity | Target Property | Target Value Type | Application Profile | Search | Display | Facet | Index |
| ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ |
| //mods | cerif:cfResPat | | |



## Product

[The CERIF XML Product Entity in the OpenAIRE context](https://guidelines.openaire.eu/wiki/OpenAIRE_Guidelines:_For_CRIS#The_CERIF_XML_Product_Entity_in_the_OpenAIRE_context):
Collection, Dataset, Event, Film, Image, InteractiveResource, Model, PhysicalObject, Service, Software, Sound, Text

depends on `//mods/genre`: {TODO list of relevant values}

| Source | Target Entity | Target Property | Target Value Type | Application Profile | Search | Display | Facet | Index |
| ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ |
| //mods | cerif:cfResProd | | |



# Properties and Classes for Administrative Data

For each created resource description we additionally create a resource to describe the description. In this we gather information about the state, the provenance, access conditions and the licence for reuse of the data.


| Source | Target Entities | Target Property | Target Value Type | Application Profile | Search | Display | Facet | Index |
| ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ |
| //mods | powder:Document, ecrm:E31_Document, prov:Entity | | | * | 
| //mods | powder:Document, ecrm:E31_Document, prov:Entity | powder:describedby | rdfs:Literal | * | 
| //mods | powder:Document, ecrm:E31_Document, prov:Entity | cc:licence | rdfs:Literal | * | 
| //mods | powder:Document, ecrm:E31_Document, prov:Entity | cc:attributionURL | rdfs:Literal | * | 
| //mods | powder:Document, ecrm:E31_Document, prov:Entity | cc:attributionName| rdfs:Literal | * | 
| //mods/recordInfo/recordCreationDate | powder:Document, ecrm:E31_Document, prov:Entity | dcterms:created | rdfs:Literal | * | 
| //mods/recordInfo/recordChangeDate | powder:Document, ecrm:E31_Document, prov:Entity | dcterms:modified | rdfs:Literal | * | 
| //mods | powder:Document, ecrm:E31_Document, prov:Entity | dcterms:hasVersion | skos:Concept | * | 
| //mods | powder:Document, ecrm:E31_Document, prov:Entity | dcterms:accessRights | skos:Concept | * | 

Example:

	<rdf:RDF 
		xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#" 
		xmlns:powder="http://www.w3.org/2007/05/powder-s#"
		xmlns:cc="http://creativecommons.org/ns#"
		xmlns:dct="http://purl.org/dc/terms/" 
	>
		<rdf:Description rdf:about="http://data.uaruhr.de/resource/1/about">
			<rdf:type rdf:resource="http://www.w3.org/2007/05/powder-s#Document" />
			<rdf:type rdf:resource="http://www.w3.org/ns/prov#Entity" />
			<rdf:type rdf:resource="http://erlangen-crm.org/120111/E31_Document" />
			<powder:describedby rdf:resource="http://data.uaruhr.de/resource/1/about-meta" />
			<cc:licence rdf:resource="http://creativecommons.org/publicdomain/zero/1.0/legalcode" />
			<cc:attributionURL rdf:resource="http://www.uaruhr.de/bibliotheken_en.php" />
			<cc:attributionName>University Alliance Ruhr, University Libraries</attributionName>
			<dcterms:created>2015-02-07</created>
			<dcterms:modified>2015-06-19</modified>
			<dcterms:hasVersion rdf:resource="http://data.uaruhr.de/resource/concept:notEdited" />	
			<dct:accessRights rdf:resource="http://data.uaruhr.de/resource/concept:public" />
		</rdf:Description>
	</rdf:RDF>

# Properties and Classes for Person Profiles

| Source | Target Entities | Target Property | Target Value Type | Application Profile | Search | Display | Facet | Index |
| ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ |
| UserProfile > pid | | | | | | | | |
| UserProfile > image | | | | | | | | |
| UserProfile > email | | | | | | | | |
| URL > type | | | | | | | | |
| URL > label | | | | | | | | |
| Interest > label | | | | | | | | |
| Award > start | | | | | | | | |
| Award > end | | | | | | | | |
| Award > label | | | | | | | | |
| CV > start | | | | | | | | |
| CV > end | | | | | | | | |
| CV > label | | | | | | | | |
| Project > start | | | | | | | | |
| Project > end | | | | | | | | |
| Project > label | | | | | | | | |
| Membership > label | | | | | | | | |
| Reviewer > label | | | | | | | | |