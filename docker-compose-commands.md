docker-compose up -d # run all services
docker-compose stop nginx # stop only one. but it still running !!!
docker-compose build --no-cache nginx 
docker-compose up -d --no-deps # link nginx to other services

docker-compose up -d --force-recreate --build

Options:
  -d                  Detached mode: Run containers in the background,
                      print new container names. Incompatible with
                      --abort-on-container-exit.
  --force-recreate    Recreate containers even if their configuration
                      and image haven't changed.
  --build             Build images before starting containers.


