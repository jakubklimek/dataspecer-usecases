# 001 Real use case 1
1. Open editor as standalone app
2. Add reused vocabulary: [dcterms](https://www.dublincore.org/specifications/dublin-core/dcmi-terms/dublin_core_terms.ttl) by its URL
3. Be able to browse the vocabulary - see the classes and properties, search in them, be able to place them on canvas

# 002 Vocabulary creation workflow using standalone editor
1. Open conceptual model editor - in this case, it will not actually create a **conceptual model** though, because it is not an **application profile**, just a **vocabulary**
2. Add reused vocabulary, e.g. [dcterms](https://www.dublincore.org/specifications/dublin-core/dcmi-terms/dublin_core_terms.ttl), via URL
    1. They are added as "Dataspecer model"
3. Create new classes, e.g. "Město", "Zoo", "Savec", "Kočička", "Pejsek"
    1. This involves establishing an IRI for the class, when it is exported => Need for IRI base and possibility of customization
    2. Name, description
4. Create specializations
    1. Between my new classes: Kočička and Savec, Pejsek and Savec
    2. Add class dcterms:Location (somehow, from the reused vocabularies)
    3. Between my class and an external class: Město and dcterms:Location
        1. This will probably create a Dataspecer "filter node" adding dcterms:Location to the filter
            1. Poznámka: Tady záleží, jestli dělám jen slovník (pak je třeb dcterms celý načtený a filtr nedělám, nebo něco jiného, pak můžu chtít filtrovat)
5. Create relations
    1. "Město" --[1..1]--"obsahuje Zoo"--[0..1]-->"Zoo"
    2. "Zoo" <--[1..1]--"žije v"--[0..*]--"Savec"
    3. again, this involves IRIs, names, descriptions
6. Create attributes
    1. "Město name"
    2. "Kočička name"
    3. Involves creating IRIs, names, range?
7.  Create attribute specializations
    1. "Město name" subPropertyOf dcterms:title
        1. Again adds these to the "DS filter node"?
    2. "Kočička name" subPropertyOf dcterms:title
        1. Again adds these to the "DS filter node"?
    3. Kdybych tady přidal vlastnost (teď to ale nedělám), která nemá kompatibilní domain, bude to nevalidní. To můžu upravit tím, že udělám "dataspecer model typu patch". **Tohle ale není ve scopu aktuálního use casu.**
8.  Download the vocabulary as **lightweight ontology** (má to být standalone v editoru, nebo v dataspeceru, nebo obojí?)
9.  Save project to file (and be able to load it)

# 003 Vocabulary creation workflow using Dataspecer+CME
Same as before, but integrated in DS
1. Land on a landing page with overview of existing **data specifications**
2. Create a new **data specification**. The intention is to create a **Vocabulary**, no need for "PSM"
3. The original use case 002, but instead of saving file, it will be saved to Dataspecer backend
4. Return to landing page where now I could continue by creating data structures.

# 004 Application profile creation
1. Land on a landing page with overview of existing **data specifications**
2. Create a new **data specification**. The intention is to create an **Application profile**, but still no need for "PSM". We will simulate DCAT here.
3. Open conceptual model editor
2. Add reused vocabulary, e.g. [dcterms](https://www.dublincore.org/specifications/dublin-core/dcmi-terms/dublin_core_terms.ttl), via URL
    1. They are added as "Dataspecer model"
3. Create new classes "Dataset", "Data Service", "Dataset Series" (subclass of Dataset)
    1. This involves establishing an IRI for the class, when it is exported => Need for IRI base and possibility of customization
    2. Name, description
    3. So far, we have done nothing to indicate an application profile, just a vocabulary
4. Create new relation `dcat:inSeries`
    1. domain: dcat:Dataset
    2. range: dcat:DatasetSeries
    3. multiplicity: --"0..\*"---inSeries---"0..\*"-->
5. Add attribute usage of `dcterms:title` to "Dataset".
    1. The name will remain "title"
    2. add definition/description "A name given to the dataset."
6. Add attribute usage of `dcterms:title` to "Distribution".
    1. The name will be "data service title"
    2. add definition/description "A name given to the data service."
7. Add attribute usage of `dcterms:issued` to "Dataset"
    1. The name will be "release date" (originally "issued")
    2. The range will be `xsd:date` (originally `rdfs:Literal`)
        1. Actually, DCAT says `xsd:gYear`, `xsd:gYearMonth`, `xsd:date`, or `xsd:dateTime`
8. Add class usage of `dcterms:Location`
    1. The name will be "Location"
    2. Usage note will be: "For an extensive geometry (i.e., a set of coordinates denoting the vertices of the relevant geographic area), the property locn:geometry LOCN SHOULD be used."
9.  Add attribute usage of `dcterms:spatial`
    1. name: "geographical coverage"
    2. domain: `dcat:Dataset`
    3. range: usage of `dcterms:Location`
10. Save to Dataspecer backend
11. Return to landing page where now I could continue by creating data structures.

# 005 AP of AP
1. Land on a landing page with overview of existing **data specifications**
2. Create a new **data specification**. The intention is to create an **Application profile**, but still no need for "PSM". We will simulate DCAT-AP here.
3. Open conceptual model editor
2. Add reused AP DCAT from 004
    1. They are added as "Dataspecer model"
    2. this should also add the reused dcterms
3. Add "Dataset" and "Dataset Series" to the canvas (class usage)
4. Add "Dataset" to the canvas and rename to "Dataset member of a Dataset Series" (class usage)
    1. Usage note "this is dataset that is part of a dataset series"
5. Add attribute usage of `dcat:inSeries`
    1. domain: "Dataset member of a Dataset Series"
    2. range: "Dataset Series"
    3. multiplicity --"0..\*"---inSeries---"1..\*"-->
10. Save to Dataspecer backend
11. Return to landing page where now I could continue by creating data structures.
