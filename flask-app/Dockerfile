FROM python:3.9

# Carpeta "home" de la imagen
WORKDIR /usr/src/app

# Instalar dependencias con pip
COPY requirements.txt requirements.txt

RUN pip install --no-cache-dir -r requirements.txt

# Copiar el código fuente
COPY src .

CMD [ "python", "app.py" ]