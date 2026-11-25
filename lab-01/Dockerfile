FROM python:3.12.16-slim AS builder

COPY requirements.txt ./

RUN pip install --no-cache-dir --prefix=/install -r requirements.txt

FROM python:3.12.16-slim
COPY --from=builder /install /usr/local

WORKDIR /app
RUN useradd app && chown app:app ./ && mkdir -p /data && chown app:app /data
USER app
COPY --chown=app:app app.py ./

HEALTHCHECK --interval=10s --timeout=3s --start-period=5s \
  CMD python -c "import urllib.request,sys; sys.exit(0 if urllib.request.urlopen('http://127.0.0.1:8080/health',timeout=2).status==200 else 1)"
CMD [ "python", "app.py" ]
EXPOSE 8080