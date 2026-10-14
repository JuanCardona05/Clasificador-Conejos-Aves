# Clasificador de conejos y aves

Aplicación web que utiliza un modelo de aprendizaje profundo para clasificar
imágenes como **ave** o **conejo**. El modelo funciona directamente en el
navegador y permite analizar imágenes del equipo o capturas de la cámara.

> Proyecto académico realizado por **Juan Gabriel Cardona Ballesteros**.

## Índice

- [Descripción](#descripción)
- [Cómo funciona](#cómo-funciona)
- [Inicio rápido](#inicio-rápido)
- [Uso de la aplicación](#uso-de-la-aplicación)
- [Dataset](#dataset)
- [Estructura del proyecto](#estructura-del-proyecto)
- [Tecnologías](#tecnologías)
- [Limitaciones](#limitaciones)
- [Mejoras futuras](#mejoras-futuras)
- [Créditos](#créditos)

## Descripción

El objetivo del proyecto es aplicar un modelo de clasificación de imágenes a
un caso concreto: diferenciar conejos y aves. La aplicación tiene una
interfaz sencilla para que el usuario pueda:

- Seleccionar una imagen desde su dispositivo.
- Arrastrar una imagen al área de carga.
- Utilizar la cámara del dispositivo.
- Consultar la predicción y el porcentaje estimado para cada clase.

Todo el procesamiento se realiza en el navegador. Las imágenes no se envían a
un backend ni a un servicio externo.

## Cómo funciona

El flujo de predicción es el siguiente:

```text
Imagen o cámara
       │
       ▼
Previsualización en el canvas
       │
       ▼
Redimensionamiento a 100 x 100 píxeles
       │
       ▼
Conversión a escala de grises y normalización
       │
       ▼
Predicción con TensorFlow.js
       │
       ▼
Resultado: Ave o Conejo
```

En cada predicción, `index.html` realiza estas operaciones:

1. Carga el modelo desde `model.json`.
2. Redimensiona la imagen a `100 x 100` píxeles.
3. Convierte cada píxel a escala de grises.
4. Normaliza los valores de los píxeles entre `0` y `1`.
5. Construye un tensor con la forma esperada por el modelo.
6. Obtiene la salida del modelo y la convierte en porcentajes.
7. Muestra la clase con mayor probabilidad y actualiza las barras visuales.

La aplicación utiliza el siguiente criterio para interpretar la salida:

| Valor obtenido por el modelo | Resultado mostrado |
| --- | --- |
| Menor o igual a `0.5` | Ave |
| Mayor a `0.5` | Conejo |

Cuando se utiliza la cámara, el proceso se repite periódicamente sobre los
fotogramas capturados. El botón **Cambiar cámara** permite alternar entre la
cámara frontal y la trasera cuando el dispositivo lo permite.

## Inicio rápido

### Requisitos

Antes de ejecutar el proyecto se necesita:

- Un navegador actualizado con soporte para JavaScript.
- Un servidor HTTP local.
- Conexión a internet para cargar TensorFlow.js y Bootstrap desde sus CDN.
- Permiso para utilizar la cámara, únicamente si se usará esa función.

No es necesario instalar TensorFlow, Node.js ni otro framework. Python se
utiliza solamente como una opción sencilla para iniciar el servidor local.

### Instalación y ejecución

#### 1. Obtener el proyecto

Clona el repositorio o descarga sus archivos. Si utilizas Git:

```bash
git clone <URL_DEL_REPOSITORIO>
cd <CARPETA_DEL_PROYECTO>
```

#### 2. Iniciar un servidor local desde CMD

Para abrir el host local en el puerto `8000`, primero ingresamos al **CMD de
Windows** y nos ubicamos en la carpeta donde se encuentra el proyecto de
Deep Learning utilizando el comando `cd`. Por ejemplo:

```cmd
cd "C:\ruta\de\la\carpeta\deep learning"
```

Después, ejecutamos el siguiente comando para iniciar un servidor web local en
el puerto `8000`:

```bash
python -m http.server 8000
```

En Windows también se puede utilizar `py`:

```cmd
py -m http.server 8000
```

Una vez iniciado correctamente el servidor, abrimos el navegador y escribimos
[http://localhost:8000](http://localhost:8000). De esta manera podemos acceder
a los archivos del proyecto y visualizar la página web desde nuestro
computador.

Este procedimiento permite probar el proyecto localmente sin necesidad de
publicarlo en un servidor externo.

> En el primer intento, el servidor no se inició porque Windows no encontró
> Python instalado o configurado correctamente. Si aparece un mensaje similar,
> es necesario instalar Python y habilitar la opción **Add Python to PATH**
> durante la instalación. Como alternativa, se puede comprobar la instalación
> utilizando `py --version` y ejecutar el servidor con `py -m http.server 8000`.

#### 3. Abrir la aplicación

Una vez disponible el host local, visita
[http://localhost:8000](http://localhost:8000) en el navegador y espera a que
el estado cambie a **“Modelo listo”**.

> No se recomienda abrir `index.html` directamente con doble clic. Los
> navegadores pueden bloquear la carga de `model.json` y de los pesos por las
> restricciones de seguridad de los archivos locales.

## Uso de la aplicación

### Clasificar una imagen

1. Inicia el servidor local.
2. Abre [http://localhost:8000](http://localhost:8000).
3. Espera a que termine de cargar el modelo.
4. Haz clic en el área de selección y elige una imagen, o arrástrala hasta esa
   área.
5. Revisa la predicción y los porcentajes de **Ave** y **Conejo**.

La aplicación acepta imágenes del dispositivo, como JPG, PNG, WEBP y otros
formatos compatibles con el navegador.

### Clasificar con la cámara

1. Abre la aplicación desde un servidor local.
2. Pulsa **Usar cámara**.
3. Acepta el permiso solicitado por el navegador.
4. Observa la predicción mientras la cámara captura imágenes.
5. Utiliza **Cambiar cámara** para alternar entre las cámaras disponibles.

La cámara puede requerir un contexto seguro según el navegador y el
dispositivo. `localhost` suele estar permitido para este propósito.

## Dataset

El modelo se entrenó utilizando como referencia el dataset de animales
disponible en el repositorio
[pabllopf/Dataset-Animals](https://github.com/pabllopf/Dataset-Animals).
Según la descripción del repositorio, contiene seis tipos de animales, con
imágenes en formato JPG y fondo blanco. Para este proyecto se utilizaron las
categorías de aves y conejos.

No se encontró un dataset amplio y específico de conejos y aves que se
ajustara al objetivo del proyecto. Por eso se utilizó el dataset disponible y
se seleccionaron las categorías necesarias para el clasificador.

También se consideró utilizar el conocido dataset de gatos y perros. Sin
embargo, no habría sido una elección coherente: el objetivo era que el modelo
aprendiera a distinguir conejos y aves, no reutilizar un problema diferente.
Usar gatos y perros habría significado seguir un ejemplo común sin aplicar lo
aprendido al problema planteado.

## Estructura del proyecto

```text
.
├── index.html              # Interfaz, estilos y lógica de predicción
├── model.json              # Arquitectura del modelo TensorFlow.js
└── group1-shard1of1.bin    # Pesos entrenados del modelo
```

Los archivos `model.json` y `group1-shard1of1.bin` son un conjunto y deben
permanecer en la misma carpeta. El archivo JSON contiene la arquitectura del
modelo y la referencia al archivo de pesos. Si alguno falta o se cambia de
ubicación, el modelo no podrá cargarse.

## Tecnologías

| Tecnología | Uso |
| --- | --- |
| HTML, CSS y JavaScript | Interfaz y lógica de la aplicación |
| [TensorFlow.js 4.22.0](https://www.tensorflow.org/js) | Carga y ejecución del modelo |
| [Bootstrap 5.0.2](https://getbootstrap.com/) | Recursos de interfaz |
| TensorFlow.js Layers Model | Formato del modelo exportado |

## Limitaciones

Este es un proyecto académico y experimental. La calidad de las predicciones
depende de la cantidad, variedad y calidad de las imágenes utilizadas durante
el entrenamiento. Por lo tanto:

- El modelo puede equivocarse con imágenes distintas a las del dataset.
- El fondo blanco del dataset puede hacer que el modelo tenga dificultades
  con fondos complejos.
- La predicción no debe interpretarse como una identificación definitiva.
- El modelo solo contempla las clases **Ave** y **Conejo**.
- Una imagen de otro animal igualmente puede ser forzada a una de esas dos
  categorías.

## Mejoras futuras

- Ampliar el dataset con imágenes más variadas y mejor balanceadas.
- Evaluar el modelo con conjuntos separados de entrenamiento, validación y
  prueba.
- Añadir una clase para indicar que la imagen no corresponde a un ave ni a un
  conejo.
- Mejorar el manejo de imágenes con diferentes proporciones e iluminación.
- Mostrar un aviso cuando la confianza de la predicción sea baja.
- Servir las dependencias localmente para permitir el uso sin conexión.

## Créditos

- **Autor:** Juan Gabriel Cardona Ballesteros
- **Dataset de referencia:** [pabllopf/Dataset-Animals](https://github.com/pabllopf/Dataset-Animals)
- **Licencia del dataset:** consultar la licencia indicada en el repositorio
  original.
