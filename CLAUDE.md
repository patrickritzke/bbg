# Bloomberg Credit Risk V3 — Parquet Data Format

## Overview

This repo contains daily snapshot files from the **Bloomberg Credit Risk V3** dataset, delivered as Parquet files. Each file is a full snapshot of company/obligor reference data for a single business day.

**Filename pattern:** `creditRiskV3Sample-YYYYMMDD (1).parquet`

Each file holds 10 sample rows and **104 columns**.

---

## Envelope columns (present on every row)

| Column | Type | Description |
|---|---|---|
| `DL_DATASET` | string | Always `"creditRiskV3"` |
| `DL_SNAPSHOT_DATE` | date | Business date of the snapshot (e.g. `2026-03-17`) |
| `DL_SNAPSHOT_START_TIME` | timestamp (UTC) | When the snapshot extract started |
| `DL_SNAPSHOT_END_TIME` | timestamp (UTC) | When the snapshot extract completed |
| `COLUMN0` | string | Duplicate of `ID_BB_COMPANY` (string form); serves as the row key |
| `RC` | int64 | Return code — `0` means success/no error |

---

## Company identity

| Column | Type | Description |
|---|---|---|
| `ID_BB_COMPANY` | int64 | Bloomberg company numeric ID (primary key) |
| `LONG_COMP_NAME` | string | Display company name |
| `COMPANY_LEGAL_NAME` | string | Full registered legal name |
| `ALTERNATE_COMPANY_NAME` | list\<struct\> | Other trading/known names (`BC_ALTERNATE_NAME`) |
| `PREVIOUS_COMPANY_NAMES` | list\<struct\> | Historical names (`LEI_PREVIOUS_NAME`) |
| `COMPANY_CORP_TICKER` | string | Bloomberg ticker |
| `COMPANY_STATUS` | string | e.g. `"PRIV"` (private), `"ACTV"` (active public) |
| `COMPANY_IS_PRIVATE` | bool | True if privately held |
| `ISSUER_BBID` | int64 | Bloomberg issuer ID |
| `OBLIGOR_BBID` | string | Bloomberg obligor ID string |
| `ISSUER_NAME_TYPES` | string | e.g. `"Company"`, `"Funds"` |

---

## Corporate hierarchy

| Column | Type | Description |
|---|---|---|
| `ID_BB_PARENT_CO` | int64 | Bloomberg ID of direct parent company |
| `LONG_PARENT_COMP_NAME` | string | Parent company display name |
| `ID_BB_ULTIMATE_PARENT_CO` | int64 | Bloomberg ID of ultimate parent |
| `LONG_ULT_PARENT_COMP_NAME` | string | Ultimate parent display name |
| `ULT_PARENT_CORP_TICKER` | string | Ultimate parent Bloomberg ticker |
| `IS_ULT_PARENT` | bool | True if this entity is its own ultimate parent |
| `ACQUIRED_BY_PARENT` | bool | True if entity has been absorbed by parent |
| `COMPANY_TO_PARENT_RELATIONSHIP` | string | e.g. `"Fund Entity"` |
| `PARENT_OBLIGOR_ID` | string | Obligor ID of direct parent |
| `PARENT_OBLIGOR_NAME` | string | Name of direct parent obligor |
| `ULTIMATE_OBLIGOR_ID` | string | Obligor ID of ultimate parent |
| `ULTIMATE_OBLIGOR_NAME` | string | Name of ultimate parent obligor |

---

## Bloomberg Global IDs (BBG)

| Column | Type | Description |
|---|---|---|
| `ID_BB_GLOBAL_COMPANY` | string | e.g. `"BBG001H26RS8"` |
| `ID_BB_GLOBAL_PARENT_CO` | string | Global ID of parent |
| `ID_BB_GLOBAL_ULTIMATE_PARENT_CO` | string | Global ID of ultimate parent |
| `ID_BB_GLOBAL_OBLIGOR_COMPANY` | string | Global ID of obligor entity |
| `ID_BB_GLOBAL_COMPANY_NAME` | string | Name associated with global company ID |
| `ID_BB_GLOBAL_PARENT_COMPANY_NAME` | string | Name associated with global parent ID |
| `ID_BB_GLOBAL_ULT_PARENT_CO_NAME` | string | Name associated with global ult-parent ID |
| `ID_BB_GLOBAL_OBLIGOR_NAME` | string | Name associated with global obligor ID |

---

## Geography

| Column | Type | Description |
|---|---|---|
| `CNTRY_OF_DOMICILE` | string | ISO-2 country where company is domiciled |
| `CNTRY_OF_INCORPORATION` | string | ISO-2 country of incorporation |
| `CNTRY_OF_RISK` | string | ISO-2 country of risk |
| `COUNTRY_RISK_ISO_CODE` | string | ISO-2 country risk code (may differ) |
| `ISO_COUNTRY_OF_OBLIGOR` | string | ISO-2 for obligor |
| `STATE_OF_DOMICILE` | string | US state (where applicable) |
| `STATE_OF_INCORPORATION` | string | US state of incorporation |
| `ULT_PARENT_CNTRY_OF_RISK` | string | Ultimate parent country of risk |
| `ULT_PARENT_CNTRY_INCORPORATION` | string | Ultimate parent country of incorporation |
| `ULT_PARENT_CNTRY_DOMICILE` | string | Ultimate parent country of domicile |
| `ULT_PARENT_TICKER_EXCHANGE` | string | Exchange where ult-parent trades |
| `COMPANY_ADDRESS` | list\<struct\> | Street address lines (`BC_COMPANY_ADDRESS`) |
| `REGISTERED_COUNTRY_LOCATION` | string | Country of registered office |
| `REGISTERED_STATE_LOCATION` | string | State of registered office |
| `REGISTRATION_LOCATION` | list\<struct\> | Registered address lines (`BC_LOCATION`) |

---

## Industry classification

| Column | Type | Description |
|---|---|---|
| `INDUSTRY_SECTOR` | string | Top-level BICS sector (e.g. `"Consumer, Non-cyclical"`) |
| `INDUSTRY_GROUP` | string | BICS group (e.g. `"Commercial Services"`) |
| `INDUSTRY_SUBGROUP` | string | BICS sub-group |
| `OBLIG_INDUSTRY_SUBGROUP` | string | Obligor-level sub-group override |
| `INDUSTRY_SECTOR_NUM` | int64 | Numeric BICS sector code |
| `INDUSTRY_GROUP_NUM` | int64 | Numeric BICS group code |
| `INDUSTRY_SUBGROUP_NUM` | int64 | Numeric BICS sub-group code |
| `CLASSIFICATION_SCHEME` | string | Scheme used (always `"BICS"` in samples) |
| `CLASSIFICATION_LEVEL_1_NAME` | string | Level-1 classification label |
| `CLASSIFICATION_LEVEL_1_CODE` | string | Level-1 code |
| `CLASSIFICATION_LEVEL_2_NAME` | string | Level-2 classification label |
| `CLASSIFICATION_LEVEL_2_CODE` | string | Level-2 code |

---

## Legal Entity Identifier (LEI)

| Column | Type | Description |
|---|---|---|
| `LEGAL_ENTITY_IDENTIFIER` | string | 20-char GLEIF LEI code |
| `LEI_ENTITY_STATUS` | string | e.g. `"ACTIVE"` |
| `LEI_LEGAL_FORM` | string | Legal form as reported to GLEIF |
| `BLOOMBERG_LEGAL_FORM` | string | Bloomberg-normalised legal form (e.g. `"LLC"`) |
| `REGISTRATION_LEGAL_FORM` | string | Form per local registry |
| `LEI_REGISTRATION_DATE` | date | When LEI was first issued |
| `LEI_LAST_UPDATE` | date | Last LEI record update |
| `LEI_DISABLED_DATE` | date | When LEI was retired (null if active) |
| `LEI_NEXT_RENEWAL_DATE` | date | Next required renewal |
| `LEI_REGISTRATION_STATUS` | string | e.g. `"ISSUED"` |
| `LEI_MAINTENANCE_STATE` | string | Maintenance status |
| `LEI_NAME` | string | Official name in LEI record |
| `LEI_ULTIMATE_PARENT_COMPANY` | string | Ultimate parent per LEI record |
| `OTHER_LEI_NAME` | list\<struct\> | Additional LEI names (`LEI_NAME`) |
| `PREVIOUS_COMPANY_NAMES` | list\<struct\> | Previous names per LEI (`LEI_PREVIOUS_NAME`) |
| `LEI_LEGAL_ADDRESS` | list\<struct\> | Legal address lines (`BC_LEI_REGISTRATION_ADDRESS`) |
| `LEI_VALIDATION_SOURCES` | string | e.g. `"FULLY_CORROBORATED"` |
| `MANAGING_LOCAL_OPER_UNIT_NAME` | string | LOU managing the LEI |
| `MANAGING_LOCAL_OPER_UNIT_LEI` | string | LEI of the managing LOU |

---

## Regulatory & external identifiers

| Column | Type | Description |
|---|---|---|
| `REGISTRATION_IDENTIFIER` | string | Local registry number |
| `REGISTRATION_SOURCE` | string | Registry name (e.g. `"Netherlands Chamber of Commerce"`) |
| `CENTRAL_INDEX_KEY_NUMBER` | string | SEC CIK number |
| `GLOBAL_INTERMEDIARY_ID` | string | FATCA GIIN |
| `SPONSORING_ENTITY_GIIN` | string | FATCA sponsoring entity GIIN |
| `EKN_ISSUER_NUMBER` | string | EKN issuer reference |
| `RSSD_ID_NUM` | string | Federal Reserve RSSD ID (string) |
| `FDIC_RSSD_ID_NUM` | int64 | FDIC RSSD ID (numeric) |
| `COMPANY_TAX_IDENTIFIER` | string | Tax/VAT registration number |
| `BS_BASEL_LEVEL_ADOPTED_INDICATOR` | double | Basel level adopted |
| `SYNTHETIC_ENTITY_INDICATOR` | bool | True for synthetic/SPV entities |

---

## Operational details

| Column | Type | Description |
|---|---|---|
| `COMPANY_WEB_ADDRESS` | string | Company website |
| `COMPANY_TEL_NUMBER` | string | Phone number |
| `COMPANY_FAX_NUMBER` | string | Fax number |
| `DATE_OR_YEAR_OF_INCORPORATION` | string | Incorporation date (format varies) |
| `CUR_EMPLOYEES` | int64 | Current headcount |
| `LATEST_ANN_CIE_FILING` | int64 | Year of most recent annual filing |
| `COMPANY_AUDITOR` | string | Auditor name |
| `CURRENT_AUDITOR` | string | Current auditor name |
| `CURRENT_AUDITOR_DATE` | date | Date current auditor was appointed |
| `AUDITORS_OPINION` | string | e.g. `"Not audited"` |
| `ID_REPLACEMENT_IDENTIFIER` | int64 | Bloomberg ID of the entity that replaced this one |

---

## Nullable / sparse columns

Most columns are sparse — expect `null`/`NaN` for entities where that field is not applicable (e.g. no LEI for a US-only private company, no state fields for non-US entities).

List columns (`COMPANY_ADDRESS`, `LEI_LEGAL_ADDRESS`, `REGISTRATION_LOCATION`, `ALTERNATE_COMPANY_NAME`, `PREVIOUS_COMPANY_NAMES`, `OTHER_LEI_NAME`) are always present as arrays but may be empty (`[]`).

---

## Loading the data

```python
import pyarrow.parquet as pq
import pandas as pd

df = pq.read_table("creditRiskV3Sample-20260317 (1).parquet").to_pandas()

# Explode an address list column
addresses = df["COMPANY_ADDRESS"].explode().dropna()
```

```python
# Compare two daily snapshots
import pyarrow.parquet as pq

t1 = pq.read_table("creditRiskV3Sample-20260316 (1).parquet")
t2 = pq.read_table("creditRiskV3Sample-20260317 (1).parquet")
```

The natural join key between snapshots is `ID_BB_COMPANY` (int64).
