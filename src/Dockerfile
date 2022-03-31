FROM python:bullseye
WORKDIR /app
#copiar ficheiro requirements para root
COPY requirements.txt .
#gerenciador pacotes python
RUN python -m pip install -r requirements.txt
#copiar todos os arquivos para pasta app
COPY . /app
EXPOSE 5000
#gunicorn é um servidor web para applicações python. acesso por qualquer IP
CMD [ "gunicorn" , "--workers=2", "--bind", "0.0.0.0:5000", "-c", "config.py", "app:app"]