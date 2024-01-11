# ql-documentation
Documentation on how to use Strobes QL with GraphQL or API client

### What is Strobes QL?

Strobes QL is an advanced query language designed to search and filter data sets within Strobes at scale.

### Example

To filter all vulnerabilities with asset hostname containing wesecureapp.com, have an exploit available and are resolved for over 30 days, or to get all vulnerabilities with title containing SSL, use the following query:
```
(hostname ~ "wesecureapp.com" and exploit_available = "true" and created > "2021-12-01") or (title ~ "SSL")
```

### Supported Operators

| Operator | Description            | Example                        |
|----------|------------------------|--------------------------------|
| =        | Equal to operation     | title = "example.com"          |
| !=       | Not Equal to operation | exposure != 1                  |
| <        | Less than operation    | created < "2021-12-01"         |
| <=       | Less than equal to     | created <= "2021-12-01"        |
| >        | Greater than operation | created > "2021-12-01"         |
| >=       | Greater than equal to  | created >= "2021-12-01"        |
| ~        | Contains operation     | os ~ "linux"                   |
| !~       | Doesn't contain        | os !~ "windows"                |
| in       | In list operation      | os in ("windows2021", "linux") |
| not in   | Not in list operation  | port not in (80, 8080)         |
| ^        | Regex operation        | hostname ^ "(.*)"              |

### Conditions and Grouping

Supported conditions within QL:
- `and`
- `or`

Queries can be grouped and executed to filter or drill down the results further. Ensure queries are not heavily nested to prevent API timeouts.

### Fields Supported

#### Assets
Updated fields supported when using the `allAssets` GQL call or `/assets/` API call, based on the provided code structure.

| Field                 | Type           | Example                    | Reference |
|-----------------------|----------------|----------------------------|-----------|
| id (assetId)          | Number         | id = 1                     |           |
| name                  | String         | name ~ "Server"            |           |
| risk_score            | Number         | risk_score > 700           |           |
| sensitivity           | Severity       | sensitivity = "High"       |           |
| exposed (exposure)    | MultiSelect    | exposed = "Public"         |           |
| type                  | MultiSelect    | type in ("Server", "Router")|          |
| cloud_type            | MultiSelect    | cloud_type = "AWS"         |           |
| target (package)      | String         | target ~ "nginx"           |           |
| ipaddress             | String         | ipaddress = "192.168.0.1"  |           |
| mac_address           | String         | mac_address = "00:14:22:1A:2B:3C" |   |
| hostname (host)       | String         | hostname = "server.domain.com" |      |
| os                    | String         | os ~ "Linux"               |           |
| tags                  | Tags           | tags in ("critical", "external") |     |
| created_by.id         | MultiSelect    | created_by.id = 123        |           |
| created (createdOn)   | Date           | created > "2021-01-01"     |           |
| scan_created_by (importedBy) | MultiSelect | scan_created_by = 123   |           |
| connector (importedUsing) | MultiSelect | connector = "Nessus"      |           |
| port_number           | Number         | port_number = 80           |           |
| region (country)      | String         | region = "US"              |           |
| port_state            | MultiSelect    | port_state = "Open"        |           |
| port_product          | String         | port_product ~ "Apache"    |           |
| port_service          | String         | port_service ~ "HTTP"      |           |
| resource_id           | String         | resource_id ~ "resource123"|           |
| account_id            | String         | account_id = "123456789"   |           |
| region (cloudRegion)  | String         | region = "us-east-1"       |           |
| asset_package.package_name (package_name) | String | asset_package.package_name ~ "nginx" | |
| asset_package.installed_version (installed_version) | String | asset_package.installed_version = "1.14.2" | |
| asset_waf             | String         | asset_waf ~ "WAF Name"     |           |
| asset_cdn             | String         | asset_cdn ~ "CDN Name"     |           |
| technology            | String         | technology ~ "Technology Name" |       |
| asset_port.webserver  | String         | asset_port.webserver ~ "Webserver Name" | |
| tls_issuer_org        | String         | tls_issuer_org ~ "Issuer Name" |        |
| domain_registrar      | String         | domain_registrar ~ "Registrar Name" |    |
| ssl_expiration_date   | Date           | ssl_expiration_date < "2024-01-01" |     |
| domain_expiration_date| Date           | domain_expiration_date < "2023-12-31" |   |
| asset_region          | String         | asset_region ~ "Region Name" |          |
| a_type_record         | String         | a_type_record ~ "Record Value" |        |
| aaaa_type_record      | String         | aaaa_type_record ~ "Record Value" |     |
| cname_type_record     | String         | cname_type_record ~ "Record Value" |     |
| ns_type_record        | String         | ns_type_record ~ "Record Value" |        |
| axfr_type_record      | String         | axfr_type_record ~ "Record Value" |      |
| soa_type_record       | String         | soa_type_record ~ "Record Value" |       |

#### Vulnerabilities
These fields are supported when using the `allBugs` GQL call or `/bugs/` API call.

| Field                  | Type     | Example                       | Reference |
|------------------------|----------|-------------------------------|-----------|
| id                     | IntType  | id = 1                        |           |
| title                  | StrType  | title = "SSL"                 |           |
| severity               | IntType  | severity in (1,2)             | [Severity Types Reference](https://github.com/strobes-co/strobes-api-client#list_vulnerabilities) |
| status                 | IntType  | status in (2,3)               | [Status Types Reference](https://github.com/strobes-co/strobes-api-client#list_vulnerabilities) |
| prioritization_score   | IntType  | prioritization_score >= 700   |           |
| exploit_available      | BoolType | exploit_available = "true"    |           |
| asset.id               | IntType  | asset.id = 1                  |           |
| cve.cve_id             | StrType  | cve.cve_id = "CVE-2020-1234"  |           |
| assigned_to.id         | IntType  | assigned_to.id = 1            |           |
| assigned_to.email      | StrType  | assigned_to.email = "example@gmail.com" | |
| description            | StrType  | description = "test"          |           |
| created                | StrType  | created >= "2021-12-01"       |           |
| sla_violated           | BoolType | sla_violated = "true"         |           |
| patch_available        | BoolType | patch_available= "true"       |           |
| asset.exposed          | IntType  | asset.exposed = 1             | [Exposure Types Reference](https://github.com/strobes-co/strobes-api-client#list_vulnerabilities) |
| cvss                   | FloatType| cvss > 9.0                    |           |
| asset.type             | IntType  | asset.type = 1                | [Asset Types Reference](https://github.com/strobes-co/strobes-api-client#list_vulnerabilities) |
| asset.sensitivity      | IntType  | asset.sensitivity = 1         | [Asset Sensitivity Types Reference](https://github.com/strobes-co/strobes-api-client#list_vulnerabilities) |
| bug_tags.name          | StrType  | bug_tags.name in ("v1", "tag2") |       |
| cwe.cwe_id             | StrType  | cwe.cwe_id in ("CWE-89", "CWE-90") |     |
| ipaddress              | StrType  | ipaddress = "192.168.0.1"     |           |
| mac_address            | StrType  | mac_address = "12:13:1E:12"   |           |
| hostname               | StrType  | hostname = "wesecureapp.com"  |           |
| bug_tracker.issue_key  | StrType  | bug_tracker.issue_key = "JIRA-123" |   |
| exploit_status                 | MultiSelect | exploit_available = "true"   |           |
| zero_day                       | MultiSelect | zero_day_available = "true"  |           |
| affected_asset                 | Number   | asset.id = 1                  |           |
| cve                            | String   | cve.cve_id = "CVE-2020-1234"  |           |
| trend                          | MultiSelect | trend = "High"               |           |
| assignees                      | MultiSelect | assigned_to.id = 1           |           |
| description                    | String   | description = "test"          |           |
| reported_on                    | Date     | created >= "2021-12-01"       |           |
| last_resolved_on               | Date     | last_resolved_on < "2021-12-01"|          |
| last_reopened_on               | Date     | last_reopened_on > "2021-12-01"|          |
| days_old                       | Number   | days_old > 30                 |           |
| sla_status                     | MultiSelect | sla_violated = "true"       |           |
| patch_status                   | MultiSelect | patch_available= "true"     |           |
| cvss                           | Number   | cvss > 9.0                    |           |
| tags                           | Tags     | vulnerability_tags = "tag1"   |           |
| scan_task_id                   | String   | scan.task_id = "123"          |           |
| config_id                      | Number   | connector_config.id = 1       |           |
| cwe                            | String   | cwe.cwe_id = "CWE-89"         |           |
| ip_address                     | String   | ipaddress = "192.168.0.1"     |           |
| mac_address                    | String   | mac_address = "12:13:1E:12"   |           |
| host_name                      | String   | hostname = "wesecureapp.com"  |           |
| imported_on                    | Date     | imported_on < "2021-12-01"    |           |
| vulnerable_port                | Number   | network_bug.port.port = 80    |           |
| vulnerable_code                | String   | bug_vulnerable_code = "code snippet" |    |
| vulnerable_filename            | String   | bug_file_name = "example.txt" |           |
| vulnerable_commit              | String   | bug_commit = "commit_id"      |           |
| vulnerable_request             | String   | web_bug_request = "request details" |    |
| vulnerable_response            | String   | web_bug_response = "response details"|   |
| vulnerable_endpoints           | String   | webbug_endpoints_url = "url"  |           |
| vulnerable_cpe                 | String   | network_bug_cpe = "cpe details"|         |
| vulnerable_package_name        | String   | package_bug_name = "package"  |           |
| vulnerable_version             | String   | package_bug_installed_version = "1.0"|   |
| issue_key                      | String   | bug_tracker.issue_key = "JIRA-123" |     |
| tracker_id                     | Number   | bug_tracker_config = 1        |           |
| reported_by                    | MultiSelect | reported_by.id = 1          |           |
| imported_by                    | MultiSelect | scan_created_by = 1          |           |
| imported_using                 | MultiSelect | connector = "tool_name"      |           |
| port_number                    | Number   | port_number = 8080            |           |
| bug_type                       | MultiSelect | bug_level = "Web"           |           |
| team                           | String   | team.name = "Security Team"   |           |
| approval_status                | MultiSelect | approval_vulnerability.approval_state = "Approved" | |
| epss_score                     | Number   | epss_score > 5                |           |
| cisa_due_date                  | Date     | cisa_due_date < "2021-12-01"  |           |
| engagement                     | MultiSelect | engagements.id = 1          |           |

