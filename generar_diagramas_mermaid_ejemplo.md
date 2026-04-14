# Documentación — SamJoko Lector Básico

Proyecto de síntesis de voz en español con **Piper TTS** y reproducción local con **simpleaudio**.
El flujo principal crea un lector, sintetiza un WAV a partir de texto y luego lo reproduce.

---

## 1. Diagrama de Clases

```mermaid
classDiagram
    class LectorPiper {
        +voice: PiperVoice
        +__init__(modelo_path: str)
        +hablar(texto: str, salida: str = "salida.wav") str
        +reproducir(archivo: str)
    }

    class PiperVoice {
        +load(modelo_path: str)$ PiperVoice
        +synthesize_wav(texto: str, wav_file)
    }

    class Path {
        +exists() bool
        +with_suffix(suffix: str) Path
    }

    class WaveObject {
        +from_wave_file(path: str)$ WaveObject
        +play() PlayObject
    }

    class PlayObject {
        +wait_done()
    }

    LectorPiper --> PiperVoice : usa/carga
    LectorPiper --> Path : usa para validar rutas
    LectorPiper --> WaveObject : usa para reproducir
    WaveObject --> PlayObject : retorna
```

---

## 2. Diagrama de Flujo de Ejecución

```mermaid
flowchart TD
    A([Inicio main]) --> B[samjoko.main]
    B --> C[Definir ruta modelo onnx]
    C --> D[Crear LectorPiper]

    D --> E{modelo onnx existe}
    E -- No --> E1[Error FileNotFound modelo]
    E -- Si --> F{json del modelo existe}
    F -- No --> F1[Error FileNotFound json]
    F -- Si --> G[PiperVoice.load]

    G --> H[LectorPiper.hablar]
    H --> I{texto vacio}
    I -- Si --> I1[Error ValueError texto vacio]
    I -- No --> J[wave.open]
    J --> K[synthesize_wav]
    K --> L[Retorna ejemplo.wav]

    L --> M[print]
    M --> N[LectorPiper.reproducir]
    N --> O{archivo wav existe}
    O -- No --> O1[Error FileNotFound audio]
    O -- Si --> P[WaveObject.from_wave_file]
    P --> Q[play]
    Q --> R[wait_done]
    R --> S([Fin])
```

---

## 3. Diagrama de Secuencia

```mermaid
sequenceDiagram
    actor Usuario
    participant MAIN as samjoko.py main()
    participant LP as LectorPiper
    participant FS as Sistema de Archivos
    participant PV as piper.PiperVoice
    participant SA as simpleaudio

    Usuario->>MAIN: Ejecuta script
    MAIN->>LP: __init__(ruta_modelo)
    LP->>FS: Verifica .onnx
    FS-->>LP: Existe / no existe
    LP->>FS: Verifica .onnx.json
    FS-->>LP: Existe / no existe
    LP->>PV: load(modelo_path)
    PV-->>LP: voice cargada
    MAIN->>LP: hablar(texto, ejemplo.wav)
    LP->>PV: synthesize_wav(texto, wav_file)
    PV-->>LP: WAV generado
    LP-->>MAIN: "ejemplo.wav"
    MAIN->>LP: reproducir(ejemplo.wav)
    LP->>FS: Verifica archivo WAV
    FS-->>LP: Existe / no existe
    LP->>SA: WaveObject.from_wave_file()
    SA-->>LP: wave_obj
    LP->>SA: wave_obj.play() + wait_done()
    SA-->>Usuario: Audio reproducido
```

---

## 4. Mapa de Dependencias entre Módulos

```mermaid
graph LR
    subgraph Proyecto
        A[samjoko.py]
        B[samjoko_lector.py]
    end

    subgraph Librerías externas
        C[piper.PiperVoice]
        D[simpleaudio.WaveObject]
        E[simpleaudio.PlayObject]
    end

    subgraph Python Stdlib
        F[pathlib.Path]
        G[wave]
    end

    subgraph Recursos
        H[voces/*.onnx]
        I[voces/*.onnx.json]
        J[ejemplo.wav]
    end

    A -- importa --> B
    B -- importa --> C
    B -- importa --> D
    D -- retorna --> E
    B -- importa --> F
    B -- importa --> G
    C -- carga --> H
    C -- usa metadata --> I
    G -- escribe --> J
    D -- lee --> J
```

---

## 5. Diagrama de Relaciones entre Funciones

```mermaid
flowchart TD
    subgraph SG1[Entrada]
        F1["samjoko.main"]
    end

    subgraph SG2[Dominio]
        F2["LectorPiper.init"]
        F3["LectorPiper.hablar"]
        F4["LectorPiper.reproducir"]
    end

    subgraph SG3[Externas]
        F5["Path.exists"]
        F6["Path.with_suffix"]
        F7["PiperVoice.load"]
        F8["wave.open"]
        F9["PiperVoice.synthesize_wav"]
        F10["WaveObject.from_wave_file"]
        F11["WaveObject.play"]
        F12["PlayObject.wait_done"]
        F13["print"]
    end

    F1 --> F2
    F1 --> F3
    F1 --> F13
    F1 --> F4

    F2 --> F5
    F2 --> F6
    F2 --> F5
    F2 --> F7

    F3 --> F8
    F3 --> F9

    F4 --> F5
    F4 --> F10
    F4 --> F11
    F4 --> F12
```

---

## 6. Resumen de responsabilidades

| Módulo / Clase | Responsabilidad |
|---|---|
| `samjoko.py` (`main`) | Punto de entrada: define modelo, orquesta síntesis y reproducción |
| `LectorPiper.__init__` | Valida archivos del modelo (`.onnx` y `.json`) y carga la voz |
| `LectorPiper.hablar` | Genera audio WAV desde texto usando `PiperVoice.synthesize_wav` |
| `LectorPiper.reproducir` | Reproduce WAV con `simpleaudio` y espera fin de reproducción |
| `piper.PiperVoice` | Librería TTS externa para carga de modelo y síntesis |
| `simpleaudio.WaveObject/PlayObject` | Librería externa para reproducción de audio |
| `pathlib.Path` / `wave` | Soporte de rutas y escritura de contenedor WAV |
