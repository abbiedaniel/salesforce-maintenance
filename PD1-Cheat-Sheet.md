# PD1 Cheat Sheet

<details>
	<summary>Dev Fundamentals</summary>

## Save Order of Execution
1. System Validation
2. Before Save Flows
3. Before Triggers
4. Validation Rules and System Validation
5. Duplicate Rules
6. _Save to database but not committed_
7. After Trigger
8. Assignment Rules
9. Auto response Rules
10. Workflow Rules
11. Escalation Rules
12. Flow Automation
13. After Save Flows
14. Commit to database

**S**am's **F**amily **T**ook **V**alerie **D**own **S**outh **T**o **A** **A**uto **W**orkshop's **E**nclosed **F**oyer.

## Governor Limits
- SOQL Queries: 100
- DML: 150

## Model View Controller Architecture
- Model: where data is saved
- View: how data is visualized
- Controller: how data is manipulated/logic

## Data Imports & Exports

|   | Data Import Wizard      | Data Loader |
| ----------- | ----------- | ----------- |
| **Max Records** | 50K    | 5M       |
| **Pros** | - Choose whether to trigger workflow rules   | - Can save mapping for later use <br> - Can delete and export data    |
| **Cons** |  - Can only insert, update or upsert <br> - Can't use on product and opportunities <br> - Can't save mappings <br> - Can't schedule imports   | - No option to turn off workflow rules |

</details>

<details>
	<summary>Process Automation</summary>
 </details>
 
<details>
	<summary>Testing & Deployments</summary>
 </details>
 
<details>
	<summary>Lightning Web Components</summary>
 </details>
 
<details>
	<summary>Visualforce</summary>
 </details>
