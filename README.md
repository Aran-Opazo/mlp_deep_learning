## Requisitos previos

Este notebook detecta automáticamente si se ejecuta en Google Colab o en un entorno local (VS Code, Jupyter), y ajusta la forma de obtener las credenciales de Kaggle según corresponda.

### Si se ejecuta en Google Colab

1. Crear cuenta en kaggle.com (si no tiene).
2. Ir a Settings → API → "Create New Token" → "Copiar API Token".
3. En Colab, ir al ícono de llave 🔑 (Secretos) en el panel izquierdo.
4. Agregar un nuevo secreto con nombre `KAGGLE_TOKEN` y como valor el token copiado.
5. Activar el acceso al notebook para ese secreto.
6. Ejecutar el notebook normalmente — la celda de descarga funcionará sola.

### Si se ejecuta localmente (VS Code / Jupyter)

1. Crear cuenta en kaggle.com (si no tiene).
2. Ir a Settings → API → "Create New Token" → "Copiar API Token".
3. En una terminal, ejecutar:
      mkdir -p ~/.kaggle
      echo "SU_TOKEN_AQUI" > ~/.kaggle/access_token
      chmod 600 ~/.kaggle/access_token
4. Instalar las dependencias del proyecto:
      pip install -r requirements.txt
5. Ejecutar el notebook normalmente — la celda de descarga funcionará sola.
