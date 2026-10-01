## Power BI Notes

![][image1]

# **Power BI Fundamentals**

* Power BI is Microsoft's business analytics platform for connecting to data, preparing/modeling it, visualizing it, and sharing insights.  
* Power BI Desktop: authoring—connect, transform, model, create visuals and publish.  
* Power BI Service: cloud platform—publish, share, refresh, secure, apps, dashboards, subscriptions and governance.  
* Power BI Mobile: consume and interact with reports/dashboards on mobile devices.  
* Semantic model: tables, columns, relationships, measures and model metadata used by reports.  
* Report: one or more pages containing visuals connected to a semantic model.  
* Dashboard: a single-page canvas in Power BI Service made of pinned tiles; it is not the same thing as a report.  
* Workspace: collaborative container where Power BI content is developed, stored and managed.  
* App: packaged, audience-facing distribution of workspace content.

| Term | Remember |
| :---- | :---- |
| Report | Multi-page interactive analysis |
| Dashboard | Single-page Service pinboard |
| Semantic model | Data/model/calculation layer |
| Workspace | Collaboration and content management |
| App | Curated distribution to consumers |

# **Get or Connect to Data**

* Common sources: Excel, CSV/Text, SQL Server and other relational databases, web, SharePoint, cloud services, dataflows, semantic models and other supported connectors.  
* Know the difference between connecting to a source and connecting to an existing shared semantic model.  
* Understand credentials, data source settings and privacy levels.  
* Parameters can store reusable values such as server/database names, file paths, dates or environment switches.  
* Storage/connectivity modes to know: Import, DirectQuery and Direct Lake.

| Mode | Core idea | Focus |
| :---- | :---- | :---- |
| Import | Data is loaded into the Power BI model | Fast in-memory analysis; refresh required |
| DirectQuery | Model queries source at interaction time | Near-real-time scenarios; source performance matters |

# **Power Query — Core Concepts**

* Power Query is the data preparation/ETL layer used to extract, transform and load data.  
* Query pane: list of queries/tables.  
* Preview grid: inspect transformed data.  
* Applied Steps: ordered, named transformation history; steps can be selected, reordered or deleted when dependencies permit.  
* Power Query uses the M language behind the UI.  
* Query folding: Power Query pushes supported transformations back to the source instead of processing everything locally.  
* Folding is especially important for large relational sources. Keep foldable filters and projections early when practical.  
* Reference creates a new query based on another query's output; Duplicate creates a copy of the query logic.  
* Disable load for staging/intermediate queries that do not need to become model tables.  
* Use clear names and organize staging, dimensions and facts.

# **Power Query — Data Profiling and Quality**

* Column quality: valid, error and empty values.  
* Column distribution: distinct and unique value information.  
* Column profile: statistics and distribution for the selected column.  
* Always inspect data types and representative samples before transformations.  
* Null/blank \= missing value; Error \= a value exists but cannot be evaluated/converted correctly.  
* Typical null actions: replace, fill down/up, remove rows, or preserve null if it is meaningful.  
* Typical error actions: remove errors, replace errors, or correct the underlying conversion/source issue.  
* Duplicates should be removed only when they are genuinely duplicates according to the business grain.  
* Redundant columns can often be removed if the same information is already represented by a proper dimension or can be derived safely.  
* Text cleaning: Trim removes leading/trailing spaces; Clean removes non-printable characters; casing transformations standardize text.  
* Dates: mixed locale formats may require locale-aware conversion; do not assume DD/MM and MM/DD are interchangeable.  
* Currency stored as text: remove symbols/separators or use appropriate locale-aware conversion, then set a numeric type.

# **Power Query — Transformations You Must Know**

* Filter rows: keep/remove rows based on values or conditions.  
* Remove columns / choose columns: reduce unnecessary data early.  
* Change data type: Text, Whole Number, Decimal Number, Fixed Decimal, Date, Date/Time, Time, Logical and other supported types.  
* Split column: by delimiter, number of characters or positions.  
* Replace values: standardize categories, remove symbols or correct known bad values.  
* Fill Down / Fill Up: propagate values into nulls when the source structure supports it.  
* Group By: aggregate rows into summaries.  
* Pivot: turn row values into columns.  
* Unpivot: turn columns into attribute-value rows; commonly used to normalize wide spreadsheets.  
* Transpose: switch rows and columns; use carefully because it can make data structure harder to maintain.  
* Conditional Column: create logic using rules.  
* Custom Column: create an M expression.  
* Index Column: create a sequential/indexing column when appropriate.  
* Format: upper/lower/proper, trim, clean and other text operations.  
* Extract: substring, text before/after delimiter, length, etc.  
* Date transformations: year, quarter, month, week, day and date arithmetic.  
* Convert semi-structured data such as JSON into usable tables by expanding records/lists as appropriate.

# **Merge vs Append**

| Feature | Merge | Append |
| :---- | :---- | :---- |
| Purpose | Combine columns from related tables | Stack rows from similar tables |
| Think | JOIN | UNION ALL |
| Result | More columns | More rows |
| Typical example | Order\_Items \+ Products using ProductID | Orders \+ Orders\_Q1 \+ Orders\_Q2 |
| Requirement | A matching key/columns | Compatible column structure |

**POWER BI NOTE:** Your original shortcut is excellent: 'Need new columns about rows you have → Merge. Need new rows like the ones you have → Append.' fileciteturn0file0L59-L76

# **Power Query — Keys, Fact and Dimension Preparation**

* A key identifies an entity or row at the intended grain.  
* Natural/business key: comes from the source/business process. Surrogate key: generated key used in a model, often an integer.  
* Fact table: events/transactions at a defined grain, e.g., one order line or one daily sales transaction.  
* Dimension table: descriptive entities such as Customer, Product, Date, Region.  
* Before loading, verify grain, uniqueness of dimension keys, data types, missing keys and duplicate business records.  
* A dimension key should normally be unique on the 'one' side of a one-to-many relationship.

# **Query Folding and Transformation Order**

* Prefer source-side filtering/projection where possible.  
* Typical efficient sequence: filter rows → remove unnecessary columns → set types → foldable transformations → combine/group → custom/non-foldable logic after volume is minimized.  
* Not every transformation folds, and folding depends on the connector/source.  
* Do not assume that a UI action always folds; verify where necessary using the available folding diagnostics/tools.  
* If folding stops, subsequent transformations may execute in the Power Query engine rather than the source.

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAnAAAAAECAYAAAAOPwJdAAAANElEQVR4Xu3WMQ0AMAwEseAOoJIpqGYvgrzkwcshuLqnHwAAOeoPAADsZuAAAMIYOACAMAOTpGslhjl7rwAAAABJRU5ErkJggg==> "horizontal line"