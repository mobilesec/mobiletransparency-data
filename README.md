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
SELECT table_schema "Database", SUM(data_length + index_length) / 1024 "Size [kB]" FROM information_schema.tables WHERE table_schema="test";
```

## Publication

This repository is part of the following publication. 

[M. Lins, R. Mayrhofer, M. Roland, and A. R. Beresford: “Mobile App Distribution Transparency (MADT): Design and evaluation of a system to mitigate necessary trust in mobile app distribution systems”, in Secure IT Systems. 28th Nordic Conference, NordSec 2023, Oslo, Norway, LNCS, vol. 14324/2024, Springer, pp. 185–​203, 2023. https://doi.org/10.1007/978-3-031-47748-5_11](https://doi.org/10.1007/978-3-031-47748-5_11)

## Acknowledgment

This work has been carried out within the scope of Digidow, the Christian Doppler Laboratory for Private Digital Authentication in the Physical World and has partially been supported by the LIT Secure and Correct Systems Lab. 
We gratefully acknowledge financial support by the Austrian Federal Ministry of Labour and Economy, the National Foundation for Research, Technology and Development, the Christian Doppler Research Association, 3 Banken IT GmbH, ekey biometric systems GmbH, Kepler Universitätsklinikum GmbH, NXP Semiconductors Austria GmbH & Co KG, Österreichische Staatsdruckerei GmbH, and the State of Upper Austria.

## LICENSE

Licensed under the EUPL, Version 1.2 or – as soon they will be approved by
the European Commission - subsequent versions of the EUPL (the "Licence").
You may not use this work except in compliance with the Licence.

**License**: [European Union Public License v1.2](https://joinup.ec.europa.eu/software/page/eupl)
