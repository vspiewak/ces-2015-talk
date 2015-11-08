docker build -t=vspiewak/nginx .
docker run -d -p 80:80 vspiewak/nginx
docker exec -it <container_id> bash

docker login
docker push vspiewak/nginx
