#Build docker image for app:
docker build ./app
docker build --no-cache -t gomap ./app


#Run
docker run gomap