# local-gloo

First export the Gloo Edge Key as env var:

```bash
export KEY_HERE=<GLOO KEY HERE>
```
Then start Gloo Edge:
```bash
docker-compose up
```

Now you can call the gateway: 
```
curl -v 127.0.0.1:8080/api/pets -v
```
