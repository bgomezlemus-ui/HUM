# HUM

Detector de texto generado por IA basado en DeBERTa-v3.

## Contenido

| Archivo | Descripción |
|:--|:--|
| `deberta_detector.py` | Clasifica un texto como generado por IA o escrito por una persona, tanto a nivel de documento como de oración. Usa el modelo `vraj33/ai-text-detector-deberta` de Hugging Face. |
| `docs/DEBERTA_EXPLAINED.md` | Explicación técnica de la arquitectura DeBERTa (en portugués). |
| `requirements.txt` | Dependencias: PyTorch, Transformers, SentencePiece y Protobuf. |

## Origen y licencia

`deberta_detector.py` y `docs/DEBERTA_EXPLAINED.md` provienen sin modificaciones del
repositorio [james1592021/AI-detector](https://github.com/james1592021/AI-detector)
(commit `ed45f41`), publicado bajo licencia MIT.

## Instalación

Se requiere Python 3.9 o superior.

```bash
python -m venv venv
source venv/bin/activate        # En Windows: venv\Scripts\activate
pip install -r requirements.txt
```

La primera ejecución descarga el modelo desde Hugging Face y lo guarda en `.model_cache/`.
Por ello, se necesita conexión a `huggingface.co` al menos una vez.

## Uso

Desde la línea de comandos:

```bash
python deberta_detector.py "Texto que se desea analizar."
```

Si no se indica texto, el programa lo solicita y termina con una línea vacía.

Desde Python:

```python
from deberta_detector import detectar_completo

resultado = detectar_completo("Texto que se desea analizar.")
print(resultado["overall"]["score_pct"], "% IA")
for s in resultado["sentencas"]:
    print(s["label"], s["score_pct"], s["sentenca"])
```

Funciones disponibles:

- `detectar_ia(texto, threshold=0.5)`: evalúa el documento completo.
- `detectar_por_sentenca(texto)`: evalúa cada oración de tres o más palabras.
- `detectar_completo(texto)`: combina ambos análisis y agrega estadísticas.

## Limitaciones

- El modelo trunca la entrada a 512 *tokens*. En textos extensos solo se evalúa el inicio
  del documento. Se recomienda analizar por párrafos o secciones.
- No se ha verificado que el modelo se haya entrenado con textos en español. Sus
  resultados sobre prosa jurídica en español deben tratarse como indicio, no como prueba
  de autoría.
- Las oraciones breves producen puntuaciones poco fiables por falta de contexto.
