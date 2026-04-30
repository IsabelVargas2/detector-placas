# 🚗 Detector de Placas Vehiculares
### Inteligencia Artificial Avanzada — UNAB Digital

Sistema completo de detección y lectura automática de placas vehiculares colombianas usando YOLOv8, FastAPI y una app móvil con Expo/React Native.

---

## 📋 Descripción

Este proyecto implementa un sistema que:
1. **Detecta** la placa de un vehículo en una imagen usando YOLOv8
2. **Lee** el texto de la placa usando EasyOCR
3. **Muestra** el resultado en una app móvil para iPhone/Android
4. **Anuncia** la placa en voz alta

---

## 🛠️ Tecnologías usadas

| Tecnología | Uso |
|-----------|-----|
| **YOLOv8** | Detección de placas en imágenes |
| **EasyOCR** | Reconocimiento de texto en la placa |
| **FastAPI** | Backend API REST |
| **AWS EC2** | Servidor en la nube |
| **Expo / React Native** | App móvil |
| **Google Colab** | Entrenamiento del modelo |
| **Roboflow** | Dataset de placas colombianas |

---

## 📁 Estructura del proyecto

```
detector-placas/
├── DetectorPlacas/          ← App móvil (Expo/React Native)
│   ├── App.tsx              ← Código principal de la app
│   ├── app.json             ← Configuración de la app
│   └── ...
├── best.pt                  ← Modelo YOLOv8 entrenado
└── README.md
```

---

## 🧠 Entrenamiento del modelo

El modelo fue entrenado en **Google Colab con GPU T4** usando:

- **Dataset:** placa-de-carro-sxy3a (Roboflow) — 836 imágenes de placas colombianas
- **Modelo base:** YOLOv8n (nano) — elegido por ser compatible con AWS
- **Épocas:** 50
- **Clase detectada:** `placa`
- **Formato:** YOLOv8

### Pasos del entrenamiento:
1. Instalar `ultralytics` y `roboflow`
2. Descargar dataset desde Roboflow
3. Entrenar con `model.train(data='data.yaml', epochs=50)`
4. Exportar `best.pt`

---

## ☁️ Backend en AWS EC2

### Configuración de la instancia:
- **Sistema operativo:** Ubuntu 24.04
- **Tipo:** t2.large
- **Almacenamiento:** 32 GiB
- **Puerto abierto:** 8080

### Dependencias instaladas:
```bash
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu
pip install ultralytics fastapi uvicorn easyocr opencv-python-headless pillow numpy python-multipart
```

### Endpoints de la API:
- `GET /` → Verifica que el servidor esté activo
- `POST /predict/` → Recibe imagen y devuelve placa detectada

### Ejecutar el servidor:
```bash
cd proyecto
source venv/bin/activate
nohup python3 app.py &
```

---

## 📱 App móvil

Desarrollada con **Expo / React Native** en TypeScript.

### Funcionalidades:
- 📷 Cámara en vivo
- 📸 Tomar foto y enviar al servidor
- 🚘 Mostrar placa detectada
- 🔊 Leer la placa en voz alta

### Instalar y correr:
```bash
cd DetectorPlacas
npx expo install expo-camera expo-image-manipulator expo-speech
npm install axios
npx expo start -c
```

Escanear el QR con **Expo Go** en el celular.

### Configuración en la app:
- **IP:** IP pública de la instancia EC2
- **Puerto:** 8080

---

## 🔄 Cómo usar cada vez

1. Entrar a **AWS Academy Learner Lab → Start Lab**
2. Ir a **EC2 → Instancias → servidor-placas**
3. Conectarse y ejecutar el servidor
4. Anotar la IP pública
5. Abrir la app con `npx expo start -c`
6. Poner la IP y puerto en la app
7. Apuntar la cámara a una placa y detectar

---

## 📚 Referencias

- [Ultralytics YOLOv8](https://docs.ultralytics.com)
- [Roboflow — Dataset placas](https://universe.roboflow.com/cicatriz/placa-de-carro-sxy3a)
- [FastAPI](https://fastapi.tiangolo.com)
- [Expo](https://expo.dev)
- [EasyOCR](https://github.com/JaidedAI/EasyOCR)
