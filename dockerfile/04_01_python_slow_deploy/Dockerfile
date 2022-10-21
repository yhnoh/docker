FROM ubuntu:20.04
COPY app.py /app
RUN apt-get update
RUN apt-get -y install python python-pip
RUN pip install -r requirement.txt
CMD ["python", "/app/app.py"]