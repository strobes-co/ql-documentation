
# ql-documentation
A documentation on how to use Strobes QL with GraphQL or API client 

### What is Strobes QL? 

Strobes QL is a advanced query language built to search and filter data sets within Strobes at scale. 

### Example

You want to filter all vulnerabilities with asset hostname containing wesecureapp.com, have an exploit available and are resolved for over 30 days else get all vulnerabilities with title containing SSL
```
(hostname ~ "wesecureapp.com" and exploit_available = "true" and created > "2021-12-01") or (title ~ "SSL")
```
### Supported Operators 

| Operator | Description | Example |
|--|--|--|
| = | Equal to operation | title = "example.com" |
| != | Not Equal to operation | exposure != 1 |
| < | Less than operation | created < "2021-12-01" |
| <= | Less than equal to operation | created <= "2021-12-01" |
| > | Greater than operation | created > "2021-12-01" |
| >= | Greater than equal to operation | created >= "2021-12-01" |
| ~ | contains operation | os ~ "linux" |
| !~ | Doesn't contain | os !~ "windows" |
| in | in list operation | os in ("windows2021", "linux2021") |
| not in | not in list operation | port not in (80, 8080) |
| ^ | regex operation | hostname ^  "(.*)" |


### Conditions and Grouping

Two conditions are supported within QL, i.e 
- and
-  or 

You can also group and execute queries to filter or drill down the results deeper.  Make sure the queries are not heavily nested, the API might time out in the process.

### Fields Supported 

#### Assets
The fields are support when you're calling allAssets GQL call or /assets/ api call.
| Field  | Type | Example | Reference
|--|--|--|--|
| id | IntType | id = 1 |
| name | StrType| name = "abc" |
| type | IntType | type in (1, 2) | Refer for asset types: [strobes-co/strobes-api-client (github.com)](https://github.com/strobes-co/strobes-api-client#list_vulnerabilities)
| target| StrType | target = "192.168.0.1" | 
| ipaddress | StrType | ipaddress = "192.168.0.1" | 
| mac_address | StrType | mac_address= "00:14:12:12" | 
| hostname | StrType | hostname= "strobes.co" | 
| os | StrType | os ~ "linux" | 
| sensitivity | IntType | sensitivity in (1, 2) | Refer for sensitivity types: [strobes-co/strobes-api-client (github.com)](https://github.com/strobes-co/strobes-api-client#list_vulnerabilities)
| exposure | IntType | exposure  in (1, 2) | Refer for exposure types: [strobes-co/strobes-api-client (github.com)](https://github.com/strobes-co/strobes-api-client#list_vulnerabilities)
| tags | StrType | tags in ("not-fixed", "fixed") | 
| exploit_available| BoolType | exploit_available = "true" | 
| patch_available | BoolType | patch_available= "true" | 
| created_by.id | IntType | created_by.id = 1 | 
| created| StrType | created > "2021-12-01" | 
| asset_port.port | IntType  | asset_port.port not in (80, 8080) | 
| asset_port.state | IntType   | asset_port.state not in (1, 2, 3) | Refer for Port types: [strobes-co/strobes-api-client (github.com)](https://github.com/strobes-co/strobes-api-client#list_vulnerabilities)
| asset_port.product | StrType | asset_port.product = "ssh" | 
| asset_port.service | StrType | asset_port.service = "ssh" | 

#### Vulnerabilities
