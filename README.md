# 📚 Wooclap Quiz - ARA

Aplicación web interactiva para practicar con las preguntas de los Wooclaps de la asignatura ARA.


## 🚀 Cómo usar

### 1. Iniciar el servidor local

Desde la carpeta del proyecto, ejecuta:

```bash
python3 -m http.server 8000
```

### 2. Abrir la aplicación

Abre tu navegador y accede a:

```
http://localhost:8000/index.html
```

### 3. Realizar el quiz

1. **Selecciona un tema** específico o el **modo aleatorio**
2. **Responde las preguntas** seleccionando una opción
3. **Navega** con el botón "Siguiente"
4. **Revisa tus resultados** al finalizar, incluyendo las preguntas que fallaste

## 📂 Estructura del proyecto

```
wooclaps/
├── index.html              # Aplicación web
├── questions_data.json     # Base de datos de preguntas (generado automáticamente)
├── wooclap-preguntas/      # Excel del Bloque I (IPv6)
├── wooclaps-bloqueII/      # Excel del Bloque II (SDN/NFV/CDN)
└── README.md               # Este archivo
```

## 📊 Temas disponibles

### Bloque I - IPv6
- Autoevaluacion-IPv6-semana2 (8 preguntas)
- Autoevaluacion-IPv6-semana_3-ND-autoconf (6 preguntas)
- Autoevaluacion-IPv6-MIPv6-semana4 (10 preguntas)
- Autoevaluacion-semana_5-MIPv6 (3 preguntas)

### Bloque II - SDN/NFV/CDN
- Autoevaluacion-BloqueII-tema1-parte1-semana6 (5 preguntas)
- Autoevaluacion-SDN-2024-2025 (7 preguntas)
- Autoevaluacion-SDN-Openflow_2024-2025 (7 preguntas)
- Autoevaluacion-bloqueII-tema1-implementacion-SLB-2024-2025 (4 preguntas)
- Autoevaluacion-CDNs-2024-2025 (3 preguntas)
- Autoevaluacion-bloqueII-persistencia-2024-2025 (3 preguntas)
- Autoevaluacion-balanceo-carga-global-2024-2025 (1 pregunta)
- Autoevaluacion-NFV-2024-2025 (9 preguntas)

## 🔄 Actualizar preguntas

Si añades o modificas archivos Excel en las carpetas `wooclap-preguntas/` o `wooclaps-bloqueII/`, ejecuta:

```bash
python3 << 'EOF'
import openpyxl
import json
import os

def extract_questions_from_excel(filepath):
    wb = openpyxl.load_workbook(filepath)
    ws = wb.active
    questions = []
    
    for row in ws.iter_rows(min_row=2, values_only=True):
        if not row[0] or not row[1] or row[2] is None:
            continue
        questions.append({
            'type': row[0],
            'title': row[1],
            'correct': int(row[2]),
            'choices': [row[3], row[4], row[5]]
        })
    return questions

all_data = {}

for filename in sorted(os.listdir('wooclap-preguntas')):
    if filename.endswith('.xlsx'):
        topic = filename.replace('.xlsx', '').split('_')[1]
        all_data[topic] = {
            'filename': filename,
            'category': 'Bloque I',
            'questions': extract_questions_from_excel(f'wooclap-preguntas/{filename}')
        }

for filename in sorted(os.listdir('wooclaps-bloqueII')):
    if filename.endswith('.xlsx'):
        topic = filename.replace('.xlsx', '').split('_')[1]
        all_data[topic] = {
            'filename': filename,
            'category': 'Bloque II',
            'questions': extract_questions_from_excel(f'wooclaps-bloqueII/{filename}')
        }

with open('questions_data.json', 'w', encoding='utf-8') as f:
    json.dump(all_data, f, ensure_ascii=False, indent=2)

print(f"✅ {len(all_data)} temas actualizados")
EOF
```

## 💡 Requisitos

- Python 3 (para el servidor local)
- Navegador web moderno (Chrome, Firefox, Safari, Edge)
- Librería `openpyxl` (solo para regenerar el JSON)

## ⚠️ Notas

- El servidor debe estar corriendo para que la aplicación funcione
- Para detener el servidor: `Ctrl+C` en la terminal
- No cierres la terminal mientras uses la aplicación

---

**¡Buena suerte con tus estudios! 🎓**
