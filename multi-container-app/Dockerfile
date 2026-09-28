FROM python:3.8-slim

WORKDIR /app

RUN pip install flask redis

COPY . .

EXPOSE 5002

CMD ["python","count.py"]