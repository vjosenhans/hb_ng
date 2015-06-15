# Properties and Classes for Authority Data

**Die CERIF-Ontologie 1.3 ist in ihrer derzeitgen Form unbrauchbar!** Das Problem ist, dass sämtliche Eigenschaften als "Classes" und keinerlei "Properties" modelliert sind (bspw. wird der Name einer Person so zu einer Klasse anstatt zu einer Eigenschaft einer Entität.). 

Folge: Wir verfolgen CERIF erstmal nur in der Form weiter, dass wir die Entitäten der Kernontologie verwenden, diese aber mittels "Ontology Allignments" und Relationen zu anderen Entitäten beschreiben.

<http://www.eurocris.org/ontologies/semcerif/1.3#Person> owl:sameAs <http://xmlns.com/foaf/0.1/Person>
<http://www.eurocris.org/ontologies/semcerif/1.3#OrgUnit> owl:sameAs <http://xmlns.com/foaf/0.1/Organization>

**Diese Mappings sind im Sinne der obeigen Bemerkung zur CERIF-Ontologie zunächst nur zum Zweck der Information aufgeführt.**

| Source | Target Entity | Target Property |
| ------ | ------ | ------ |
| //name[@type="personal" and not(@authority)] | foaf:Person | |
| //name[@type="personal" and not(@authority)]/namePart[@type="family"] | foaf:Person | foaf:familyName |
| //name[@type="personal" and not(@authority)]/namePart[@type="given"] | foaf:Person | foaf:givenName |
| //name[@type="personal" and starts-with(@valueURI, "http://d-nb.info/gnd/") and @authority="gnd"]/@valueURI | cerif:cfPers | cerif:cfPersID |
| //name[@type="personal" and starts-with(@valueURI, "http://d-nb.info/gnd/") and @authority="gnd"]/@valueURI | cerif:cfPers | owl:sameAs |
| //name[@type="personal" and starts-with(@valueURI, "http://d-nb.info/gnd/") and @authority="gnd"]/namePart[@type="family"] | cerif:cfPersName | cerif:cfFamilyNames (*) |
| //name[@type="personal" and starts-with(@valueURI, "http://d-nb.info/gnd/") and @authority="gnd"]/namePart[@type="given"] | cerif:cfPersName | cerif:cfFirstNames (*) |
| //name[@type="personal"]/role/roleTerm[@type="code" and @authorityURI="http://www.loc.gov/marc/relators/"] | must be defined at the publication lavel | |
| //name[@type="corporate" and not(@authority)] | foaf:Organization | |
| //name[@type="corporate" and not(@authority)]/namePart | foaf:Organization | foaf:name |
| //name[@type="corporate" and starts-with(@valueURI, "http://d-nb.info/gnd/") and @authority="gnd"]/@valueURI | cerif:cfOrgUnitName | cerif:cfOrgUnitID |
| //name[@type="corporate" and starts-with(@valueURI, "http://d-nb.info/gnd/") and @authority="gnd"]/@valueURI | cerif:cfOrgUnitName | owl:sameAs |
| //name[@type="corporate" and starts-with(@valueURI, "http://d-nb.info/gnd/") and @authority="gnd"]/namePart | cerif:cfOrgUnitName | cerif:cfName (*) |
| //name[@type="corporate"]/role/roleTerm[@type="code" and @authorityURI="http://www.loc.gov/marc/relators/"] | must be defined at the publication lavel | |

# Properties and Classes for Bibliographic Description


## Publications (depends on //mods/genre)

| Source | Target Entity | Target Property |
| ------ | ------ | ------ |
| concat(//mods/titleInfo/title, " : ", //mods/titleInfo/subTitle) | cfResPubl | dc:title |
| //mods/titleInfo/title | cfResPublTitle | cfTitle (*) |
| //mods/titleInfo/subTitle | cfRePublSubtitle | cfSubtitle (*) |
| concat(//mods/titleInfo/title[@type="abbreviated"], " : ", //mods/titleInfo/subTitle[@type="abbreviated"]) | cfResPubl | dcterms:title |
| //mods/titleInfo[@type="abbreviated"]/title | cfResPublNameAbbrev | cfNameAbbrev (*) |
| concat(//mods/titleInfo/title[@type="translated"], " : ", //mods/titleInfo/subTitle[@type="translated"]) | cfResPubl | dcterms:title |
| concat(//mods/titleInfo/title[@type="uniform"], " : ", //mods/titleInfo/subTitle[@type="uniform"]) | cfResPubl | dcterms:title |
| concat(//mods/titleInfo/title[@type="alternative"], " : ", //mods/titleInfo/subTitle[@type="alternative"]) | cfResPubl | dcterms:title |
| concat(//mods/titleInfo/title[@type="enumerated"], " : ", //mods/titleInfo/subTitle[@type="enumerated"]) | cfResPubl | dcterms:title |

## Patent (depends on //mods/genre)

| Source | Target Entity | Target Property |
| ------ | ------ | ------ |
| concat(//mods/titleInfo/title, " : ", //mods/titleInfo/subTitle) | cfResPat | dc:title |
| //mods/titleInfo/title | cfResPatTitle | cfTitle (*) |
| concat(//mods/titleInfo/title[@type="abbreviated"], " : ", //mods/titleInfo/subTitle[@type="abbreviated"]) | cfResPat | dcterms:title |
| concat(//mods/titleInfo/title[@type="translated"], " : ", //mods/titleInfo/subTitle[@type="translated"]) | cfResPat | dcterms:title |
| concat(//mods/titleInfo/title[@type="uniform"], " : ", //mods/titleInfo/subTitle[@type="uniform"]) | cfResPat | dcterms:title |
| concat(//mods/titleInfo/title[@type="alternative"], " : ", //mods/titleInfo/subTitle[@type="alternative"]) | cfResPat | dcterms:title |
| concat(//mods/titleInfo/title[@type="enumerated"], " : ", //mods/titleInfo/subTitle[@type="enumerated"]) | cfResPat | dcterms:title |

## Product (depends on //mods/genre)

| Source | Target Entity | Target Property |
| ------ | ------ | ------ |
| concat(//mods/titleInfo/title, " : ", //mods/titleInfo/subTitle) | cfResPat | dc:title |
| //mods/titleInfo/title | cfResProdName | cfName (*) |
| concat(//mods/titleInfo/title[@type="abbreviated"], " : ", //mods/titleInfo/subTitle[@type="abbreviated"]) | cfResProd | dcterms:title |
| concat(//mods/titleInfo/title[@type="abbreviated"], " : ", //mods/titleInfo/subTitle[@type="abbreviated"]) | cfResProdAltName | cfAltName (*) |
| concat(//mods/titleInfo/title[@type="translated"], " : ", //mods/titleInfo/subTitle[@type="translated"]) | cfResProd | dcterms:title |
| concat(//mods/titleInfo/title[@type="translated"], " : ", //mods/titleInfo/subTitle[@type="translated"]) | cfResProdAltName | cfAltName (*) |
| concat(//mods/titleInfo/title[@type="uniform"], " : ", //mods/titleInfo/subTitle[@type="uniform"]) | cfResPat | dcterms:title |
| concat(//mods/titleInfo/title[@type="uniform"], " : ", //mods/titleInfo/subTitle[@type="uniform"]) | cfResProdAltName | cfAltName (*) |
| concat(//mods/titleInfo/title[@type="alternative"], " : ", //mods/titleInfo/subTitle[@type="alternative"]) | cfResProd | dcterms:title |
| concat(//mods/titleInfo/title[@type="alternative"], " : ", //mods/titleInfo/subTitle[@type="alternative"]) | cfResProdAltName | cfAltName (*) |
| concat(//mods/titleInfo/title[@type="enumerated"], " : ", //mods/titleInfo/subTitle[@type="enumerated"]) | cfResProd | dcterms:title |
| concat(//mods/titleInfo/title[@type="enumerated"], " : ", //mods/titleInfo/subTitle[@type="enumerated"]) | cfResProdAltName | cfAltName (*) |

`dcterms:type` verlinkt auf eine kontrolliertes Vokabular (z.B. ein skos:ConceptScheme)
registerNamespace('xlink', 'http://www.w3.org/1999/xlink')

| Source | Target Entity | Target Property |
| ------ | ------ | ------ |
| //mods/language/languageTerm[@type="code" and @authority="iso639-2b"] | cfRes{Publ,Pat,Prod} | dc:language |
| //mods/genre[@authority="dct" starts-with(@valueURI="http://purl.org/dc/dcmitype/")] | cfRes{Publ,Pat,Prod} | dcterms:type |
| //mods/genre[not(@authority) and starts-with(@valueURI="http://purl.org/info:eu-repo/semantics/")] | cfRes{Publ,Pat,Prod} | dcterms:type |
| //mods/genre[@authority="local"] | cfRes{Publ,Pat,Prod} | dcterms:type |
| //mods/genre[@authority="marcgt"] | cfRes{Publ,Pat,Prod} | dcterms:type |
| //mods/typeOfResource | | |
| //mods/identifier[@type="isbn"] | | |
| //mods/identifier[@type="issn"] | | |
| //mods/relatedItem/identifier[@type="isbn"] | | |
| //mods/relatedItem/identifier[@type="issn"] | | |
| //mods/identifier[@type="doi"] | | |
| //mods/identifier[@type="pm"] | | |
| //mods/identifier[@type="isi"] | | |
| //mods/tableOfContents[@xlink:href] | | |
| //mods/accessCondition[@type="use and reproduction"] | | |
| //mods/accessCondition[@type="restriction on access"] | | |
| //mods/note | | |
| //mods/note[@type="publication status"] | | |
| //mods/note[@displayLabel="Preis"] | | |
| //mods/note[@displayLabel="Titelzusätze"] | | |
| //mods/subject[@authority="mesh"]/topic | | |
| //mods/subject[@authority="thesoz"]/topic | | |
| //mods/subject[@authority="stw"]/topic | | |
| //mods/subject[@authority="lcsh"]/topic | | |
| //mods/language/languageTerm | | |
| //mods/location/physicalLocation | | |
| //mods/location/shelfLocator | | |
| //mods/location/url | | |
| //mods/physicalDescription/extent | | |
| //mods/physicalDescription/internetMediaType | | |
| //mods/abstract[@sharable="no"] | | |
| //mods/abstract | | |
| //mods/recordInfo/recordCreationDate | | |
| //mods/recordInfo/recordChangeDate | | |
| //mods/extension/dcterms:bibliographicCitation | | |
| //mods/genre[@authority="dct" and @valueURI="http://purl.org/dc/dcmitype/Text"] | | |
| //mods/genre[starts-with(@valueURI, "http://purl.org/info:eu-repo/semantics/")] | | |

Properties and Classes for Administrative Data
==============================================
| Source | Target Entity | Target Property |
| ------ | ------ | ------ |
