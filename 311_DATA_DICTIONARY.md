# Data Dictionary: Columbus 311 Service Requests (2025)

## requests (fact table, 209,577 rows, one row per case + request type)
| Field | Type | Definition |
|---|---|---|
| case_id | text | City case number. One case can include more than one request type |
| reported_date | datetime | When the resident reported the request (Eastern time) |
| report_month | text (YYYY-MM) | Month the request was reported |
| request_type | text | Specific issue reported. Joins to `request_types` |
| status_group | text | Closed, Canceled, or Open (Open = received, in progress, re-assigned, on hold, or work order issued) |
| days_to_close | decimal | Days from report to close. Only for Closed requests that have a close date |
| closed_within_7_days | true/false | Closed within 7 days of being reported. Blank if not timed |
| councildistrict | text | Columbus City Council district (District 1–9, or Unknown) |
| columbuscommunity | text | Columbus community/neighborhood name, or Unknown |
| zip | text | ZIP code, or Unknown |

## request_types (dimension table, 468 rows)
| Field | Type | Definition |
|---|---|---|
| request_type | text | Specific issue reported (key) |
| request_category | text | Broad category (e.g., Trash, Recycling, Yard Waste and Illegal Dumping Issues) |
| department_name | text | City department responsible |
| division_name | text | Division within the department |

## Business rules
- **Duplicate** = same `case_id` and `request_type`. The most recent status update is kept.
- **Internal staff logs** (the *City Staff Requests* category and *Information/Notes* request types) are excluded. They are staff notes, not resident requests.
- **Timing metrics** use Closed requests with a close date only. Canceled, open, and closed-without-a-date requests are excluded from timing but included in volume.
- Blank labels are filled with **Unknown**.

## Metrics (Power BI DAX measures)
| Measure | Definition |
|---|---|
| Total Requests | Count of rows in `requests` |
| Avg Days to Close | Average of `days_to_close` |
| % Closed Within 7 Days | Closed within 7 days ÷ timed requests |
| Open Requests | Requests with `status_group` = Open |
| MoM % Change | This month's requests vs. last month's |
