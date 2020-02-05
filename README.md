GO MAP Web Server

# Build docker image for app:
docker build ./app
docker build --no-cache -t gomap ./app


# Build with docker-compose:
docker-compose build
docker-compose build --no-cache

# Run
docker run gomap
docker-compose up
