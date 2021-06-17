# merge-psychonautwiki-tripsit-data
Script to merge data from PsychonautWiki API + pages and TripSit factsheets into one standardized format

## Output Schema

| Property                  | Type                     | Description                                                                           | Source                                                                                 |
|---------------------------|--------------------------|---------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------|
| `url`                     | `string`                 | Substance page URL                                                                    | PsychonautWiki if exists, TripSit factsheet otherwise                                  |
| `name`                    | `string`                 | Substance Name                                                                        | shortest display name of PsychonautWiki page header and TripSit API's `pretty_name`    |
| `aliases`                 | `string[]`               | List of aliases (lowercase)                                                           | Both                                                                                   |
| `aliasesStr`              | `string`                 | Comma-separated list of aliases (for fuzzy searching)                                 | Both                                                                                   |
| `summary`                 | `string`                 | Substance summary                                                                     | TripSit                                                                                |
| `classes`                 | `null` \| `Dictionary`   | Substance classifications                                                             | PsychonautWiki                                                                         |
| `classes.chemical`        | `string[]`               | Chemical classifications                                                              | PsychonautWiki                                                                         |
| `classes.psychoactive`    | `string[]`               | Psychoactive classifications                                                          | PsychonautWiki                                                                         |
| `toxicity`                | `null` \| `string[]`     | List of toxicity characteristics                                                      | PsychonautWiki                                                                         |
| `addictionPotential`      | `null` \| `string`       | Description of potential for addiction                                                | PsychonautWiki                                                                         |
| `tolerance`               | `null` \| `Dictionary`   | Tolerance information                                                                 | PsychonautWiki                                                                         |
| `tolerance.full`          | `null` \| `string`       | When full tolerance activates                                                         | PsychonautWiki                                                                         |
| `tolerance.half`          | `null` \| `string`       | When tolerance is reduced by half                                                     | PsychonautWiki                                                                         |
| `tolerance.zero`          | `null` \| `string`       | When tolerance is back to zero                                                        | PsychonautWiki                                                                         |
| `crossTolerances`         | `null` \| `string[]`     | Other substances or classes with cross tolerance                                      | PsychonautWiki                                                                         |
| `bioavailability`         | `null` \| `string`       | Bioavailability of different ROAs (TODO: parse this string and merge into roas below) | TripSit                                                                                |
| `roas`                    | `Dictionary[]`           | Routes of administration                                                              | Both  (Note: PsychonautWiki ROAs are prioritized, and TripSit factsheets fill in gaps) |
| `roas[].name`             | `string`                 | Name of ROA (e.g. 'Oral', 'Smoked')                                                   | Both                                                                                   |
| `roas[].bioavailability`  | `Optional<string>`       | Substance's bioavailability for ROA                                                   | PsychonautWiki (TODO: parse TripSit factsheet string and merge with this)              |
| `roas[].dosage`           | `null` \| `Dictionary[]` | Dosage information for ROA                                                            | Both                                                                                   |
| `roas[].dosage[].name`    | `string`                 | Name of dosage level for ROA (e.g. 'Threshold', 'Strong')                             | Both                                                                                   |
| `roas[].dosage[].value`   | `string`                 | Typical dosage for given level of ROA (e.g. '10 - 25 mg', '100 mg +')                 | Both                                                                                   |
| `roas[].dosage[].note`    | `Optional<string>`       | Additional note                                                                       | PsychonautWiki                                                                         |
| `roas[].duration`         | `null` \| `Dictionary[]` | Duration information for ROA                                                          | Both                                                                                   |
| `roas[].duration[].name`  | `string`                 | Name of duration item for ROA (e.g. 'Onset', 'Peak')                                  | Both                                                                                   |
| `roas[].duration[].value` | `string`                 | Typical duration for given item of ROA (e.g. '30 - 60 minutes', '2 - 4 hours')        | Both                                                                                   |
| `roas[].duration[].note`  | `Optional<string>`       | Additional note                                                                       | PsychonautWiki                                                                         |
| `interactions`            | `null \| Dictionary[]`   | Interactions with other substances or classes                                         | TripSit                                                                                |
| `interactions[].name`     | `string`                 | Name of other substance or class                                                      | TripSit                                                                                |
| `interactions[].status`   | `string`                 | Interaction status from TripSit combos                                                | TripSit                                                                                |
| `interactions[].note`     | `Optional<string>`       | Additional note                                                                       | TripSit                                                                                |
