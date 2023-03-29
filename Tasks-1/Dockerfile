FROM python:3.9
RUN apt-get update
RUN apt-get install -y build-essential nginx-extras passenger 
RUN   rm -rf /var/lib/apt/lists/*
WORKDIR /app
COPY . /app
COPY requirements.txt /app
RUN pip3 install --no-cache-dir -r requirements.txt
RUN rm /etc/nginx/sites-enabled/default
COPY nginx.conf /etc/nginx/sites-enabled/webapp.conf
RUN ln -sf /dev/stdout /var/log/nginx/access.log && \
    ln -sf /dev/stderr /var/log/nginx/error.log

EXPOSE 5000
CMD [ "nginx", "-g", "deamonoff"]
ENTRYPOINT [ "python3", "./app.py"]
