# MongoDB useful commands
## Import data from .json file to MongoDB
```mongoimport --db <database_name> --collection <table_name> --file <file_name.json>```
- also supply ```--jsonArray``` flag at the end of command if you are importing multiple json objects (=> array)