FROM nginx:latest
RUN useradd -r -u 1001 appuser && \
    mkdir /app && \
    chown appuser /app
COPY ./nginx.conf /etc/nginx/nginx.conf
EXPOSE 8000/tcp
