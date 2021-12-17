# Criar a rede para agendamento
docker network create dletra-net-schedule

# Para compilar o Dockerfile, acessar cada pasta e executar
docker build -t dletra/schedule:src laravel/ -f laravel/Dockerfile
docker build -t dletra/schedule:nginx nginx/ -f nginx/Dockerfile
docker build -t dletra/schedule:db db/ -f db/Dockerfile

# Para executar o docker Laravel
docker run -d --network dletra_net-schedule -v $(pwd)/laravel/src:/var/www/dletra-schedule --name dletra-schedule-src dletra/schedule:src
docker run -d --network dletra_net-schedule --name dletra-schedule-nginx -p 8081:80 dletra/schedule:nginx
docker run -d --network dletra_net-schedule -v $(pwd)/db/data:/var/lib/mysql --name dletra-schedule-db -p 3381:3306 -e MYSQL_ROOT_PASSWORD=my-secret-pw dletra/schedule:db