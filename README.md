# Introduction
This repository contains relevant scripts and configuration files to conduct performance measurements on our transparency log system. 
The directory "data" contains reference snapshots of collected and analyzed data. 

ATTENTION: Do not check-in the buildserver password in the file "api_credentials.json".

# Setup:

The following steps are relevant to prepare the expirement:

1. Clone git repository [1]
2. [optional] Open ssh tunnel to personality address
3. [optional] Test connection by open https://localhost:8071/swagger/index.html in your browser
4. Check available trees on that instance: swagger --> /Log/ListTrees
5. Request access token of user "admin" ;
    1. Get admin credentials from salt configuration
    2. Open swagger and go to /Login/RequestAccessToken
    3. Fill in { "username": "admin", "password": "<INSERT ADMIN PW>" } and execute request
    4. Copy authentication token from response (only the value of the key "token")!!
    5. Use "Authorize" button at top of the swagger page and fill in "Bearer: <INSERT TOKEN>"
6. Create new tree for experiment: swagger  --> /Admin/CreateTree
7. Execute /Log/ListTrees again to get real treeId (ATTENTION: CreateTree does not provide full treeID)
8. Set treeID in fdroid-log.py file 
9. Get buildserver credentials from salt configuration and fill in related field in api_credentials.json 
10. Run fdroid-log.py

# Measurements

The time-related measurements are done by the script itself and can be viewed via the console.
The increased database size can be conducted by running the following sql query before and after the script has been executed:

```sql
SELECT table_schema "Database", ROUND(SUM(data_length + index_length) / 1024 / 1024,2) "Size [MB]" FROM information_schema.tables WHERE table_schema="test";
```
