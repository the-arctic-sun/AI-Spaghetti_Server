# AI-Spaghetti_Server

1. clone the repo in your directory:
```
    git clone https://github.com/tanmoy-sabir/AI-Spaghetti_Server
```

2. Copy the MAR file in the directory:
```
    cd AI-Spaghetti_Server/AI_Model/
    wget -c http://~~~~//~~.mar -O solutions_model.mar
    cd ..
```

3. build the dockerfile using following command:
```
sudo docker build --file Dockerfile -t torchserve_ai:server_ai .
```


4. run the built image using following command:
```
sudo docker run --rm -it --gpus all -p 8080:8080 -p 8081:8081 -p 8082:8082 -p 7070:7070 -p 7071:7071 torchserve_ai:server_ai
```
