FROM python:3.9.2-alpine
RUN apt-get update && apt-get -y install python python-pip
RUN pip install -r requirement.txt
COPY app.py /app
CMD ["python", "/app/app.py"]