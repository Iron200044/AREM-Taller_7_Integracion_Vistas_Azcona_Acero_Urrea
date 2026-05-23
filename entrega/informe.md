# 📄 Informe Técnico del Taller

## 🔖 Nombre del Taller
_Taller 7 - Integración de Vistas de Arquitectura_

## 👥 Integrantes del equipo
- Samuel Acero García (GitHub: Iron200044)
- Andrés Felipe Azcona (GitHub: andresazcona)
- Nicolás Urrea Lara (GitHub: DNico21)

## 🧠 Descripción general del trabajo
El objetivo del taller fue integrar en una sola narrativa visual coherente todas las vistas arquitectónicas desarrolladas a lo largo del curso —negocio, información, aplicaciones, infraestructura y seguridad— mostrando cómo se relacionan entre sí y cómo soportan los objetivos estratégicos del cliente. Inicialmente se trabajó el caso base FarmApp (cadena de farmacias con e-commerce) para comprender la lógica de integración de capas, y posteriormente se aplicó el mismo enfoque al cliente real CEMEDICA IPS, articulando los entregables previos del curso (proceso BPMN, modelo entidad-relación, mapa de infraestructura, análisis STRIDE y checklist normativo) en un tablero integrado único.

## 🔧 Proceso de desarrollo
El equipo inició analizando el caso base FarmApp, identificando sus cinco vistas y organizándolas en un tablero por capas para entender cómo se conectan negocio, datos, aplicaciones e infraestructura, con la seguridad como preocupación transversal.

Luego se trasladó este enfoque al caso real de CEMEDICA. Se recopilaron los entregables previos del curso y se mapeó cada uno a una capa del tablero: el proceso de generación y envío de RIPS a la capa de negocio, el modelo entidad-relación a la capa de información, el sistema legacy EGMH y la aplicación desarrollada a la capa de aplicaciones, el mapa de servidor local y red a la capa de infraestructura, y el análisis STRIDE a la banda transversal de seguridad.

El trabajo central consistió en definir las conexiones verticales (hilos conductores) que atraviesan las capas y permiten rastrear un objetivo de negocio hasta el componente de software y la infraestructura que lo soportan. Para la construcción del tablero se utilizó la herramienta draw.io.

## 🧩 Análisis del modelo propuesto
El modelo propuesto se estructura como un tablero de vistas integradas organizado en cuatro capas horizontales apiladas (negocio → información → aplicaciones → infraestructura) más una banda transversal de seguridad. La coherencia del modelo no reside en las capas individuales sino en tres hilos conductores verticales que las articulan:

- **Hilo 1 — Generación del RIPS:** la actividad de negocio "Generación del RIPS" produce la entidad `RIPS_JSON`, construida por el módulo `construir_json.py` de la aplicación, que se ejecuta sobre el servidor local.
- **Hilo 2 — Envío a SISPRO:** la actividad "Envío y reporte a SISPRO" se registra en las entidades `Envio_RIPS` y `Validacion_RIPS`, es ejecutada por el módulo `cliente_fevrips.py` y viaja por la conexión a Internet hacia la plataforma externa SISPRO.
- **Hilo 3 — Atención del paciente:** la atención y su registro se materializan en las entidades `Paciente`, `Profesional` y `Atención`, gestionadas por los módulos del sistema legacy EGMH y almacenadas en la base de datos SQL Server local.

Este modelo representa adecuadamente las necesidades del cliente porque demuestra trazabilidad: todo objetivo estratégico de CEMEDICA puede seguirse hasta un componente físico concreto. Adicionalmente, el tablero distingue visualmente el estado actual (AS-IS, proceso manual de conversión a JSON) del estado objetivo (TO-BE, la aplicación desarrollada por el equipo), evidenciando el aporte del proyecto.

Se asumió que el sistema legacy EGMH no genera RIPS en formato JSON ni dispone de API, que el proceso actual de reporte es manual, y que la organización busca cumplir la normativa vigente sin reemplazar por completo el sistema legacy, conservándolo para la operación clínica e interviniendo únicamente el tramo regulatorio.

## 📋 Tabla de actores, entidades o componentes (si aplica)

| Nombre del elemento | Tipo | Descripción | Responsable |
|---------------------|------|-------------|-------------|
| Paciente | Actor | Persona que recibe atención médica y cuyos datos alimentan el RIPS | Cliente |
| Profesional de salud | Actor | Registra la atención médica en el sistema | Cemedica |
| Sistema legacy EGMH | Sistema | Plataforma monolítica (2008) de historias clínicas; sin API ni JSON | Cemedica |
| App RIPS (Python/Tkinter) | Componente | Aplicación desarrollada que extrae datos, construye el JSON y orquesta el envío | Cemedica |
| Validador MUV (Docker FEV-RIPS) | Componente externo | Contenedor oficial de MinSalud que valida el RIPS JSON | MinSalud |
| SISPRO | Sistema externo | Plataforma del gobierno para la recepción del RIPS | MinSalud |
| Servidor local | Infraestructura | Equipo on-premise único donde corre toda la operación | Cemedica |
| Administrador TI | Actor | Responsable de la gestión técnica, la seguridad y el gobierno del sistema | Cemedica |

## 🔍 Investigación complementaria
### Tema investigado:
Marcos y buenas prácticas para la documentación e integración de vistas arquitectónicas

### Resumen:
La integración de vistas arquitectónicas es una práctica formalizada en los principales marcos de arquitectura empresarial. TOGAF (The Open Group Architecture Framework) define los mismos dominios que componen el tablero —negocio, datos, aplicaciones y tecnología— y aporta la lógica de Análisis de Brechas entre un estado actual (Baseline / AS-IS) y un estado objetivo (Target / TO-BE), que es la estructura sobre la que se construyó el tablero de CEMEDICA.

ArchiMate, el lenguaje estándar de The Open Group para modelar arquitecturas empresariales, es la referencia más directa de este taller: su propuesta central es representar las capas de negocio, aplicación y tecnología en un único modelo coherente con relaciones explícitas entre ellas, idea que inspiró las conexiones verticales del tablero. A nivel teórico, el modelo de vistas "4+1" de Philippe Kruchten sustenta el principio de que ninguna vista única describe una arquitectura completa: se requieren varias vistas consistentes entre sí. Complementariamente, el modelo C4 de Simon Brown aporta la buena práctica de mantener coherencia entre niveles de detalle, lo que orientó la decisión de mantener el tablero a nivel de componentes y reservar el detalle fino para los entregables previos.

Estos marcos evidencian que una arquitectura empresarial no es una colección de diagramas aislados, sino un conjunto de vistas que deben articularse para servir como herramienta de decisión organizacional.