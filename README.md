# ARSearch: Searching for API Related Resources from Stack Overflow and GitHub

Please follow these steps to run ARSearch in your machine. These steps have been tested on Ubuntu 20.04.4 LTS with Docker 20.10.12 installed.

## Run the web server and UI
```
cd web-search-engine
docker-compose up -d
```

Several containers will be run: facos, web-server-engine, web-server-engine-ui, and elasticsearch. Based on our test, elasticsearch container may fail to start in some machines. Below are the errors we encountered and the commands we ran (from web-search-engine directory) to fix the errors:
- Caused by: java.nio.file.AccessDeniedException: /usr/share/elasticsearch/data/nodes
```
sudo chown 1000:1000 ../data/elastic/
```
- ERROR: [1] bootstrap checks failed
[1]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
```
sudo sysctl -w vm.max_map_count=262144
```

## Run FACOS to predict Stack Overflow Resource for APIs
```
docker exec -it facos bash
cd facos/
python process_with_facos.py
```

## Import data to Elasticsearch
```
docker exec -it facos bash
cd facos/scripts
python index_so_content.py
python index_thread_data.py
```

At this point, ARSearch should be accessible at http://localhost
