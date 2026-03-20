# Content Structure

## Shared Fields Across Content Types

> **Note:** This document was produced on 2026-03-05 using GitHub Copilot (Claude Opus 4.6). A Python script parsed the Drupal field configuration files (`config/sync/field.field.node.*.yml`) to extract field names, types, labels, cardinality, and content-type assignments. The AI assisted in writing the script, interpreting the results, and formatting the output. All data is derived directly from the project's exported configuration and should be verified against the live site if discrepancies are suspected.

### Overview

* **Total content types:** 10
* **Total unique fields on nodes:** 97
* **Fields shared across 2+ content types:** 13

### Content Types

| Machine name       | Label            |
| ------------------ | ---------------- |
| `alert_submission` | Alert submission |
| `basic_page`       | Basic page       |
| `forum`            | Forum topic      |
| `qualified_entity` | Qualified Entity |
| `rad_case`         | RAD Case         |
| `space_info_page`  | Info page        |
| `ws_article`       | Article          |
| `ws_event`         | Event            |
| `ws_request`       | Request          |
| `ws_subarea`       | Folder           |

### Shared Fields Matrix

The table below shows which fields are reused across multiple content types.

| Field                              | Label                      | Type                         | Cardinality | alert\_submission | basic\_page | forum | qualified\_entity | rad\_case | space\_info\_page | ws\_article | ws\_event | ws\_request | ws\_subarea |
| ---------------------------------- | -------------------------- | ---------------------------- | ----------- | ----------------- | ----------- | ----- | ----------------- | --------- | ----------------- | ----------- | --------- | ----------- | ----------- |
| `body`                             | Body                       | text\_with\_summary          | 1           | ✓                 | ✓           | ✓     | ✓                 |           | ✓                 | ✓           | ✓         | ✓           | ✓           |
| `field_m_banner`                   | Banner                     | entity\_reference            | 1           |                   | ✓           | ✓     | ✓                 |           | ✓                 | ✓           | ✓         | ✓           | ✓           |
| `field_space`                      | Space                      | strm\_space\_machine\_name   | 1           |                   |             | ✓     | ✓                 |           | ✓                 | ✓           | ✓         | ✓           | ✓           |
| `field_keywords`                   | Keywords                   | entity\_reference            | Unlimited   |                   |             | ✓     |                   |           | ✓                 | ✓           | ✓         | ✓           | ✓           |
| `field_attachments`                | Attachments (legacy)       | entity\_reference            | Unlimited   |                   |             | ✓     | ✓                 |           | ✓                 | ✓           |           | ✓           |             |
| `field_attachment`                 | Attachment items           | entity\_reference\_revisions | Unlimited   |                   |             | ✓     | ✓                 |           |                   |             | ✓         |             | ✓           |
| `field_ms_of_designation`          | MS of designation          | entity\_reference            | 1           | ✓                 |             |       | ✓                 | ✓         |                   |             |           |             |             |
| `field_comments`                   | Comments                   | comment                      | 1           |                   |             |       |                   |           |                   | ✓           | ✓         |             |             |
| `field_drupal_7_id`                | Drupal 7 ID                | integer                      | 1           |                   |             |       |                   |           |                   | ✓           |           |             | ✓           |
| `field_operational_responder_type` | Operational responder type | list\_string                 | 1           |                   |             | ✓     |                   |           |                   |             |           | ✓           |             |
| `field_operational_responders`     | Recipients                 | entity\_reference            | Unlimited   |                   |             | ✓     |                   |           |                   |             |           | ✓           |             |
| `field_qualified_entity_workflow`  | Transition                 | strm\_workflow\_state        | 1           |                   |             |       | ✓                 | ✓         |                   |             |           |             |             |
| `field_related_forum_topic`        | Related forum topic        | entity\_reference            | 1           | ✓                 |             |       |                   | ✓         |                   |             |           |             |             |

### Shared Fields Detail

#### `body` — Body (9 content types)

* **Type:** `text_with_summary`
* **Cardinality:** 1
* **Used in:** Alert submission, Basic page, Forum topic, Qualified Entity, Info page, Article, Event, Request, Folder

#### `field_m_banner` — Banner (8 content types)

* **Type:** `entity_reference`
* **Cardinality:** 1
* **Used in:** Basic page, Forum topic, Qualified Entity, Info page, Article, Event, Request, Folder

#### `field_space` — Space (7 content types)

* **Type:** `strm_space_machine_name`
* **Cardinality:** 1
* **Used in:** Forum topic, Qualified Entity, Info page, Article, Event, Request, Folder

#### `field_keywords` — Keywords (6 content types)

* **Type:** `entity_reference`
* **Cardinality:** Unlimited
* **Used in:** Forum topic, Info page, Article, Event, Request, Folder

#### `field_attachments` — Attachments (legacy) (5 content types)

* **Type:** `entity_reference`
* **Cardinality:** Unlimited
* **Used in:** Forum topic, Qualified Entity, Info page, Article, Request

#### `field_attachment` — Attachment items (4 content types)

* **Type:** `entity_reference_revisions`
* **Cardinality:** Unlimited
* **Used in:** Forum topic, Qualified Entity, Event, Folder

#### `field_ms_of_designation` — MS of designation (3 content types)

* **Type:** `entity_reference`
* **Cardinality:** 1
* **Used in:** Alert submission, Qualified Entity, RAD Case

#### `field_comments` — Comments (2 content types)

* **Type:** `comment`
* **Cardinality:** 1
* **Used in:** Article, Event

#### `field_drupal_7_id` — Drupal 7 ID (2 content types)

* **Type:** `integer`
* **Cardinality:** 1
* **Used in:** Article, Folder

#### `field_operational_responder_type` — Operational responder type (2 content types)

* **Type:** `list_string`
* **Cardinality:** 1
* **Used in:** Forum topic, Request

#### `field_operational_responders` — Recipients (2 content types)

* **Type:** `entity_reference`
* **Cardinality:** Unlimited
* **Used in:** Forum topic, Request

#### `field_qualified_entity_workflow` — Transition (2 content types)

* **Type:** `strm_workflow_state`
* **Cardinality:** 1
* **Used in:** Qualified Entity, RAD Case

#### `field_related_forum_topic` — Related forum topic (2 content types)

* **Type:** `entity_reference`
* **Cardinality:** 1
* **Used in:** Alert submission, RAD Case

### All Fields by Content Type

#### Alert submission (`alert_submission`) — 18 fields

| Field                              | Label                                           | Type                | Shared |
| ---------------------------------- | ----------------------------------------------- | ------------------- | ------ |
| `body`                             | Description of incident or concern              | text\_with\_summary | ✓      |
| `field_alert_type`                 | What kind of alert would you like to submit?    | list\_string        |        |
| `field_alert_type_other`           | If Other, please specify                        | string              |        |
| `field_comments_or_additional_not` | Comments or additional notes                    | text\_long          |        |
| `field_concerned_platforms`        | Online platform(s) concerned by the alert       | entity\_reference   |        |
| `field_content_type`               | Content type                                    | list\_string        |        |
| `field_date_of_incident_or_detect` | Date of incident or initial detection           | datetime            |        |
| `field_etoh_attachments`           | Attachments                                     | file                |        |
| `field_etoh_declaration`           | Declaration confirmation                        | boolean             |        |
| `field_ground_of_hate`             | Ground of hate                                  | entity\_reference   |        |
| `field_ground_of_hate_other`       | Ground of hate other                            | text                |        |
| `field_has_this_content_already_b` | Has this content already been flagged?          | boolean             |        |
| `field_links_of_the_related_trend` | Links of the related alerts                     | entity\_reference   |        |
| `field_ms_of_designation`          | Primary national context concerned by the alert | entity\_reference   | ✓      |
| `field_related_forum_topic`        | Related forum topic                             | entity\_reference   | ✓      |
| `field_related_organisations`      | Related organisations                           | entity\_reference   |        |
| `field_secondary_national_context` | Secondary national context(s)                   | entity\_reference   |        |
| `field_url_s_link_s`               | URL(s) / Link(s)                                | link                |        |

#### Basic page (`basic_page`) — 2 fields

| Field            | Label  | Type                | Shared |
| ---------------- | ------ | ------------------- | ------ |
| `body`           | Body   | text\_with\_summary | ✓      |
| `field_m_banner` | Banner | entity\_reference   | ✓      |

#### Forum topic (`forum`) — 10 fields

| Field                              | Label                      | Type                         | Shared |
| ---------------------------------- | -------------------------- | ---------------------------- | ------ |
| `body`                             | Body                       | text\_with\_summary          | ✓      |
| `comment_forum`                    | Comments                   | comment                      |        |
| `field_attachment`                 | Attachment items           | entity\_reference\_revisions | ✓      |
| `field_attachments`                | Attachments (legacy)       | entity\_reference            | ✓      |
| `field_keywords`                   | Keywords                   | entity\_reference            | ✓      |
| `field_m_banner`                   | Banner                     | entity\_reference            | ✓      |
| `field_operational_responder_type` | Operational responder type | list\_string                 | ✓      |
| `field_operational_responders`     | Recipients                 | entity\_reference            | ✓      |
| `field_space`                      | Space                      | strm\_space\_machine\_name   | ✓      |
| `taxonomy_forums`                  | Forums                     | entity\_reference            |        |

#### Qualified Entity (`qualified_entity`) — 31 fields

| Field                              | Label                                        | Type                         | Shared |
| ---------------------------------- | -------------------------------------------- | ---------------------------- | ------ |
| `body`                             | Body                                         | text\_with\_summary          | ✓      |
| `field_attachment`                 | Attachment items                             | entity\_reference\_revisions | ✓      |
| `field_attachments`                | Attachments (legacy)                         | entity\_reference            | ✓      |
| `field_city`                       | City                                         | string                       |        |
| `field_email_address`              | E-mail address(es)                           | string                       |        |
| `field_fax_number`                 | Fax number                                   | string                       |        |
| `field_m_banner`                   | Banner                                       | entity\_reference            | ✓      |
| `field_ms_of_designation`          | MS of designation                            | entity\_reference            | ✓      |
| `field_name_of_the_entity_in_your` | Name of the entity in your national language | string                       |        |
| `field_phone_number`               | Phone number                                 | string                       |        |
| `field_postal_code`                | Postal code                                  | string                       |        |
| `field_purpose_bg`                 | Statutory Purpose – Bulgarian                | text\_with\_summary          |        |
| `field_purpose_cs`                 | Statutory Purpose – Czech                    | text\_with\_summary          |        |
| `field_purpose_da`                 | Statutory Purpose – Danish                   | text\_with\_summary          |        |
| `field_purpose_de`                 | Statutory Purpose – German                   | text\_with\_summary          |        |
| `field_purpose_el`                 | Statutory Purpose – Greek                    | text\_with\_summary          |        |
| `field_purpose_es`                 | Statutory Purpose – Spanish                  | text\_with\_summary          |        |
| `field_purpose_et`                 | Statutory Purpose – Estonian                 | text\_with\_summary          |        |
| `field_purpose_fi`                 | Statutory Purpose – Finnish                  | text\_with\_summary          |        |
| `field_purpose_fr`                 | Statutory Purpose – French                   | text\_with\_summary          |        |
| `field_purpose_hr`                 | Statutory Purpose – Croatian                 | text\_with\_summary          |        |
| `field_purpose_hu`                 | Statutory Purpose – Hungarian                | text\_with\_summary          |        |
| `field_purpose_it`                 | Statutory Purpose – Italian                  | text\_with\_summary          |        |
| `field_purpose_lt`                 | Statutory Purpose – Lithuanian               | text\_with\_summary          |        |
| `field_purpose_lv`                 | Statutory Purpose – Latvian                  | text\_with\_summary          |        |
| `field_purpose_mt`                 | Statutory Purpose – Maltese                  | text\_with\_summary          |        |
| `field_purpose_nl`                 | Statutory Purpose – Dutch                    | text\_with\_summary          |        |
| `field_purpose_pl`                 | Statutory Purpose – Polish                   | text\_with\_summary          |        |
| `field_purpose_pt_pt`              | Statutory Purpose – Portuguese, Portugal     | text\_with\_summary          |        |
| `field_purpose_ro`                 | Statutory Purpose – Romanian                 | text\_with\_summary          |        |
| `field_purpose_sk`                 | Statutory Purpose – Slovak                   | text\_with\_summary          |        |
| `field_purpose_sl`                 | Statutory Purpose – Slovenian                | text\_with\_summary          |        |
| `field_purpose_sv`                 | Statutory Purpose – Swedish                  | text\_with\_summary          |        |
| `field_qe_character`               | Character                                    | list\_string                 |        |
| `field_qualified_entity_workflow`  | Transition                                   | strm\_workflow\_state        | ✓      |
| `field_space`                      | Space                                        | strm\_space\_machine\_name   | ✓      |
| `field_street_and_number`          | Street and number                            | string                       |        |
| `field_type_of_qualified_entity`   | Type of Qualified Entity                     | list\_string                 |        |
| `field_website`                    | Website(s)                                   | string                       |        |

#### RAD Case (`rad_case`) — 24 fields

| Field                              | Label                                                 | Type                  | Shared |
| ---------------------------------- | ----------------------------------------------------- | --------------------- | ------ |
| `feeds_item`                       | Feeds item                                            | feeds\_item           |        |
| `field_annoucement_channels`       | Announcement channels                                 | string                |        |
| `field_announcement_on_final_outc` | Announcement on final outcome                         | string                |        |
| `field_case_identification_number` | Action identification number                          | text                  |        |
| `field_case_start_date`            | Action start date                                     | datetime              |        |
| `field_case_status`                | Action status                                         | string\_long          |        |
| `field_communication_sources`      | Communication materials sources                       | string                |        |
| `field_compensation_amount`        | Compensation Amount                                   | text\_long            |        |
| `field_court_name`                 | Court Name                                            | text                  |        |
| `field_defendant_1`                | Defendant (1)                                         | text                  |        |
| `field_defendant_2`                | Defendant (2)                                         | text                  |        |
| `field_defendant_domicile`         | Defendant Domicile                                    | string                |        |
| `field_domestic_cross_border`      | Domestic/Cross border                                 | list\_string          |        |
| `field_ms_of_designation`          | MS of designation                                     | entity\_reference     | ✓      |
| `field_number_of_consumers_affect` | Number of consumers affected                          | string\_long          |        |
| `field_opt_in_or_opt_out_mechanis` | Opt-in or opt-out mechanism                           | string                |        |
| `field_plaintiff`                  | Plaintiff                                             | string                |        |
| `field_plaintiff_or_qe_website`    | Plaintiff's or QE's website on specific case          | text\_long            |        |
| `field_qualified_entity_workflow`  | Transition                                            | strm\_workflow\_state | ✓      |
| `field_related_forum_topic`        | Related forum topic                                   | entity\_reference     | ✓      |
| `field_represented_by`             | Represented by                                        | string                |        |
| `field_scope_of_action`            | Scope of action                                       | text                  |        |
| `field_the_injunctive_and_or_redr` | Measures to obtain                                    | string                |        |
| `field_type_of_funding`            | Type of funding                                       | string                |        |
| `field_website_announcement_date`  | Communication date of initial announcement on website | datetime              |        |

#### Info page (`space_info_page`) — 5 fields

| Field               | Label                | Type                       | Shared |
| ------------------- | -------------------- | -------------------------- | ------ |
| `body`              | Body                 | text\_with\_summary        | ✓      |
| `field_attachments` | Attachments (legacy) | entity\_reference          | ✓      |
| `field_keywords`    | Keywords             | entity\_reference          | ✓      |
| `field_m_banner`    | Banner               | entity\_reference          | ✓      |
| `field_space`       | Space                | strm\_space\_machine\_name | ✓      |

#### Article (`ws_article`) — 10 fields

| Field                   | Label                | Type                       | Shared |
| ----------------------- | -------------------- | -------------------------- | ------ |
| `body`                  | Body                 | text\_with\_summary        | ✓      |
| `field_article_subarea` | Subarea              | entity\_reference          |        |
| `field_attachments`     | Attachments (legacy) | entity\_reference          | ✓      |
| `field_comments`        | Comments             | comment                    | ✓      |
| `field_drupal_7_id`     | Drupal 7 ID          | integer                    | ✓      |
| `field_keywords`        | Keywords             | entity\_reference          | ✓      |
| `field_m_banner`        | Banner               | entity\_reference          | ✓      |
| `field_shared_editing`  | Shared Editing       | list\_string               |        |
| `field_space`           | Space                | strm\_space\_machine\_name | ✓      |
| `field_third_country`   | Third country        | entity\_reference          |        |

#### Event (`ws_event`) — 9 fields

| Field                     | Label                         | Type                         | Shared |
| ------------------------- | ----------------------------- | ---------------------------- | ------ |
| `body`                    | Body                          | text\_with\_summary          | ✓      |
| `field_attachment`        | Attachment items              | entity\_reference\_revisions | ✓      |
| `field_comments`          | Comments                      | comment                      | ✓      |
| `field_event_date`        | Date                          | daterange                    |        |
| `field_event_type`        | Event Type                    | entity\_reference            |        |
| `field_keywords`          | Keywords                      | entity\_reference            | ✓      |
| `field_m_banner`          | Banner                        | entity\_reference            | ✓      |
| `field_registration_link` | External Registration Service | link                         |        |
| `field_space`             | Space                         | strm\_space\_machine\_name   | ✓      |

#### Request (`ws_request`) — 10 fields

| Field                              | Label                      | Type                       | Shared |
| ---------------------------------- | -------------------------- | -------------------------- | ------ |
| `body`                             | Body                       | text\_with\_summary        | ✓      |
| `field_attachments`                | Attachments (legacy)       | entity\_reference          | ✓      |
| `field_keywords`                   | Keywords                   | entity\_reference          | ✓      |
| `field_m_banner`                   | Banner                     | entity\_reference          | ✓      |
| `field_operational_responder_type` | Operational responder type | list\_string               | ✓      |
| `field_operational_responders`     | Recipients                 | entity\_reference          | ✓      |
| `field_request_objectives`         | Request objectives         | entity\_reference          |        |
| `field_request_responses`          | Responses                  | comment                    |        |
| `field_request_workflow_state`     | Request state              | strm\_workflow\_state      |        |
| `field_space`                      | Space                      | strm\_space\_machine\_name | ✓      |

#### Folder (`ws_subarea`) — 10 fields

| Field                  | Label            | Type                         | Shared |
| ---------------------- | ---------------- | ---------------------------- | ------ |
| `body`                 | Body             | text\_with\_summary          | ✓      |
| `field_attachment`     | Attachment items | entity\_reference\_revisions | ✓      |
| `field_drupal_7_id`    | Drupal 7 ID      | integer                      | ✓      |
| `field_keywords`       | Keywords         | entity\_reference            | ✓      |
| `field_m_banner`       | Banner           | entity\_reference            | ✓      |
| `field_parent_subarea` | Parent subarea   | entity\_reference            |        |
| `field_space`          | Space            | strm\_space\_machine\_name   | ✓      |
| `field_subarea_logo`   | Logo             | entity\_reference            |        |
| `field_subarea_mode`   | Mode             | list\_string                 |        |
| `field_weight`         | Weight           | integer                      |        |
