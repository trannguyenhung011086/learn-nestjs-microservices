## Run with docker-compose

- `sudo docker-compose up --build`

## Create local registry Docker

- `sudo docker run -d -p 5000:5000 --restart=always --name registry registry:2`
- `sudo docker tag learn-nestjs_gateway:latest localhost:5000/learn-nestjs_gateway` (do the same for other images)
- `sudo docker push localhost:5000/learn-nestjs_gateway` (do the same for other images)

## Conver docker-compose.yml to kubernetes yaml

- `curl -L https://github.com/kubernetes/kompose/releases/download/v1.22.0/kompose-linux-amd64 -o kompose`
- `chmod +x kompose`
- `sudo mv ./kompose /usr/local/bin/kompose`
- `kompose convert`
- set `imagePullPolicy: IfNotPresent` to \*.deployment.yaml to use local Docker registry

## Deploy kubernetes locally

- Apply deployment `kubectl apply -f gateway-deployment.yaml` (do the same for other deployment yaml files)
- Expose gateway endpoint `kubectl expose deployment gateway --type=LoadBalancer --port=3000 --name=my-gateway`

## Notes

- Check status `kubectl get all`
- Get log from running service `kubectl logs gateway-77cb68b6fb-vfbrr`
- Delete resources `kubectl delete all --all`

## Flow

- Make requests to Gateway service > send event to Order service > send event to Payment service > send update event to Order service
- Order service has cron job running every 30 seconds to update db
