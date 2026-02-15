# PYTHON_IA
LECTOR DE MANOS
1) CREAR CARPETA: Y ARCHIVO
pythonIA.py          ← tu programa
requirements.txt     ← dependencias

2) CREAR ENTORNO VIRTUAL (MUY IMPORTANTE)
En la carpeta del proyecto:

python -m venv ia_env
Activar:
ia_env\Scripts\activate

✅ 3️⃣ INSTALAR DEPENDENCIAS

Con el entorno activo:
pip install numpy==1.26.4
pip install opencv-python==4.8.0.76
pip install mediapipe==0.10.9
pip install cvzone==1.5.6

✅ 4️⃣ CODIGO:
import cv2
from cvzone.HandTrackingModule import HandDetector

# =============================
# CONFIGURACIÓN
# =============================

CAMARA = 1  # Cambia a 1 si tienes cámara externa
ANCHO = 1280
ALTO = 720

# =============================
# INICIALIZACIÓN
# =============================

webcam = cv2.VideoCapture(CAMARA)

if not webcam.isOpened():
    print("❌ No se pudo abrir la cámara.")
    exit()

rastreador = HandDetector(detectionCon=0.8, maxHands=2)

# =============================
# BUCLE PRINCIPAL
# =============================

while True:
    exito, imagen = webcam.read()

    if not exito:
        print("⚠ Error al capturar imagen.")
        break

    # Redimensionar
    imagen = cv2.resize(imagen, (ANCHO, ALTO))

    # Voltear horizontalmente (efecto espejo)
    imagen = cv2.flip(imagen, 1)

    # Detectar manos
    manos, imagen = rastreador.findHands(imagen)

    if manos:
        for mano in manos:
            puntos = mano["lmList"]  # Lista de 21 puntos
            tipo = mano["type"]      # Left o Right
            
            print(f"🖐 Mano detectada: {tipo}")
            print("Primer punto:", puntos[0])

    cv2.imshow("Proyecto IA - Seguimiento de Manos", imagen)

    # Salir con tecla ESC
    if cv2.waitKey(1) & 0xFF == 27:
        break

# =============================
# LIBERAR RECURSOS
# =============================

webcam.release()
cv2.destroyAllWindows()

