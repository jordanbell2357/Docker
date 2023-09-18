# Run a container

```bash
& docker container run -t ubuntu top
```

```bash
docker container ls
```

```bash
docker container exec -it c1801777fc09 bash
```

# Run multiple containers

```bash
docker container run --detach --publish 8080:80 --name nginx nginx
```

```bash
docker container run --detach --publish 8081:27017 --name mongo mongo:3.4
```

# Remove the containers

```bash
docker container ls
```

```bash
docker container stop c7b425cf301c
docker container stop 332edffcd09b
```

```bash
docker system prune
```

# Create a Python app (without using Docker)

`app.py`

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():
    return "hello world!"

if __name__ == "__main__":
    app.run(host="0.0.0.0")
```

# Create and build the Docker image

`Dockerfile`

```bash
FROM python:3.6.1-alpine
RUN pip install --upgrade pip
RUN pip install flask
CMD ["python","app.py"]
COPY app.py /app.py
```

```bash
docker image build -t python-hello-world .
```

```
REPOSITORY                         TAG        IMAGE ID       CREATED          SIZE
python-hello-world                 latest     cae0a4c89e9d   42 seconds ago   108MB
```

```bash
docker run -p 5001:5000 -d python-hello-world
```

```
CONTAINER ID   IMAGE                COMMAND           CREATED          STATUS          PORTS                    NAMES
71efa56c9c68   python-hello-world   "python app.py"   34 seconds ago   Up 33 seconds   0.0.0.0:5001->5000/tcp   jovial_buck
```

```
 * Serving Flask app 'app' (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on all addresses.
   WARNING: This is a development server. Do not use it in a production deployment.
 * Running on http://172.17.0.2:5000/ (Press CTRL+C to quit)
172.17.0.1 - - [18/Sep/2023 17:31:33] "GET / HTTP/1.1" 200 -
172.17.0.1 - - [18/Sep/2023 17:31:33] "GET /favicon.ico HTTP/1.1" 404 -
```

# Push to a central registry

```bash
docker login
```

```bash
docker tag python-hello-world jordanbell2357/python-hello-world
docker push jordanbell2357/python-hello-world
```

# Deploy a change

```bash
docker image build -t jordanbell2357/python-hello-world .
docker push jordanbell2357/python-hello-world
```

# Create your first swarm

[Docker Playground](https://labs.play-with-docker.com/p/ck4apvufml8g00c9vph0#ck4apvuf_ck4aq16fml8g00c9vphg)

```bash
docker swarm init --advertise-addr eth0
```

```bash
docker swarm join --token SWMTKN-1-57fhbhman4s1hzu2gj9msehneecvvoh7xk5s4sdcs2tqnqepm7-b15gq19iha1o26wbaay5ozjrx 192.168.0.18:2377
```