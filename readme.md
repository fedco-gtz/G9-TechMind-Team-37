<div align="center">

# 🧠 TechMind
### Organización Inteligente del Conocimiento Técnico

[![QA](https://img.shields.io/badge/Quality_Assurance-QA-0A66C2?style=flat&logo=checkmarx&logoColor=white)](https://github.com/)
[![Postman](https://img.shields.io/badge/Postman-Web-FF6C37?style=flat&logo=postman&logoColor=white)](https://web.postman.co/)
[![API Testing](https://img.shields.io/badge/API-Testing-00A8E8?style=flat&logo=fastapi&logoColor=white)](https://github.com/)
[![Automation](https://img.shields.io/badge/Test-Automation-6DB33F?style=flat&logo=selenium&logoColor=white)](https://github.com/)
[![JSON](https://img.shields.io/badge/JSON-Validation-000000?style=flat&logo=json&logoColor=white)](https://www.json.org/)
[![OCI](https://img.shields.io/badge/Oracle-OCI-F80000?style=flat&logo=oracle&logoColor=white)](https://www.oracle.com/cloud/)
[![GitHub](https://img.shields.io/badge/GitHub-QA_Repository-181717?style=flat&logo=github&logoColor=white)](https://github.com/)
[![Sprint](https://img.shields.io/badge/Sprint-1-7C3AED?style=flat)](https://github.com/)
[![Hackathon](https://img.shields.io/badge/G9_LATAM-TechMind-blueviolet?style=flat)](https://github.com/ernes2111/G9-Tech-mind-Team-37)
[![Repository](https://img.shields.io/badge/Repository-QA_Testing-2EA44F?style=flat&logo=github&logoColor=white)](https://github.com/ernes2111/G9-Tech-mind-Team-37)

**Hackathon TechMind · G9 LATAM · Equipo 37**

</div>

---
##  📌 ¿Qué es este repositorio?

Este repositorio centraliza toda la documentación, automatización y evidencia del área de Quality Assurance (QA) del proyecto TechMind – Organización Inteligente del Conocimiento Técnico.

El objetivo del área de QA es garantizar que el MVP cumpla los requisitos funcionales definidos por el equipo y que la API REST procese contenido técnico de forma correcta, consistente y trazable.

---
## 🎯 Objetivos de QA
- Validar el funcionamiento de la API REST.
- Verificar la estructura de las respuestas JSON.
- Ejecutar pruebas funcionales, negativas y de integración.
- Automatizar validaciones críticas en Postman.
- Registrar evidencia de ejecución y defectos detectados.
- Mantener trazabilidad de los resultados del sprint.

---
## 🏗️ Arquitectura Bajo Prueba

```
Postman Web / Cliente
        │  POST /contenido
        ▼
┌──────────────────────────────────┐
│   Spring Boot — Puerto 8080      │
└───────────────┬──────────────────┘
                │  HTTP interno POST /predecir
                ▼
┌──────────────────────────────────┐
│   FastAPI (Python) — Puerto 8000 │
│   TF-IDF + Regresión Logística   │
└──────────┬───────────────────────┘
           │
           ▼
┌──────────────────────────────────┐
│   PostgreSQL / OCI ATP           │
│   contenidos · predicciones      │
└──────────────────────────────────┘
```
### Componentes Validados por QA

| Componente | Validación | 
|-----------|-----------|
| **Spring Boot** | Endpoints REST | 
| **FastAPI** | Respuesta del modelo | 
| **PostgreSQL / OCI ATP** | Persistencia de resultados | 
| **JSON** | Estructura y tipos de datos |
| **OCI** | Disponibilidad e integración |

---
## 📬 Contrato de la API

### Endpoint principal

POST `/contenido`

### Request esperado

```
{
  "titulo": "Introducción a Spring Boot",
  "texto": "En este contenido se presentan los conceptos básicos para la creación de APIs REST utilizando Java y Spring Boot."
}
```

### Response esperado

```
{
  "categoria": "Backend",
  "probabilidad": 0.8879,
  "informaciones_adicionales": [
    "spring boot",
    "java",
    "api rest",
    "creación apis",
    "spring"
  ]
}
```

### Validaciones críticas
|Campo | Tipo | Regla |
|-----------|-----------|-----------|
| `categoria` | string | No nulo |
| `probabilidad` | float | 0 ≤ valor ≤ 1 |
| `informaciones_adicionales` | array | Mínimo 1 elemento |

---
## 🧪 Estrategia de Pruebas

### Pruebas Funcionales

Se valida que el sistema:
- Procese contenido técnico correctamente.
- Devuelva una categoría temática.
- Genere una probabilidad de clasificación.
- Extraiga palabras clave relevantes.
- Responda con JSON válido.

### Pruebas Negativas

Se verifica el comportamiento ante:
-   Título vacío.
- Texto vacío.
- JSON mal formado.
- Método HTTP incorrecto.
- Content-Type inválido.
- Campos faltantes.

### Pruebas de Integración

Se comprueba la comunicación entre:
- Spring Boot ↔ FastAPI.
- FastAPI ↔ PostgreSQL.
- API ↔ OCI.

### Pruebas de Regresión

Los casos críticos se reejecutan en cada sprint para asegurar que nuevas modificaciones no afecten funcionalidades previamente validadas.

---
## 📋 Casos de Prueba Críticos

| ID | Escenario | Resultado Esperado | Prioridad |
|-----------|-----------|-----------|-----------|
| CP-01 |	Contenido válido | HTTP 200 + JSON válido | Alta |
| CP-02 |	Título vacío | HTTP 400 | |Alta |
| CP-03 |	Texto vacío | HTTP 400 | Alta |
| CP-04 |	JSON inválido | HTTP 400 | Media |
| CP-05 |	Método GET sobre POST | HTTP 405 | Media | 
| CP-06 |	Validación de estructura JSON | Campos obligatorios presentes | Alta |
| CP-07 |	Persistencia en ATP | Registro almacenado correctamente | Alta |
| CP-08 |	Tiempo de respuesta | < 2000 ms | Alta |

---
## ⚡ Automatización con Postman

La colección de Postman implementa validaciones automáticas para:
- Código HTTP esperado.
- Presencia de `categoria`.
- Presencia de `probabilidad`.
- Rango válido de probabilidad.
- Tiempo de respuesta.
- Formato JSON.

### Script de validación

```
pm.test("Status code is 200", function () { 
    pm.response.to.have.status(200);
});

pm.test("Response has categoria", function () { 
    const jsonData = pm.response.json(); 
    pm.expect(jsonData).to.have.property("categoria");
});

pm.test("Response has probabilidad", function () { 
    const jsonData = pm.response.json(); 
    pm.expect(jsonData).to.have.property("probabilidad");
});

pm.test("Probabilidad is between 0 and 1", function () {
    const jsonData = pm.response.json();
    pm.expect(jsonData.probabilidad).to.be.at.least(0);
    pm.expect(jsonData.probabilidad).to.be.at.most(1);
});

pm.test("Response time is less than 2000ms", function () { 
    pm.expect(pm.response.responseTime).to.be.below(2000);
});
```
---
## 📁 Estructura del Repositorio QA

```
qa/
├── casos-de-prueba/
│   └── Matriz de Casos de Prueba – Sprint 1.xlsx
├── evidencias/
│   ├── capturas/
│   └── respuestas-json/
├── postman/
│   ├── techmind_collection.json
│   └── techmind_environment.json
├── reportes/
│   └── resultados-sprint-1.md
├── QA_TESTING.md
└── README.md
```

---
## 🚀 Cómo ejecutar las pruebas

1. Importar la colección
- Abrir Postman Web e importar:
    **postman/techmind_collection.json**

2. Importar el entorno
- Importar:
    **postman/techmind_environment.json**

3. Configurar la URL
- Variable requerida:
    **base_url = http://localhost:8080**

4. Ejecutar el endpoint
**POST `{{base_url}}/contenido`**

---
## 📊 Criterios de Aceptación

Un caso de prueba se considera APROBADO cuando:
- El código HTTP coincide con el esperado.
- La respuesta es un JSON válido.
- Los campos obligatorios están presentes.
- La probabilidad se encuentra en el rango permitido.
- No se generan errores internos del servidor.
- El tiempo de respuesta cumple el umbral definido.

---
## 📸 Evidencia de Ejecución

Para cada ejecución se registra:
- ID del caso de prueba.
- Fecha y hora.
- Request enviado.
- Response recibido.
- Estado (PASÓ / FALLÓ).
- Captura de Postman.
- Observaciones y defectos detectados.

---
## 📈 Estado del Sprint

### Sprint 1 — QA Status
| Actividad | Estado |
|-----------|-----------|
| Plan de Pruebas | ✅ Completado |
| Matriz de Casos | ✅ Completado |
| Configuración Postman | ✅ Completado |
| Scripts Automáticos | ✅ Completado |
| Ejecución sobre endpoint final | ⏳ Pendiente |
| Evidencia de resultados | ⏳ Pendiente |

---
## 🔍 Checklist Pre-Demo

- [x] Colección de Postman creada.
- [x] Variables de entorno configuradas.
- [x] Casos de prueba documentados.
- [x] Scripts automáticos implementados.
- [ ] Endpoint final disponible.
- [ ] Ejecución completa de pruebas.
- [ ] Evidencia consolidada para la demo.

---
## 🧾 Trazabilidad

Todos los avances, bloqueos y resultados de QA se registran en el canal oficial de Discord del proyecto para mantener trazabilidad del sprint y soporte a la demostración final del hackathon.

---
## 👤 Responsable

**Federico G. Gutierrez**

*QA Tester — TechMind — G9 LATAM Team 37*

---

<div align="center">

TechMind · Quality Assurance Repository · Hackathon G9 LATAM · Equipo 37

*Actualización 22/07/2026*

</div>