FROM python:3.6
CMD echo "Init running enviroment... "
WORKDIR /py
ADD . /py 
RUN pip3 install -r requirements.txt
CMD python3 app.py
