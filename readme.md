<div align="center">

# 🧠 TechMind
### Organización Inteligente del Conocimiento Técnico

[![QA](https://img.shields.io/badge/Quality_Assurance-QA_v1.0-0A66C2?style=flat&logo=checkmarx&logoColor=white)](https://github.com/)
[![Postman](https://img.shields.io/badge/Postman-Web-FF6C37?style=flat&logo=postman&logoColor=white)](https://web.postman.co/)
[![API Testing](https://img.shields.io/badge/API-Testing-00A8E8?style=flat&logo=fastapi&logoColor=white)](https://github.com/)
[![Automation](https://img.shields.io/badge/Test-Automation-6DB33F?style=flat&logo=selenium&logoColor=white)](https://github.com/)
[![JSON](https://img.shields.io/badge/JSON-Validation-000000?style=flat&logo=json&logoColor=white)](https://www.json.org/)
[![OCI](https://img.shields.io/badge/Oracle-OCI-F80000?style=flat&logo=oracle&logoColor=white)](https://www.oracle.com/cloud/)
[![Sprint](https://img.shields.io/badge/Sprint-1_Completado-7C3AED?style=flat)](https://github.com/)
[![Hackathon](https://img.shields.io/badge/G9_LATAM-TechMind-blueviolet?style=flat)](https://github.com/ernes2111/G9-Tech-mind-Team-37)
[![Repository](https://img.shields.io/badge/Repository-QA_Testing-2EA44F?style=flat&logo=github&logoColor=white)](https://github.com/ernes2111/G9-Tech-mind-Team-37)

**Hackathon TechMind · G9 LATAM · Equipo 37**

</div>

---
## 📌 ¿Qué es este repositorio?

Este repositorio centraliza toda la documentación, automatización, evidencias y reportes del área de **Quality Assurance (QA)** del proyecto **TechMind – Organización Inteligente del Conocimiento Técnico**.

El objetivo principal de QA es garantizar que el MVP cumpla los requisitos funcionales, asegure la resiliencia ante errores de entrada y valide que la API REST procese contenido técnico de forma correcta, consistente y trazable.

---
## 🎯 Objetivos de QA
- Validar el funcionamiento integral de la API REST (`FastAPI`).
- Verificar la estructura y los tipos de datos de las respuestas JSON.
- Ejecutar pruebas funcionales, negativas, de borde y de seguridad (Inyección SQL).
- Automatizar validaciones críticas en Postman.
- Registrar evidencia visual y técnica de ejecución de pruebas.
- Mantener trazabilidad de los resultados del sprint mediante reportes ejecutivos.

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

| Componente | Validación QA | 
|-----------|-----------|
| **Spring Boot** | Endpoints REST de cara al cliente | 
| **FastAPI** | Respuesta del modelo ML y sanitización de entrada | 
| **PostgreSQL / OCI ATP** | Persistencia correcta de resultados y esquema | 
| **JSON** | Estructura, presencia de claves y tipos de datos (Pydantic) |
| **OCI** | Disponibilidad e integración en la nube |

---
## 📬 Contrato de la API

### Endpoint Principal
`POST /predecir` (vía FastAPI) / `POST /contenido` (vía Spring Boot)

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
## 🧪 Estrategia de Pruebas Ejecutadas

- Pruebas Funcionales (Flujo Feliz): Verificación de clasificación correcta, extracción de palabras clave, cálculo de probabilidad y normalización de textos (mayúsculas/tildes).
- Casos Borde y Stress Leve: Evaluación ante payloads masivos (+500k caracteres) y resistencia a caracteres especiales/emojis (UTF-8).
- Validación de Esquema y Tipos: Confirmación de rechazo inmediato (HTTP 422) ante datos con tipos incorrectos (números o booleanos).
- Pruebas de Seguridad y Robustez: Inmunidad contra inyección SQL y rechazo de cabeceras no soportadas (XML).
- Endpoints Complementarios: Diagnóstico del estado del servicio (GET /health) y consulta de catálogo (GET /categorias).
- Integridad de Datos: Manejo de excepciones ante títulos vacíos, textos vacíos, JSONs mal formados o métodos HTTP no permitidos.

---
## 📊 Resumen de Resultados QA — Sprint 1 (v1.0)

| Categoria | Planificado | Pasó | Falló |
|-----------|-----------|-----------|-----------|
| Funcionales (Flujo Feliz) | 7 | 7 | 0 |
| Casos Borde / Encoding (UTF-8) | 2 | 2 | 0 |
| Validación de Esquema / Tipos | 1 | 1 | 0 |
| Seguridad (Inyección SQL / Content-Type) | 2 | 2 | 0 |
| Endpoints Complementarios | 2 | 2 | 0 | 
| Integridad de Datos |	6 | 6 | 0 |
| **TOTAL** | **20** | **20** | **0** |

** 📄 El detalle de ejecución caso por caso y los gráficos de cobertura se encuentran en reportes/resultados-sprint-1.md.

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
│   ├── informes/
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
## 📈 Estado del Sprint

### Sprint 1 — QA Status
| Actividad | Estado |
|-----------|-----------|
| Plan de Pruebas | ✅ Completado |
| Matriz de Casos | ✅ Completado |
| Configuración Postman | ✅ Completado |
| Scripts Automáticos | ✅ Completado |
| Ejecución sobre endpoint final | ✅ Completado |
| Evidencia de resultados | ✅ Completado |

---
## 🔍 Checklist Pre-Demo

- [x] Colección de Postman creada.
- [x] Variables de entorno configuradas.
- [x] Casos de prueba documentados.
- [x] Scripts automáticos implementados.
- [x] Endpoint final disponible.
- [x] Ejecución completa de pruebas.
- [x] Evidencia consolidada para la demo.

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

*Actualización 23/07/2026*

</div>