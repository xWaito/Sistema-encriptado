<h1>
  encriptado
</h1>
-estado en contruccion 
# [ !NOTES ] Se utiliza una imagen base de Python optimizada para aplicaciones livianas.
# La imagen `python:3.9-slim` es ligera y adecuada para entornos donde se requiere optimizar el tamaño del contenedor.
FROM python:3.9-slim

# [ !NOTES ] Establece el directorio de trabajo dentro del contenedor.
# Este será el lugar donde se ejecutarán todos los comandos y scripts subsecuentes.
WORKDIR /backend

# [ !TIP ] Copia todo el contenido del proyecto al directorio de trabajo dentro del contenedor.
# Esto asegura que todos los archivos necesarios estén disponibles en el contenedor.
COPY ./ /backend/

# [ !IMPORTANT ] Copia la carpeta específica de la aplicación en un directorio separado.
# Esto es útil para mantener una estructura de directorios organizada y compatible con sistemas de CI/CD.
COPY ./app /app/

# [ !NOTES ] Copia el archivo de dependencias para asegurar que estén disponibles durante la instalación.
# Esto permite instalar solo las librerías requeridas para el backend.
COPY requirements.txt /backend/requirements.txt

# [ !WARNING ] La instalación de dependencias puede fallar si hay problemas de compatibilidad.
# Asegúrate de especificar versiones exactas en el archivo `requirements.txt` para evitar conflictos.
RUN pip install --no-cache-dir -r /backend/requirements.txt

# [ !NOTES ] Expone el puerto 8000 para que el servicio sea accesible desde el host.
# Este puerto es donde se ejecutará la aplicación.
EXPOSE 8000

# [ CAUTION ] Usa el comando siguiente para entornos de desarrollo.
# El parámetro `--reload` permite recargar automáticamente la aplicación al detectar cambios, pero no es adecuado para producción.
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000", "--reload"]
