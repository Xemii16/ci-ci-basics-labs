FROM python:3.12-slim

COPY requirements.txt ./

RUN pip install --no-cache-dir -r requirements.txt
COPY ./ ./app

RUN useradd app && mkdir -p ./data && chown app:app /data
USER app

HEALTHCHECK --interval=10s --timeout=3s --start-period=5s \
  CMD python -c "import urllib.request,sys; sys.exit(0 if urllib.request.urlopen('http://127.0.0.1:8080/health',timeout=2).status==200 else 1)"
CMD [ "python", "app/app.py" ]