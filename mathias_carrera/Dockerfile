FROM python:3.12-slim

# 2. Instalación de uv con limpieza de caché de apt
# los comandos con && para que todo ocurra en una sola capa
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# 3. Instalador de uv 
ADD https://astral.sh/uv/install.sh /uv-installer.sh
RUN sh /uv-installer.sh && rm /uv-installer.sh

# 4. Configurar el entorno
ENV PATH="/root/.local/bin/:$PATH"
ENV UV_PROJECT_ENVIRONMENT="/usr/local"

# 5. Directorio de trabajo
WORKDIR /app

# 6. Optimización de Caché: Copiamos SOLO el archivo de requisitos primero
# si cambia el código de app.py, Docker no reinstala las librerías
COPY src/requirements.txt .

# 7. Instalación de librerías con uv
RUN if [ -s requirements.txt ]; then \
        uv pip install --system -r requirements.txt; \
    else \
        uv pip install --system flask; \
    fi

# 8. Copiar el código fuente al final
COPY src/ .

# 9. Puerto y ejecución
EXPOSE 5001

CMD ["python", "app.py"]