# AREP - Taller LLM
## Autor: David Leonardo Piñeros Cortés

El objetivo de este taller es construir una aplicación web simple para la traducción de texto usando
LangChain y un modelo de lenguaje LLM.

### Arquitectura

La arquitectura de la aplicación se resumen en un cliente que interactúa con el servidor a través de solicitudes HTTP, ya sea directamente o mediante la UI proporcionada por LangServe. FastAPI gestiona las solicitudes y las pasa a LangChain, que está compuesto por el modelo de lenguaje ChatOpenAI y el StrOutputParser para procesar las respuestas.
LangServe expone la API REST y se ejecuta mediante Uvicorn, que maneja las solicitudes y coordina la ejecución del servidor. Finalmente RemoteRunnable permite que el cliente interactúe con el servidor de manera programática.

```mermaid

graph TD
    A[Cliente] -->|Envía solicitud HTTP| B[FastAPI]
    B -->|Pasa solicitud| C[LangChain]
    C -->|Invoca| D[ChatOpenAI Modelo de Lenguaje]
    D -->|Respuesta Texto| E[StrOutputParser]
    E -->|Texto procesado| B
    B -->|Devuelve respuesta| A
    B -->|Ruta /chain| F[LangServe]
    F -->|Despliega API REST| G[Uvicorn]
    G -->|Ejecuta FastAPI| B
    A -->|Interacción UI| F
    A -->|Interacción programática| H[RemoteRunnable]
    H -->|Invoca API REST| F

    classDef fastAPI fill:#F4A300,stroke:#333,stroke-width:2px;
    class B fastAPI;
    
    classDef langChain fill:#6F9B6A,stroke:#333,stroke-width:2px;
    class C langChain;
    
    classDef model fill:#1E77B9,stroke:#333,stroke-width:2px;
    class D model;
    
    classDef parser fill:#FFC107,stroke:#333,stroke-width:2px;
    class E parser;
    
    classDef langServe fill:#7F8C8D,stroke:#333,stroke-width:2px;
    class F langServe;
    
    classDef uvicorn fill:#16A085,stroke:#333,stroke-width:2px;
    class G uvicorn;
    
    classDef remoteRunnable fill:#8E44AD,stroke:#333,stroke-width:2px;
    class H remoteRunnable;

```

### Instalación y Ejecución
Para ejecutar la aplicación es necesario instalar Python 3.12.7 o versiones similares y git. El primer paso es clonar el repositorio e ingresar a la carpeta resultante

```
git clone https://github.com/leoncico/AREP-LAB08-PART1.git
````

A continuación debe editar el archivo langchainserver.py en la linea 9 para ingresar la clave API de chatgpt (se indica cual es la clave en comentario de entrega).

```
os.environ["OPENAI_API_KEY"] = "(clave api)"
```

Luego ingrese a la terminal y escriba los siguientes comandos (para windows):

```
python -m venv .venv
```

```
.venv\Scripts\activate
```

```
pip install -r requirements.txt
```

Una vez terminada la instalación de las dependencias puede ejecutar el servidor con:

```
python langchainserver.py
```

Finalmente pruebe el servidor ingresando a http://localhost:8000/chain/playground/ 

![](img/01.PNG)

En el primer campo digite al idioma que quiere traducir, en el segundo campo el texto que desea traducir y finalmente pulse el botón Start y obtendrá el texto traducido.