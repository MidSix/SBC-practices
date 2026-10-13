# Toda la teoría necesaria — Modelo de Contexto (CommonKADS) para nuestro SBC de envíos

> **Para qué sirve este documento.** Para que cualquiera del grupo pueda **defender cada casilla** del Modelo de Contexto delante de la profesora: saber *qué pide* cada formulario, *por qué* existe, *qué hemos puesto* y *por qué es correcto* (o dónde es débil). Está construido **solo con los conceptos de la asignatura** (Tema 1, Tema 2, enunciado de la práctica, presentación del tema de prácticas, transcripciones de clase y ejemplo de Horarios), más el conocimiento del dominio (logística y normativa) que necesitamos para que el contenido sea creíble.
>
> **Cómo usarlo.**
> 1. Lee las Partes I y II (teoría) una vez con calma.
> 2. Para cada formulario, la Parte III te dice qué pide, los errores típicos y cómo argumentar el nuestro.
> 3. La Parte IV amplía el dominio (mercancías peligrosas, segregación, leyes por país…).
> 4. La **Parte V es el banco de preguntas**: respondedlas en grupo **por escrito**. Si alguna respuesta contradice lo que hay en el LaTeX, hay que cambiar el LaTeX.
> 5. La Parte VI resume qué he cambiado en OM-1…OM-3 y por qué.

---


| Concepto | Qué es | Dónde está |
|----------|--------|------------|
| **Proceso** | Una actividad de negocio que la empresa hace hoy, como recepción, admisión, embalaje o agrupación | Se dibuja en OM-2 ("Procesos") |
| **Tarea** | Un paso dentro de un proceso | OM-3 (T1...T9) |
| **Solución** | Lo que proponemos construir, el SBC | OM-1, y se valora en OM-5 |

En OM-3 describes lo que se hace hoy, y vuestro documento ya lo hace bien. No propones nada ahí.

Qué son las tareas de OM-3
Son los pasos que ejecutan hoy las personas (o el software actual)

El enunciado pide el modelo "ANTES DEL DESARROLLO DEL SBC". OM-1 y OM-2 a OM-4 describen el estado actual de la empresa. La propuesta aparece solo en OM-5, que decide qué tareas de OM-3 se automatizan

## ⚠️ 0. Estado de la entrega y plazos

| Fecha | Qué |
|---|---|
| **16/10/2026** | **Entrega del Modelo de Contexto** |
| 13/10 – 16/10 | Presentación del Modelo de Contexto (sin clase en aulas) |
| 11/12/2026 (según calendario) | Entrega del Modelo Conceptual |

**Ojo: según el enunciado, el Modelo de Contexto NO es solo OM-1…OM-5.** El índice del enunciado incluye:

- OM-1, OM-2, OM-3, OM-4, OM-5 → **hechos** (OM-4 y OM-5 los he redactado yo; revisadlos).
- **TM-1** (una por cada tarea intensiva en conocimiento que se vaya a automatizar, es decir, las elegidas en OM-5: **T2, T3, T4, T6, T7**). **Pendiente.**
- **TM-2** (una por cada elemento de conocimiento relevante de esas tareas). **Pendiente.**
- **AM-1** (una por cada agente implicado en esas tareas). **Pendiente.**
- OTA-1 → **NO es necesario** en prácticas (lo dice el enunciado explícitamente).

Si el `Todo.md` del repo solo lista OM1–OM5, **le faltan TM-1, TM-2 y AM-1**. Confirmadlo con la profesora si tenéis dudas, pero el enunciado es claro.

---

# PARTE I — Fundamentos (Tema 1)

## 1. Conceptos base

### 1.1 Sistema Basado en Conocimiento (SBC)
Definición de la asignatura:
> *"Un sistema software capaz de soportar la **representación explícita del conocimiento** de un dominio específico y de **explotarlo** a través de los mecanismos apropiados de **razonamiento** para proporcionar un comportamiento de nivel alto en la resolución de problemas."*

Dos palabras clave para defender nuestro SBC: **explícita** (el conocimiento está separado y es legible, no enterrado en el código) y **razonamiento** (no es una consulta a una tabla; combina conocimiento para llegar a una conclusión).

En la presentación de prácticas: *"Un SBC es software que implementa un proceso inteligente. **Lo difícil es obtener el 'algoritmo' que simula el proceso.**"* → Por eso el 80 % del trabajo de la asignatura es **adquirir y modelar** conocimiento, no programar.

### 1.2 Ingeniería del Conocimiento (IC)
> Disciplina que aplica una aproximación **sistemática, disciplinada y cuantificable** al desarrollo, funcionamiento y mantenimiento de SBC (paralelo a la definición IEEE de Ingeniería del Software).

El trabajo del ingeniero de conocimiento: **adquirir, estructurar y formalizar** el conocimiento experto, es decir, *extraerlo de las fuentes apropiadas y modelarlo de forma computacionalmente adecuada*.

### 1.3 Datos → información → conocimiento
- **Dato**: símbolo sin interpretar ("UN1266", "3 kg").
- **Información**: dato con significado en un contexto ("el paquete contiene 3 kg de productos de perfumería, nº ONU 1266").
- **Conocimiento**: capacidad de **usar** la información para decidir/actuar ("UN1266 es clase 3, inflamable; en avión de pasajeros solo se admite hasta cierta cantidad por bulto; no puede ir junto a comburentes…").

**El conocimiento es un recurso estratégico de la organización** (Tema 1). Esta frase justifica el OM-4 entero y nuestra oportunidad O2.

### 1.4 Características de los SBC (vs. software convencional)
| Software convencional (SI) | SBC |
|---|---|
| Sistemático y procedimental | **Declarativo y heurístico** |
| Entrada organizada, salida por algoritmo determinista | Se adapta al estado del problema, puede modificar objetivos en ejecución |
| — | **Separación entre conocimiento del dominio y métodos de resolución** → reutilización |
| — | **Justificación y explicación** de sus decisiones |
| — | Mantenimiento perfectivo continuo |

**Ventajas**: mantenimiento del conocimiento, resolución de problemas complejos, ajuste de objetivos, tratamiento de la incertidumbre, **explicación del razonamiento**, reducción de costes, aumento de la fiabilidad, modularidad.
**Limitaciones**: **dificultad en la adquisición de conocimiento** (cuello de botella), reutilización del conocimiento, falta de creatividad y sentido común, obstáculos para aprender y adaptarse.

> 💬 **Para defender**: casi todas las ventajas encajan con nuestro problema: la **explicación** (la normativa obliga a poder justificar), la **separación conocimiento/razonamiento** (las normas cambian cada 1–2 años → P4) y la **fiabilidad** (seguridad).

### 1.5 Historia y crisis (por qué existe la metodología)
- Años 50–70: métodos generales de resolución de problemas → no escalan → se abandona el entusiasmo inicial (informe Lighthill).
- Nacen los SBC con **conocimiento específico**: DENDRAL, **MYCIN** (separa conocimiento específico del de resolución; razona con incertidumbre), PROSPECTOR.
- Años 80, éxito industrial: R1/XCON de DEC (ahorró 40 M$ en 1986; por cierto, es una tarea de **configuración**, como nuestro módulo de agrupación).
- **Crisis**: ciclo codificación-reparación, conocimiento en un único nivel de abstracción, mezcla de **qué, cómo y por qué**, mala estimación de plazos, verificación y mantenimiento costosos.
- **Las metodologías derivadas de la IS fallan** porque el dominio y la adquisición no están bien definidos, es difícil fijar todos los requisitos al principio, los resultados solo se ven al final y el coste y el mantenimiento son complicados.

### 1.6 Perspectiva de transferencia vs. perspectiva de modelado
- **Transferencia** (antigua): experto → ingeniero → ordenador; se "vacía" la cabeza del experto en reglas. **Cuello de botella: la adquisición de conocimiento.**
- **Modelado** (Newell, CommonKADS): el ingeniero construye un **modelo en el nivel de conocimiento**, independiente de la implementación, que después se implementa.
- Consecuencias: *desarrollar un SBC es una actividad de modelado*, el conocimiento tiene una **estructura interna analizable** (roles, categorías), el modelado tiene algo de **subjetivo** (el sesgo del diseñador) y **hace falta evaluar el sistema frente a la realidad**.

### 1.7 Newell: el nivel de conocimiento
- Un sistema se puede describir por **qué sabe**, **qué objetivos persigue** y **qué acciones son racionales dadas sus creencias**, sin hablar de reglas, redes neuronales o LLM (independiente de la implementación).
- *"El nivel de conocimiento explica **por qué** una acción tiene sentido, no **cómo** se implementa."*
- Hoy vuelve a ser relevante: la IA actual "predice muy bien" pero no responde a *¿por qué esta decisión?, ¿con qué supuestos?, ¿cuándo no es fiable?* → **CommonKADS = ingeniería del nivel de conocimiento.**
- Ejemplo del tema (diagnóstico clínico): un LLM solo extrae conceptos; la **capa simbólica** (ontología, reglas, objetivos explícitos de seguridad) **razona y explica**.

> 💬 **Para defender la alternativa "usar un LLM" (OM-5)**: los LLM operan *por debajo* del nivel de conocimiento (símbolos estadísticos). En un dominio regulado y de seguridad necesitamos conocimiento explícito, trazable y auditable. **Sí** sería razonable un sistema **híbrido** en el futuro (un LLM extrae datos de la descripción libre del paquete y la capa simbólica decide), que es justo la línea de "hibridación simbólico-aprendizaje" del tema.

### 1.8 McDermott: métodos de limitación de roles
El conocimiento juega **roles** concretos en el razonamiento. Se separa el **conocimiento de resolución de problemas** (genérico, parecido a un pseudocódigo: "primero abstraigo los síntomas, luego…") del **conocimiento declarativo del dominio**. El método genérico se puede **reutilizar** en dominios distintos ("me da igual diagnosticar una red que una persona").
⚠️ En clase avisaron: estos "roles" no son los "roles" que aparecerán después en el modelo de conocimiento.

### 1.9 Chandrasekaran: tareas genéricas y estructura de tareas
- **Tarea**: tipo de problema con un objetivo (entrada → salida).
- **Método**: forma de realizar la tarea descomponiéndola en **subtareas** (con orden y control).
- **Estructura de tareas**: árbol tarea → método → subtareas, aplicado recursivamente hasta llegar a tareas que se resuelven con el conocimiento disponible. **Una tarea puede tener varios métodos.**
- Existen "cajones" genéricos (diagnóstico, monitorización, asignación…) que después se rellenan con el dominio.

### 1.10 CommonKADS: la "suite" de modelos

| Nivel | Modelos | Pregunta |
|---|---|---|
| **Contextual** | Organización (OM), Tareas (TM), Agentes (AM) | **¿Por qué?** |
| **Conceptual** | Conocimiento, Comunicación | **¿Qué?** |
| **Artefactual** | Diseño | **¿Cómo?** |

- **El modelo central es el de conocimiento** (el siguiente entregable).
- **Ciclo de vida en espiral**: la planificación se centra en productos/salidas, es adaptativa en ciclos dirigidos por una **valoración de riesgos** y el **control de calidad** forma parte del proyecto.
- Por eso el enunciado dice que el modelo de contexto se hace varias veces y que **nosotros lo hacemos en el estado inicial: ANTES de que exista el SBC.** ← Esto condiciona la redacción de OM-2, OM-3, OM-4, TM y AM (describen **cómo se trabaja hoy**), mientras que OM-5 sí habla de la solución propuesta.

### 1.11 CommonKADS hoy (Tema 1, final)
- Documentación, **auditoría**, puente entre técnicos, expertos y reguladores.
- El **AI Act europeo** exige documentación, evaluación de riesgos y **supervisión humana** → los modelos de tareas/agentes/comunicación son una base natural para cumplirlo.
- *"La explicabilidad no se añade al final: se diseña desde el modelo de conocimiento."*
- En clase: en sistemas críticos **la decisión final la toma una persona**, y la explicación debe adaptarse a cada destinatario (no se explica igual al operario que al director).

---

# PARTE II — El modelado del contexto (Tema 2)

## 2. Por qué modelar el contexto
- Un sistema inteligente **no suele fracasar por la técnica**, sino por una **mala integración organizativa**, tareas mal entendidas y agentes mal definidos (Tema 2, "¿por qué falla un sistema inteligente?").
- Un SBC actúa como **un agente más** que coopera con otros (humanos o no) y realiza una **fracción** de las tareas de la organización.
- **La aceptación de los usuarios es crítica.** Ejemplo de clase: diseñas un sistema estupendo, pero nadie tiene entre sus tareas meterle los datos → **hay que crear una tarea nueva** (lo hemos recogido en OM-5: el operario de recepción debe registrar los datos de forma estructurada).
- Qué recoge el modelo de contexto: el **contexto de la organización** (objetivos, estrategias, políticas), los **actores** (personas y roles, responsabilidades, interacciones), las **tareas y procesos**, los **recursos** (datos, herramientas, tecnología) y las **restricciones** (aspectos éticos, legales y de seguridad).

### Metas del modelado del contexto
1. Identificar problemas y oportunidades de desarrollo de SBC.
2. Decidir soluciones y estudiar su **viabilidad**.
3. Mejorar las tareas en cuanto a organización y conocimiento.
4. Planificar los **cambios necesarios en la organización**.

## 3. Los dos pasos (¡clave para entender el orden!)

| Paso | Estudio | Herramienta | Análisis | Síntesis (decisión) |
|---|---|---|---|---|
| **1** | **Alcance y viabilidad** | **OM** (OM-1…OM-5) | Identificar áreas problema/oportunidad y soluciones; situarlas en la organización | Estudiar la viabilidad **económica, técnica y del proyecto**; **elegir el área más prometedora** y la solución |
| **2** | **Impacto y mejoras** | **TM, AM** (TM-1, TM-2, AM-1) + OTA-1 | Interrelaciones entre tareas, agentes y conocimiento; ¿qué mejoras se pueden lograr? | Decidir **cambios** en tareas y organización; asegurar la **aceptación** e integración |

**Cada estudio tiene una parte de análisis y otra de decisión constructiva.**

### Formularios
| Modelo | Formularios |
|---|---|
| Organización | OM-1, OM-2, OM-3, OM-4, OM-5 (5) |
| Tareas | TM-1, TM-2 (2) |
| Agentes | AM-1 (1) |
| Resumen | OTA-1 (1) — **no se pide en prácticas** |

### Proceso general (diagrama del Tema 2)
```
OM-1 (problemas, soluciones, contexto)
  └─> OM-2 (describir el área foco) ──> OM-3 (descomponer el proceso) ─┐
                                   └──> OM-4 (activos de conocimiento) ─┤
                                                                         v
                                              OM-5 (juzgar la viabilidad = documento de decisión)
                                                 ├─ si no es viable → PARAR
                                                 └─ si es viable → TM-1 → TM-2 → AM-1 → OTA-1
                                                                  (comparar la situación actual y la propuesta)
```

### Documentación de viabilidad (Tema 2)
- 1 OM-1.
- 1 OM-2 **por área** problema/oportunidad (prácticas: **solo la elegida**).
- 1 OM-3 por proceso (una fila por tarea).
- 1 OM-4 con una fila **por activo de conocimiento**.
- Para cada solución: la decisión de automatizar o no, con sus razones, y el OM-5.

## 4. Las 9 categorías de tareas de los SBC (presentación de prácticas)
Clasificación · Asesoramiento · Diagnóstico · Monitorización · Síntesis · Diseño de configuraciones · Asignación · Planificación · Ordenación cronológica (*scheduling*).

**¿Dónde encaja nuestro SBC?**
| Parte del SBC | Categoría | Por qué |
|---|---|---|
| Admisión (T2, T3, T4) | **Asesoramiento / valoración** (con una subtarea de clasificación) | "Encontrar la decisión correcta para un caso basándose en criterios": ¿se admite el paquete y en qué condiciones? |
| Agrupación (T7) + modo (T6) | **Asignación** (y **diseño de configuraciones** si se modela la colocación) | "Crear una relación entre dos grupos de objetos (paquetes ↔ unidades de carga) que cumpla requisitos y restricciones" |

> ⚠️ La presentación **desaconseja la clasificación pura** porque produce prácticas demasiado sencillas. Nuestro SBC **no** es solo clasificar la mercancía: la clasificación es una subtarea de una **valoración**, seguida de una **asignación** con restricciones. Esto es un buen argumento de complejidad.
> ⚠️ También dicen: *"No tengáis miedo a elegir un tema complejo: simplificarlo es muy sencillo."* Nuestro alcance es amplio; OM-5 ya lo acota (las mercancías más frecuentes y los principales destinos).

---

# PARTE III — Formulario a formulario: qué pide, errores típicos y cómo defender el nuestro

## OM-1 — Problemas, oportunidades, contexto y soluciones

**Qué pide (enunciado):**
- **Problemas y oportunidades**: lista corta de los problemas que puede resolver el SBC **y otras posibilidades de mejora** observadas (en la realidad se obtienen de entrevistas, tormentas de ideas y reuniones con los directivos).
- **Contexto organizacional**: 1) misión, visión y objetivos; 2) factores externos; 3) estrategia; 4) escala de valores. Sirve para **situar los problemas en perspectiva** (parte "invariante" de la organización).
- **Soluciones**: posibles soluciones para los problemas y oportunidades **dentro del contexto**. ⚠️ Aunque el enunciado dice "para cada uno", la profesora pide **como máximo 2 soluciones, idealmente 1 que los cubra todos** (ver abajo).

**Errores típicos / feedback recibido:**
- ❌ **"Oportunidad" que lo es para nosotros (el equipo) y no para la empresa.** Ejemplo de error: "aprender a hacer un SBC" o "es una buena oportunidad para usar IA". ✅ Una oportunidad es una **posibilidad de mejora para la ORGANIZACIÓN** compatible con su contexto (misión, estrategia). Por eso O1 ahora es "**captar un segmento de clientes** (comercio electrónico con mercancías peligrosas de uso cotidiano) que hoy la empresa pierde" y O2 "convertir el conocimiento tácito en **un activo de la empresa**". Las dos se derivan de su estrategia (fiabilidad, cumplimiento) y de factores externos (crecimiento del comercio electrónico, rotación de personal).
- ❌ **Una solución por cada problema** (segunda corrección de la profesora). ✅ **Como máximo 2 soluciones, idealmente 1 que resuelva todos los problemas.** Motivo teórico: en CommonKADS **cada solución de OM-1 abre su propia cadena de análisis**. El Tema 2 dice "para cada solución sugerida: formulario OM-5", y el enunciado dice que OTA-1 solo hace falta "cuando se identifican varios problemas y se proponen **varias soluciones** en OM-1 y, en consecuencia, es necesario hacer **varios OM-5**". Con 8 "soluciones" deberíamos tener 8 viabilidades; con una sola, el modelo es coherente: 1 solución → 1 OM-2 → 1 OM-5 → sin OTA-1. Es lo que hace el ejemplo de Horarios ("La solución que se propone es desarrollar un SBC que…").
  - Ahora OM-1 tiene **una única solución** (el SBC de apoyo a la admisión, preparación y agrupación), descrita por sus **tres capacidades**, indicando entre paréntesis qué problemas y oportunidades cubre cada una (P1, P2, P3, O1 / P5, P6 / P4, O2). Son **capacidades de un mismo sistema**, no soluciones separadas: así se demuestra que la solución cubre todos los problemas sin volver a "una por problema".
  - Las **alternativas no SBC** (más expertos, software comercial, listas de comprobación, LLM generalista) **ya no están en OM-1**: se comparan en OM-5, en "Comparación con soluciones alternativas", que es donde la plantilla las pide ("¿Y comparado con otras soluciones alternativas?").
  - Si quisierais **2 soluciones**, la segunda tendría que ser una alternativa real y completa (por ejemplo, "reorganización sin SBC: un segundo técnico de mercancías peligrosas + software comercial por modo + procedimientos escritos"), y en rigor necesitaría su propia viabilidad. No compensa: con una basta.
- ❌ Contexto sin datos concretos de la empresa. En el ejemplo de Horarios dan cifras (66 profesores, 33 aulas). ✅ He añadido un **perfil de empresa** (≈60 empleados, ≈400 envíos/día, transitario que consolida). **Estas cifras son inventadas (empresa ficticia, que está permitido)**: decidid si las mantenéis, porque OM-5 se apoya en ellas.

**Cómo defender nuestro OM-1:**
- Cada problema se puede **rastrear** hasta una tarea de OM-3 y un activo de OM-4: P1/P3 → T2, T4 y K3; P2 → T5, T8 y K2, K7; P4 → K2, K4, K6 (cambian); P5 → T6 y K8, K9; P6 → T7 y K10, K11.
- El contexto **explica por qué** son problemas: la estrategia de la empresa es la fiabilidad y el cumplimiento normativo, así que un error de cumplimiento golpea en el centro de su estrategia.

## OM-2 — Aspectos variables del área elegida

**Qué pide:** describir la **subparte** de la organización afectada por la solución SBC elegida (prácticas: solo una), con estos epígrafes:
| Epígrafe | Contenido | Ojo |
|---|---|---|
| Estructura | Organigrama de departamentos, grupos y unidades | Solo la parte relevante |
| Procesos | Diagrama del proceso que **luego se descompone en tareas en OM-3** | Si solo hay un proceso de interés, basta su nombre |
| Personal | Quién participa: **demandantes, destinatarios o proveedores de conocimiento** | **Roles funcionales, NO personas reales** ("no hay que poner María Jiménez") |
| Recursos | 1) Sistemas de información; 2) Equipamiento y material; 3) Experiencia social o interpersonal no intensiva en conocimiento; 4) Tecnología, patentes | **NO incluir conocimientos ni competencias** aquí |
| Conocimiento | Recurso especial → **"Véase OM-4"** | Correcto tal como está |
| Cultura y potencial | **Reglas no escritas**, formas de trabajar y de comunicarse, relaciones informales | Justifica riesgos de aceptación |

**Cómo defender el nuestro:**
- **Personal**: cada rol está etiquetado como proveedor/demandante/destinatario de conocimiento, que es exactamente lo que pide el enunciado.
- **Recursos**: el conocimiento no aparece aquí (está en OM-4). He corregido "Tecnología, patentes": antes contenía los contratos con transportistas, que no son tecnología; ahora dice explícitamente que no hay patentes y añade las **publicaciones normativas** (el *soporte* físico/PDF es un recurso material; el *contenido* como conocimiento está en OM-4 como K2).
- **Cultura y potencial**: "se consulta informalmente al experto", "conocimiento no documentado" y "resistencia al cambio" son los argumentos que luego usa OM-5 (compromiso de los expertos) y que usará AM-1.

## OM-3 — Descomposición del proceso en tareas

**Columnas:** Nº | Tarea (parte de los procesos de OM-2) | Realizada por (agente humano o software; **personal o recursos de OM-2**) | ¿Dónde? (**posición en la estructura de OM-2**) | Recursos de conocimiento (nombre; se detallan en OM-4) | ¿Intensiva en conocimiento? (**sí/no**) | Importancia (**criterio definido por el ingeniero de conocimiento**, p. ej. escala 1–5).

**Nota del enunciado:** *"es muy importante identificar y **distinguir los recursos de conocimiento formales y los no formales**"*.

**Conceptos para argumentar:**
- **Intensiva en conocimiento**: la tarea requiere **razonar con conocimiento especializado** (normas que interpretar, heurísticas, experiencia), no solo ejecutar un procedimiento. Ejemplo de clase (Horarios): "determinar los recursos del IES" es **obligatoria e imprescindible** pero **no intensiva** ("lo que voy a hacer es contar"). → **La importancia y la intensidad son ejes independientes.** Nuestra T1 (registro) es Baja/No; nuestra T5 (embalaje) es Media/No: es importante, pero el operario **sigue instrucciones** que vienen de otro sitio.
- **Formal (F)**: explícito, escrito, verificable (normativa, procedimientos, tablas). **No formal (NF)**: tácito, basado en la experiencia, en la cabeza del experto.
- **¿Por qué descomponer procesos y activos?** (Tema 2) Porque el conocimiento **no está centralizado**, **puede ser tácito** y **puede ser un cuello de botella**.

**Cambios que he hecho (y por qué):**
1. Nombres **coherentes con OM-2** (antes ponía "Responsable de transporte", que no existe en OM-2: el rol es "Responsable de tráfico"; "Apartamento" → "Mercancías peligrosas"; "¿Dónde?" ahora usa los nombres exactos de las cajas del organigrama).
2. Recursos de conocimiento con **código K1…K13** y marca **(F)/(NF)** → trazabilidad directa con OM-4 y cumplimiento de la nota del enunciado.
3. **Criterio de importancia definido** (Alta = un error puede causar un incidente de seguridad, una sanción o una retención; Media = retrasos o sobrecostes; Baja = errores fáciles de corregir). Antes no había criterio, y el enunciado lo pide.
4. T7 (agrupación) pasa de **Media a Alta**, aplicando el criterio: agrupar mercancías incompatibles = **incidente de seguridad**. Además es el núcleo de vuestra idea.
5. T4 antes decía "Resultados de las tareas previas" como recurso de conocimiento. **Eso es un objeto de entrada, no conocimiento.** Ahora T4 usa K2, K3, K4 y K6 (criterios de admisión).

**Punto débil a vigilar:** T9 tiene "Parcialmente" en una columna que pide sí/no. Lo he dejado porque la profesora ya lo vio, pero si os lo piden: "Sí, en grado bajo (requiere experiencia K5 en retenciones), pero de importancia media y no se automatiza en esta iteración".

## OM-4 — Activos de conocimiento

**Columnas:** Recurso de conocimiento (nombre, véase OM-3) | Pertenece a (agente, OM-3) | Usado en (tarea, OM-3) | **¿Forma correcta?** | **¿Lugar correcto?** | **¿Tiempo correcto?** | **¿Calidad correcta?** (sí/no + **comentario obligatorio si es "no"**).

**Las cuatro preguntas (lo más importante de defender):**
| Pregunta | Significado | Ejemplo de clase |
|---|---|---|
| **Forma** | ¿Está el conocimiento en la forma que necesita la tarea? (mental vs. papel vs. electrónico; tácito vs. explícito; estructurado o no) | Horarios: *"No, reside en el experto"* y queremos meterlo en un sistema. *"Lo tengo en papel y lo quiero automático → no está en la forma adecuada."* |
| **Lugar** | ¿Está disponible **donde** se ejecuta la tarea? | *"No está en el sitio adecuado, necesito cambiarlo."* |
| **Tiempo** | ¿Está disponible **cuando** se necesita, con la rapidez necesaria? | Horarios: el director tarda un mes en hacer los horarios. |
| **Calidad** | ¿Es correcto, completo, actualizado y homogéneo? ¿Se puede medir? | Horarios: *"la solución suele ser incorrecta y a veces incompleta"*. |

**Idea clave para defenderlo:** cada "No" es un **cuello de botella del conocimiento** y, por tanto, un argumento a favor (o en contra) del SBC. Un activo con cuatro "Sí" (K1, K12) **no necesita SBC**; por eso T1 y T8 no se automatizan. **OM-4 es la justificación cuantitativa de qué tareas automatizar en OM-5.**

**Decisiones de nuestro OM-4 que debéis entender:**
- **K2 (normativa)**: la calidad es **Sí** (es la fuente oficial). El problema es forma/lugar/tiempo. Esto demuestra que no ponemos "No" a todo por sistema.
- **K3 (experiencia del técnico)**: calidad "Sí, pero…": correcta, pero **no homogénea ni registrada**. Si os preguntan "¿por qué no 'No'?": porque el resultado del experto es fiable; lo que falla es la consistencia y la trazabilidad (es una mejora que aporta el SBC, no un fallo del experto).
- **K10 (segregación)**: cuatro "No". Es **el activo más crítico** y el núcleo de P6: la tabla la conoce el técnico, pero la aplica tráfico, con prisa y sin criterios internos para mercancía no peligrosa (limpieza y alimentos).
- **K5**: lugar Sí (la experiencia se usa en Aduanas, donde está), pero tiempo No (un único responsable).
- **K8**: forma **Sí** (electrónica) → demuestra que distinguimos entre "electrónico" y "bien organizado"; los problemas están en el tiempo y la calidad.

**El Tema 2 añade (diapositiva 15)** que de los activos de conocimiento salen la **identificación de casos de uso**, el **análisis del impacto de la IA** y la **evaluación de la viabilidad sociotécnica**. Es decir: OM-4 alimenta OM-5.

## OM-5 — Documento de viabilidad

**Formato:** **narración, NO pregunta-respuesta** (nota del enunciado). Puede hacerse en subsecciones. Responde a las preguntas *relevantes*.

**Cuatro bloques:**
1. **Viabilidad empresarial**: beneficios tangibles e intangibles, valor añadido, **coste** (etapas en cascada × perfiles × coste por hora + otros recursos), comparación con alternativas, cambios organizacionales, riesgos de negocio.
2. **Viabilidad técnica**: complejidad del conocimiento y del razonamiento (¿hay métodos?), aspectos críticos (tiempo, calidad, recursos), **criterios de éxito medibles y validación**, complejidad de las interfaces, integración con otros sistemas, riesgos tecnológicos.
3. **Viabilidad del proyecto**: compromiso del personal, recursos, disponibilidad del conocimiento, **expectativas realistas**, organización del proyecto, riesgos.
4. **Acciones propuestas**: **en esta etapa solo las preguntas 1 y 2**: ¿qué área se recomienda? ¿Qué solución y qué tareas de OM-3 se automatizan?

**Cómo se ha calculado el coste (para defenderlo con números):**
- Salarios brutos de referencia en España en 2026 (guías salariales; rangos orientativos: júnior 22–32 k€, sénior 40–65 k€, ingeniero de IA de nivel medio en el mercado local 45–75 k€): ingeniero de conocimiento **45.000 €**, sénior **48.000 €**, júnior **26.000 €**.
- **Coste empresa = bruto × 1,32**. La cotización empresarial en 2026 es de ≈30,65 % con contrato indefinido: contingencias comunes 23,60 % + desempleo 5,50 % + FOGASA 0,20 % + formación profesional 0,60 % + MEI 0,75 %; a eso se suma el tipo de accidentes de trabajo, que depende de la actividad (≈1–1,5 %).
- **1.760 h/año** (220 días × 8 h).
- Coste por hora: ingeniero de conocimiento 33,75 €/h, sénior 36,00 €/h, júnior 19,50 €/h.
- Etapas (32 semanas): análisis 3, modelado conceptual 10 (la más larga, porque incluye la **adquisición de conocimiento**, el cuello de botella clásico), diseño 4, implementación 10, pruebas 5.
- Personal: **66.840 €**. Más expertos internos (≈4.700 €, **coste de oportunidad**), publicaciones y datos (≈2.000 €), infraestructura (≈1.000 €), formación (≈1.000 €) y contingencia del 10 % → **≈83.100 €**. Mantenimiento ≈6.000 €/año.
- Beneficio (con **hipótesis explícitas**): ≈104 k€/año en el escenario previsto, ≈52 k€/año en el conservador → recuperación en < 1 año / < 2 años.

> ⚠️ **Hipótesis que tenéis que poder defender o cambiar**: 400 envíos/día, 5 % de envíos con consulta, 0,5 % con retenciones a 150 € de media y 2 M€/año en fletes consolidados. Son inventadas, pero **coherentes con el perfil de OM-1**. Si os preguntan "¿de dónde salen?": *"Es una empresa ficticia; son hipótesis de trabajo que en un proyecto real se validarían con los datos de la empresa durante el estudio de contexto, y por eso hay un escenario conservador."*

**Ideas de clase usadas en OM-5:**
- *"Fijar un nivel aceptable de rendimiento es importante… cómo lo vamos a evaluar y por qué es aceptable."* → Criterios de éxito con números (≥95 % de coincidencia, **0 falsos "admisible"**, ≥50 % menos consultas, < 5 min por admisión).
- **Especificar bien los objetivos** (los ejemplos de clase del coche que da vueltas para sumar puntos, los tuits tóxicos y el sistema que sale del *sandbox*): si le dices al sistema "maximiza la ocupación", puede juntar mercancías incompatibles. **Por eso en OM-5 la seguridad son restricciones duras y la ocupación solo se optimiza entre las soluciones que cumplen todas.** Este argumento suele gustar porque conecta directamente con lo explicado en clase.
- *"Lo más sencillo es siempre lo mejor"* → la primera versión se integra mediante ficheros, no con API, y el alcance está acotado.
- Puede ser necesario **formar** al personal o **crear tareas nuevas** (conclusión típica de un modelo de contexto).
- **Supervisión humana**: el SBC recomienda; el personal cualificado valida y firma (requisito legal y del AI Act).

**Formato del OM-5 en el LaTeX (igual que el ejemplo de Horarios):** una tabla `longtable` de dos columnas, con la cabecera «Modelo de Organización | Formulario OM-5: Análisis de viabilidad» repetida en cada página y, a la izquierda, los cuatro bloques de la plantilla: **Viabilidad Empresarial, Viabilidad Técnica, Viabilidad del Proyecto y Acciones Propuestas**. El texto sigue siendo narrativo. Cada bloque se reparte en varias filas sin línea entre ellas, porque `longtable` solo puede cortar entre filas y una fila no puede ocupar más de una página.

### Material de OM-5 retirado del LaTeX (por el límite de 22 páginas)
Lo que sigue estaba en el OM-5 y se quitó o se resumió al pasarlo a tabla. Os sirve para defender los números o para volver a meterlo si sobra espacio.

**Dedicación de cada perfil por etapa** (de aquí salen las horas de la tabla de costes, con jornadas de 40 h/semana):
- **Análisis del contexto** (3 semanas): ingeniero de conocimiento a tiempo completo (modelos de organización, tareas y agentes).
- **Modelado conceptual** (10 semanas): ingeniero de conocimiento a tiempo completo + programador sénior al 25 % (para conocer el modelo que tendrá que diseñar).
- **Diseño del software** (4 semanas): sénior a tiempo completo, ingeniero de conocimiento al 50 %, júnior al 25 %.
- **Implementación** (10 semanas): los dos programadores a tiempo completo + ingeniero de conocimiento al 25 % (dudas sobre la base de conocimiento).
- **Pruebas** (5 semanas): júnior a tiempo completo, sénior e ingeniero de conocimiento al 50 % (el ingeniero de conocimiento dirige la validación con los expertos).

**Desglose de otros costes:** tiempo de los expertos ≈ 4.700 € = **80 h** del técnico de mercancías peligrosas + **40 h** del responsable de aduanas + **40 h** del responsable de tráfico + **20 h** de operarios como usuarios de prueba (coste de oportunidad, no gasto nuevo); publicaciones y datos ≈ 2.000 € (IMDG e IATA DGR son de pago, el ADR es gratuito); infraestructura ≈ 1.000 €; formación ≈ 1.000 €; **contingencia del 10 % ≈ 7.550 €** → total ≈ 83.100 €.

**Diagrama de Gantt** (etapas en cascada, 32 semanas). Código TikZ para volver a ponerlo dentro de la celda de «Coste esperado» (con `x=0.25cm` para que quepa en la columna de 12,6 cm):
```latex
{\centering
\begin{tikzpicture}[x=0.25cm, y=0.55cm, font=\scriptsize]
\foreach \nombre/\ini/\fin/\fila in {Análisis del contexto/0/3/1, Modelado conceptual/3/13/2, Diseño del software/13/17/3, Implementación/17/27/4, Pruebas/27/32/5} {
  \node[anchor=east] at (-0.3,-\fila) {\nombre};
  \fill[gray!45] (\ini,-\fila-0.3) rectangle (\fin,-\fila+0.3);
}
\draw[-{Stealth}] (0,-5.8) -- (33,-5.8) node[right] {semanas};
\foreach \s in {0,4,...,32} {
  \draw (\s,-5.7) -- (\s,-5.9) node[below] {\s};
}
\end{tikzpicture}\par}
```
| Etapa | Semanas | Inicio → fin |
|---|---|---|
| Análisis del contexto | 3 | 0 → 3 |
| Modelado conceptual | 10 | 3 → 13 |
| Diseño del software | 4 | 13 → 17 |
| Implementación | 10 | 17 → 27 |
| Pruebas | 5 | 27 → 32 |

**Por qué T1, T5 y T8 no se automatizan (pregunta segura):** no son intensivas en conocimiento (OM-3) y sus activos (K1, K7, K12) tienen pocos cuellos de botella o ninguno (OM-4). Se resuelven con software convencional o se benefician indirectamente de la salida del SBC.
**Por qué T9 no:** importancia media; su activo K13 aún no está estructurado; se deja para otra vuelta de la espiral.
**Por qué T6 solo en parte:** la parte normativa (qué modos están permitidos) es conocimiento formal y crítico → la automatiza el SBC; la elección final entre los permitidos depende de factores comerciales y de relación con los transportistas (K9, experiencia tácita, cambiante) → **la mantiene el responsable de tráfico**.

## TM-1 — Análisis de tareas (PENDIENTE: una por tarea elegida: T2, T3, T4, T6, T7)

Vista doble de la tarea (Tema 2): la **vista de sistemas de información** (funciones y flujo, objetos/datos, tiempo y control) y la **vista de dirección** (agentes, conocimiento y competencias, recursos, rendimiento y calidad, meta y valor).

| Campo | Qué poner | Referencia |
|---|---|---|
| Tarea | Nº y nombre | OM-3 |
| Organización | Proceso del que forma parte y dónde se realiza | OM-3 |
| Objetivo y valor | Para qué sirve y qué valor añade al proceso | — |
| Dependencia y flujos | Tareas **precedentes** (le dan entradas) y **siguientes** (usan sus salidas) | OM-3 |
| Objetos manipulados | Entrada, salida e **internos** (información **y** conocimiento) | — |
| Tiempo y control | Frecuencia y duración; control respecto a otras tareas; **restricciones: pre y poscondiciones** | — |
| Agentes | Quién la ejecuta | OM-2, OM-3 |
| Conocimiento y capacidad | Capacidades necesarias, qué parte es intensiva en conocimiento, qué capacidades aporta a la organización; los elementos concretos van en TM-2 | OM-4 |
| Recursos | Refinamiento de OM-2, cuantificado si se puede | OM-2 |
| Calidad y eficiencia | **Medidas que usa la organización HOY** para saber si la tarea sale bien | — |

⚠️ **Situación actual** (antes del SBC): describid cómo se hace **hoy**. Ejemplo UniLever: "la tarea **debe** realizarse para cada producto nuevo"; frecuencia "docenas de veces al año"; restricciones "requisitos legales nacionales"; calidad "ISO 9000".
⚠️ Lección del ejemplo de Horarios (contada en clase): pusieron como objetivo "cumplir el **mayor número** de preferencias" y estaba mal (el sistema satisfacía muchas de unos y ninguna de otros); tuvieron que cambiarlo a "maximizar la **media**" de preferencias satisfechas por profesor. → **Redactad el objetivo con cuidado** (aplicado a nosotros: "maximizar la ocupación **sujeto a** cumplir toda la segregación").

**Pistas para nuestras TM-1 (situación actual):**
- **T2** — Precedente: T1. Siguiente: T4. Entrada: paquete registrado, descripción del contenido, ficha de datos de seguridad si existe. Salida: identificación del riesgo (¿es mercancía peligrosa? clase, nº ONU, grupo de embalaje, cantidad). Interno: consulta al técnico. Frecuencia: en cada paquete con contenido "sospechoso" (¿cuántos al día?). Control: si hay duda → el técnico. Calidad hoy: ¿cuántas mercancías peligrosas no detectadas aparecen después?
- **T3** — Entrada: destino, tránsitos, contenido, valor. Salida: restricciones o prohibiciones aplicables, documentación aduanera requerida. Restricción: sanciones/embargos vigentes.
- **T4** — Precedentes: T2 y T3. Siguientes: T5 (si se admite) o el rechazo/aviso al cliente. Salida: decisión (admitido / admitido con condiciones / rechazado) + condiciones. Poscondición: todo paquete admitido tiene definidas sus condiciones de embalaje, etiquetado y modos permitidos.
- **T6** — Entrada: paquetes admitidos con sus modos permitidos, plazo pedido, tarifas. Salida: modo y transportista. Control: horarios de cierre de los vuelos, buques y camiones.
- **T7** — Entrada: paquetes de un mismo modo y ruta, sus clases y propiedades (peso, volumen, apilable, temperatura, ¿alimento?), unidades de carga disponibles. Salida: plan de agrupación (qué va en cada unidad). Restricción dura: segregación. Objetivo: ocupación.

## TM-2 — Elementos de conocimiento y cuellos de botella (PENDIENTE)

Uno por cada elemento de conocimiento relevante de las tareas elegidas (candidatos: **K2, K3, K4, K6, K9, K10, K11**; podéis agrupar si lo justificáis).

**Estructura:** Nombre (OM-3), Poseído por (OM-4), Usado en (OM-3), Dominio + tres bloques de **sí/no** con una columna **"¿Supone un cuello de botella? ¿Debe ser mejorado?"**:
- **Naturaleza**: formal y riguroso · empírico y cuantitativo · heurístico y de sentido común · altamente especializado y específico del dominio · basado en la experiencia · basado en la acción (*habilidad física repetida: conducir, encestar*) · incompleto · incierto, puede ser incorrecto · cambia con rapidez · difícil de verificar · tácito y difícil de transferir.
- **Forma**: mental · papel · electrónica · habilidades · otros.
- **Disponibilidad**: limitaciones de tiempo · espacio · acceso · calidad · forma.

⚠️ **Nota del enunciado (importantísima):** la última columna son **DOS preguntas** que se responden **por separado**:
1. **¿Cuello de botella?** = ¿esa característica impide hacer bien la tarea?
2. **¿Debe/puede ser mejorado?** = ¿puede superarse con el SBC o no?
…y **no basta con sí/no: hay que añadir una breve explicación** (UniLever: *"Sí, pero no puede mejorarse en el SBC a priori"*; *"Sí, paliable con el SBC"*; *"Sí, se explicita en el SBC"*).

**Pistas:**
- **K2 (normativa)**: formal y riguroso **Sí**; altamente especializado **Sí**; cambia con rapidez **Sí** (cuello de botella sí → mejorable: base de conocimiento versionada); forma papel/electrónica (PDF); limitación de forma **Sí** (no operativa para consultar rápido) → mejorable.
- **K3 (experiencia del técnico)**: heurístico **Sí**; basado en la experiencia **Sí**; tácito **Sí** (cuello de botella sí; mejorable en parte: se extrae y se explicita en reglas, pero los casos nuevos siempre necesitarán al experto); mental **Sí**; limitación de tiempo y de espacio **Sí** → mejorable.
- **K10 (segregación)**: formal (las tablas) + heurístico (criterios internos) → explicad la mezcla; incompleto **Sí** (para la mercancía no peligrosa) → mejorable completándolo con los expertos.
- **K11 (estiba)**: basado en la experiencia y, en parte, **basado en la acción** (habilidad de los operarios para cargar físicamente): el SBC **no** puede mejorar la habilidad física, sí el plan. ← Buen ejemplo de "cuello de botella: sí; ¿mejorable con el SBC?: no".

## AM-1 — Agentes (PENDIENTE)

Uno por agente implicado en las tareas elegidas: **Operario de recepción y admisión, Técnico de mercancías peligrosas, Responsable de aduanas, Responsable de tráfico** (y quizá el Director de operaciones; el programa de gestión puede aparecer como agente de tipo **sistema de información**).

| Campo | Qué poner |
|---|---|
| Nombre | Rol |
| Organización | Posición en la estructura (OM-2) y **tipo** (humano / sistema de información) |
| Implicado en | Tareas (TM-1) |
| Se comunica con | Otros agentes |
| Conocimiento | Elementos que **posee** (TM-2) |
| Otras competencias | Competencias no intensivas en conocimiento (idiomas, trato con el cliente…) |
| Responsabilidades y restricciones | Qué debe hacer y **límites de autoridad, normas legales o profesionales** |

**Por qué existe (Tema 2):** OM y TM ven la organización desde las tareas; **AM la ve desde cada agente**, para juzgar responsabilidades, capacidades, **impacto humano** y cambios organizativos. Reorganiza información de otros formularios y es **entrada del modelo de comunicación**.
**Ejemplo del Tema 2 ("asignador de residencias")**: responsabilidades "asegurarse de que la gente se trata igualitariamente (no hay favores)". → Para nosotros, en responsabilidades y restricciones: el técnico/consejero de seguridad **responde legalmente**; el operario **no puede** admitir mercancía peligrosa sin la validación del técnico; el expedidor (cliente) es responsable de declarar correctamente…

## OTA-1 — No se pide en prácticas
Se usa cuando hay **varias** soluciones y varios OM-5. Contenido: impactos y cambios en la organización (los seis aspectos de OM-2), impactos en tareas y agentes, actitudes y compromisos, y acciones propuestas. **Aun así**, parte de su espíritu (cambios organizativos, actitudes) ya está dentro de nuestro OM-5.

## Mapa de trazabilidad (la profesora lo comprueba)
```
OM-1 Pn/On ──► OM-2 (proceso) ──► OM-3 Tn ──► OM-4 Kn (forma/lugar/tiempo/calidad)
                                     │             │
                                     ▼             ▼
                         OM-5 elige T2,T3,T4,T6,T7 (las intensivas, con activos con "No")
                                     │
                     TM-1 (una por tarea elegida) ──► TM-2 (uno por Kn usado) ──► AM-1 (uno por agente)
```
**Comprobación rápida:** todo nombre de agente/tarea/activo debe escribirse **igual** en todos los formularios.

---

# PARTE IV — El dominio: ampliación de la idea

> Fuentes de la normativa citada al final. ⚠️ Los números de apartado concretos de los reglamentos conviene **verificarlos en la edición vigente** antes de citarlos en la memoria. En el LaTeX he evitado citar apartados concretos precisamente por eso.

## 5. El negocio: qué hace una empresa como la nuestra
- **Transitario / operador logístico**: no suele tener aviones ni barcos; **contrata** el transporte (aerolíneas, navieras, transportistas de carretera) y aporta valor organizando, **consolidando** (grupaje: juntar paquetes de varios clientes en una unidad) y documentando.
- **Unidades de carga**: camión/semirremolque (carretera), **contenedor** de 20'/40' (marítimo; FCL = contenedor completo, LCL = grupaje), **ULD** (*Unit Load Device*: contenedores y palés aeronáuticos, con contornos que dependen del avión).
- **Peso tasable o volumétrico** (clave para el "aprovechamiento del espacio"): se cobra por el mayor entre el peso real y el volumétrico. Convenciones habituales: **aéreo ≈ 1 m³ = 167 kg** (divisor 6.000 cm³/kg), **carretera ≈ 1 m³ = 333 kg**, **marítimo LCL ≈ 1 m³ = 1 t** ("W/M"). → Una mercancía ligera y voluminosa "ocupa" más de lo que pesa. Es un factor del modo de transporte (T6) y del plan de carga (T7).
- **Restricciones físicas al agrupar**: peso máximo y volumen de la unidad, **distribución del peso**, apilabilidad (no poner peso sobre lo frágil), orientación ("este lado arriba"), **temperatura controlada** (alimentos perecederos, fármacos), separación de olores (café y té absorben olores)…

## 6. Mercancías peligrosas (MMPP): el núcleo normativo

### 6.1 Un reglamento por modo (todos derivan de las Recomendaciones de la ONU, el "Libro Naranja")
| Modo | Reglamento | Actualización |
|---|---|---|
| Carretera | **ADR** (Acuerdo europeo) | Bienal (años impares). **ADR 2027 en vigor el 1/1/2027**, con transición hasta el 30/6/2027 |
| Ferrocarril | **RID** | Bienal, en paralelo al ADR |
| Vías navegables | **ADN** | Bienal |
| Marítimo | **Código IMDG** (OMI, convenio SOLAS) | Enmiendas bienales |
| Aéreo | **Instrucciones Técnicas de la OACI**; en la práctica, la **Reglamentación IATA DGR** | **Anual** (edición 67 = 2026) |

→ **Esto justifica P4** ("la normativa cambia constantemente") y el diseño **versionado** de la base de conocimiento.

### 6.2 Clases de peligro (comunes a todos los modos)
| Clase | Qué es | Ejemplos que puede traer un cliente |
|---|---|---|
| 1 | Explosivos | Pirotecnia, munición |
| 2 | Gases (2.1 inflamables, 2.2 no inflamables, 2.3 tóxicos) | **Aerosoles (UN1950)**, extintores, bombonas de camping |
| 3 | Líquidos inflamables | **Perfumes (UN1266)**, **pinturas (UN1263)**, alcohol, quitaesmaltes |
| 4 | Sólidos inflamables (4.1), combustión espontánea (4.2), desprenden gases inflamables con el agua (4.3) | Cerillas, algunas fórmulas en polvo |
| 5 | Comburentes (5.1), peróxidos orgánicos (5.2) | Peróxido de hidrógeno concentrado, fertilizantes con nitratos, cloro para piscinas |
| 6 | Tóxicas (6.1), infecciosas (6.2) | Plaguicidas, muestras biológicas |
| 7 | Radiactivas | Fuentes de equipos médicos e industriales |
| 8 | **Corrosivas** | **Productos de limpieza** (lejías concentradas, desatascadores: p. ej. UN1719, UN1760), baterías de plomo-ácido |
| 9 | Varias | **Baterías de litio (UN3480/3481 ion; UN3090/3091 metal)**, hielo seco, imanes potentes (aéreo) |

Conceptos que un experto usa constantemente:
- **Número ONU** (4 cifras) + **designación oficial de transporte**.
- **Grupo de embalaje** I/II/III = peligro alto/medio/bajo → condiciona el **embalaje homologado ONU**.
- **Cantidades limitadas (LQ)** y **exceptuadas (EQ)**: por debajo de ciertas cantidades por envase, los requisitos se relajan mucho (marca específica, sin documentación completa en carretera…). **Muchos productos de limpieza y cosmética de consumo viajan como LQ.** → Es una heurística típica del experto ("¿esto va en LQ?").
- **Disposiciones especiales** y **variaciones de Estados y operadores** (aéreo): países y aerolíneas pueden ser **más restrictivos** que la norma general. → Aquí encajan "**las leyes de cada país**" dentro de la normativa de mercancías peligrosas.
- **Baterías de litio por avión** (caso estrella de O1): según la versión del método de embalaje (instrucciones de embalaje 965–970), hay límites de vatios-hora, de estado de carga (**≤ 30 % obligatorio para las baterías de ion litio sueltas, y desde la DGR 2026 también para las embaladas con el equipo (PI 966)**; para las instaladas en el equipo (PI 967) sigue siendo una recomendación) y algunas solo pueden ir en **avión de carga** ("Cargo Aircraft Only"). Es un ejemplo perfecto de conocimiento **formal, muy especializado y que cambia rápido**.

### 6.3 Segregación / compatibilidad (el corazón de P6 y T7)
- Cada reglamento tiene su **tabla de segregación** (clase contra clase) con distintos niveles: en el marítimo, términos como "*away from*", "*separated from*", "*separated by a complete compartment*"…; en carretera, la **prohibición de carga en común** de ciertas combinaciones en un mismo vehículo; en el aéreo, una tabla de **bultos que no pueden estibarse en contacto o adyacentes**.
- Ejemplos intuitivos de incompatibilidad: **comburentes (5.1) con inflamables (3)**: el comburente aporta oxígeno y aviva el fuego (vuestro ejemplo de "inflamables junto a inflamadores"); **ácidos con bases** (clase 8 contra clase 8: reaccionan entre sí; es una **segregación dentro de la misma clase** que las tablas por clase no capturan del todo → hace falta conocimiento experto); **reactivos con el agua (4.3) junto a líquidos**; **explosivos** con casi todo.
- **Alimentos**:
  - **Carretera (ADR)**: los alimentos y piensos **no deben cargarse con sustancias tóxicas (6.1), infecciosas (6.2) ni con ciertas mercancías de la clase 9**, salvo con precauciones (separaciones al menos tan altas como los bultos, separación con otros bultos, **distancia libre ≥ 0,8 m**, embalaje adicional).
  - **Marítimo (IMDG)**: hay una lista de clases que **no pueden compartir unidad de transporte** con alimentos y otra que exige **≥ 3 m de distancia**. *(Verificad qué clases están en cada lista en la enmienda vigente.)*
  - **Lo importante para defender el SBC**: un producto de limpieza **no clasificado como peligroso**, o que viaja como LQ, **puede no estar sujeto a ninguna separación legal de los alimentos**, pero **el sentido común y la política de calidad sí lo exigen** (un derrame contamina la comida). → Por eso K10 tiene una **parte formal** (las tablas) y una **parte no formal** (los criterios internos de la empresa), y por eso un software comercial que solo aplica las tablas legales **no basta** (argumento de OM-5).
- **Productos que absorben o desprenden olores**, **temperatura** (perecederos: acuerdo **ATP** en carretera; medicamentos: **Buenas Prácticas de Distribución**), **animales vivos** (IATA LAR): incompatibilidades no "peligrosas" pero reales.

### 6.4 Quién es responsable de qué (para AM-1 y para "responsabilidades y restricciones")
- **Expedidor/cargador (el cliente)**: clasificar, embalar, marcar, etiquetar y declarar correctamente. En el aéreo firma la **Declaración del expedidor de mercancías peligrosas**.
- **Si nuestra empresa embala por cuenta del cliente** (nuestro proceso 3 "Embalaje y etiquetado"), asume las obligaciones de **embalador** → por eso la calidad de K7 importa.
- **Transportista**: verificar, no aceptar lo que no cumple, documentación a bordo.
- **Consejero de seguridad (DGSA)**: figura **obligatoria** en las empresas que expiden, cargan o transportan MMPP por carretera, ferrocarril o vías navegables; en España, regulada por el **RD 97/2014**. → Lo he añadido en OM-2 como función del técnico de MMPP. **El SBC no sustituye esta figura** (argumento de expectativas realistas).
- **Formación obligatoria** del personal que interviene en el transporte de MMPP (todos los reglamentos la exigen; en el aéreo, con validez limitada y renovación periódica) → argumento para la **tarea nueva de formación** en OM-5.

### 6.5 Otras normas de carga y estiba
- **Código CTU** (OMI/OIT/CEPE): buenas prácticas para **arrumar** (cargar) contenedores y vehículos: distribución del peso, sujeción, compatibilidad.
- **VGM** (SOLAS, desde 2016): la **masa bruta verificada** del contenedor es obligatoria antes de embarcarlo.
- **Sujeción de la carga** en carretera (norma EN 12195-1 de cálculo de amarres).
- Aéreo: los límites de peso y forma de los ULD dependen del avión; el peso y el centrado los decide la aerolínea.

## 7. "Las leyes de cada país" (T3, K4)
- **UE / España**: **Código Aduanero de la Unión** (Reglamento (UE) 952/2013); **TARIC** (arancel integrado, con prohibiciones y restricciones por código de mercancía); **ICS2** (*Import Control System 2*: declaración sumaria de entrada con datos previos a la carga, especialmente en el aéreo); sistema de exportación (en España, el despacho electrónico).
- **Sanciones y embargos** (por ejemplo, las medidas restrictivas de la UE relacionadas con Rusia, Bielorrusia, Irán…): cambian con mucha frecuencia (K4: "tiempo No").
- **Productos de doble uso** (Reglamento (UE) 2021/821): ciertos bienes civiles con posible uso militar necesitan licencia de exportación.
- **CITES** (especies protegidas), controles **sanitarios y fitosanitarios** (alimentos, plantas, madera de embalaje → norma **NIMF-15** para palés y cajas de madera), **medicamentos**, **armas**…
- **Restricciones del país de destino**: cada país prohíbe o restringe cosas distintas. Algunos ejemplos (verificad los vigentes antes de usarlos): EE. UU. exige un **aviso previo** a la FDA para los alimentos; Australia y Nueva Zelanda tienen controles de **bioseguridad** muy estrictos; muchos países restringen los cigarrillos electrónicos, los medicamentos o las semillas.
- **Países de tránsito**: un envío puede verse afectado por las reglas del país por el que pasa (por ejemplo, las variaciones de Estado en el aéreo).
- **Incoterms 2020**: definen **quién paga y asume el riesgo** en cada tramo (EXW, FCA, DAP, DDP…) → afecta a quién gestiona la aduana.

## 8. Nuestro SBC visto en el nivel de conocimiento (adelanto del modelo conceptual)
```
ENTRADA: paquete (descripción, cantidades, ficha de datos de seguridad, peso/volumen, origen, destino, tránsitos, plazo deseado)

[Módulo de admisión — VALORACIÓN]
  T2 abstraer/clasificar: descripción → ¿mercancía peligrosa? → clase, nº ONU, grupo de embalaje, ¿LQ/EQ?
  T3 normas del destino: país × mercancía → prohibido / requisitos
  T4 decidir: normas (del modo + del país + de la empresa + del transportista) → ADMITIDO / ADMITIDO CON CONDICIONES / RECHAZADO / REMITIR AL EXPERTO
       + condiciones: embalaje, etiquetas, modos permitidos   + EXPLICACIÓN

[Módulo de agrupación — ASIGNACIÓN / CONFIGURACIÓN]
  T6 filtrar modos permitidos → ordenar por plazo/coste (el humano elige)
  T7 asignar paquetes → unidades de carga
       restricciones DURAS: segregación, alimentos, capacidad, peso, temperatura
       objetivo BLANDO: maximizar la ocupación (solo entre las soluciones válidas)
SALIDA: plan de agrupación + instrucciones de embalaje para el almacén (T5) + datos para la documentación (T8)
```
- **Conocimiento de dominio**: la lista de mercancías peligrosas, las tablas de segregación, las reglas por país y las reglas internas.
- **Conocimiento inferencial y de tareas** (genérico, reutilizable): las plantillas de valoración y asignación de la biblioteca de CommonKADS. En clase: *"el pseudocódigo genérico que me da igual si diagnostico un aeropuerto o un embarazo"*.

---

# PARTE V — Banco de preguntas para el grupo (respondedlas por escrito)

> Si alguna respuesta cambia algo, **actualizad el LaTeX** y la trazabilidad.

## A. Sobre la empresa (OM-1, OM-2)
- [ ] ¿La empresa es **real o ficticia**? Si es real, ¿cuál, y qué podemos decir de ella? Si es ficticia, ¿aceptamos el perfil que he puesto (60 empleados, 400 envíos/día, Galicia, transitario con grupaje)?
- [ ] ¿Qué **modos** usa realmente: aéreo, marítimo, carretera… ¿y ferrocarril? ¿Los tres desde el principio o solo uno en la primera versión?
- [ ] ¿Clientes B2B, B2C o ambos? ¿Comercio electrónico? (Afecta mucho a O1.)
- [ ] ¿La empresa **embala** ella misma o solo revisa lo que trae el cliente? (Cambia sus obligaciones legales.)
- [ ] ¿Tiene almacén propio con zona de mercancías peligrosas? ¿Gestiona perecederos o temperatura controlada?
- [ ] ¿Cuáles son sus **principales destinos** (los que acotan T3)?
- [ ] ¿Qué **valores** la distinguen de la competencia? ¿La estrategia es "fiable y conforme" o "barata y rápida"? (Cambia el peso de cada beneficio en OM-5.)
- [ ] ¿Hay algún **factor externo** reciente que lo cambie todo? (Nueva normativa, una sanción recibida, un incidente…)
- [ ] ¿Qué pasa hoy cuando se detecta tarde una mercancía peligrosa no declarada? ¿Quién paga?
- [ ] ¿Se ha intentado ya alguna solución (una hoja Excel, una lista de comprobación, un software)? Si sí, ¿por qué falló? (OM-5 pide los "intentos anteriores".)

## B. Sobre los problemas y oportunidades (OM-1)
- [ ] ¿Cada Pn es un **problema de la empresa** observable (no "nos gustaría usar IA")?
- [ ] ¿O1 y O2 son oportunidades **para la empresa**? ¿Os convence O1 (captar el comercio electrónico con mercancías peligrosas de uso cotidiano)? Alternativas: "entrar en un nuevo modo de transporte", "ofrecer un servicio de consultoría normativa"…
- [ ] ¿Falta algún problema? (¿Errores en la documentación aduanera? ¿Rotación del personal temporal?)
- [ ] ¿La **única** solución de OM-1 cubre **todos** los problemas y oportunidades? (Comprobad que cada Pn/On aparece entre los paréntesis de alguna capacidad.)
- [ ] ¿Sabríais explicar por qué una sola solución y no una por problema? (1 solución → 1 OM-5; varias soluciones → varios OM-5 + OTA-1.)

## C. Sobre el proceso y las tareas (OM-2, OM-3)
- [ ] ¿El orden de las tareas es el real? ¿T2 y T3 van **en paralelo** o en serie?
- [ ] ¿T3 (normativa del destino) ocurre **antes** de admitir o después? (Si es después, T4 no puede usar K4.)
- [ ] ¿Quién decide **de verdad** la admisión hoy: el operario o el técnico?
- [ ] ¿Hay alguna tarea que falte? (Por ejemplo, "pesaje y medición", "cotización al cliente", "reserva de espacio con el transportista".)
- [ ] ¿El criterio de importancia (Alta/Media/Baja) os parece bien o preferís una escala 1–5 con definición?
- [ ] ¿Podéis justificar por qué **T5 no es intensiva** aunque sea importante?
- [ ] ¿Y T8? (Una declaración de mercancías peligrosas sí requiere saber cosas…) → posible respuesta: el contenido de la declaración **sale de T2/T4**; T8 solo lo transcribe a la plantilla.

## D. Sobre los activos de conocimiento (OM-4)
- [ ] Para cada Kn: ¿por qué **ese** sí/no? ¿Sabríais explicarlo en una frase sin leer?
- [ ] ¿Hay activos que **debieran separarse**? (K2 junta tres reglamentos; ¿preferís K2a/K2b/K2c por modo?)
- [ ] ¿Hay activos que **debieran unirse**? (¿K9 y K11 son realmente distintos?)
- [ ] ¿Quién **posee** K10: el técnico (que conoce las tablas) o tráfico (que lo aplica)? Lo he puesto en el técnico precisamente para generar el "Lugar: No"; ¿estáis de acuerdo?
- [ ] ¿Hay conocimiento **basado en la acción** (habilidad física)? → la estiba manual. ¿Lo reflejamos en TM-2?

## E. Sobre la viabilidad (OM-5)
- [ ] ¿Aceptáis las hipótesis numéricas (5 % de consultas, 0,5 % de retenciones, 150 €, 2 M€ en fletes)? ¿Las podéis defender?
- [ ] ¿Los salarios (45k / 48k / 26k) os parecen razonables para Galicia? ¿Preferís citar una guía salarial concreta en la memoria?
- [ ] ¿Las duraciones (32 semanas) son coherentes? ¿Por qué el modelado conceptual es la etapa más larga? (Respuesta: adquisición de conocimiento = cuello de botella.)
- [ ] ¿Qué criterio de éxito es **innegociable**? (Propuesta: **0 falsos "admisible"** y 0 agrupaciones incompatibles.) ¿Por qué un falso negativo es peor que un falso positivo aquí?
- [ ] ¿Qué pasa si el cliente **miente** en la declaración? ¿El SBC se hace responsable? (No: responsabilidad legal del expedidor; el SBC solo señala descripciones sospechosas.)
- [ ] ¿Qué alternativas comerciales conocéis y qué les falta? (Validadores de la declaración aérea como IATA DG AutoCheck; comprobadores de segregación marítima como Hazcheck; planificadores de carga.)
- [ ] ¿Cuándo habría que **reconsiderar** la decisión? (Pregunta 5 de las acciones propuestas, opcional: si cambia mucho la normativa, si la empresa deja un modo, si el volumen de mercancías peligrosas es insignificante, si aparece un producto comercial integrado…)
- [ ] **Alcance**: ¿automatizamos T2, T3, T4, T6 y T7 (lo que pone OM-5) o reducimos a **T2 + T4 + T7** para que el modelo conceptual sea manejable? (Recomendación: mantener las cinco en el contexto, pero en el modelo conceptual centrarse en **una valoración + una asignación**.)

## F. Sobre TM-1 / TM-2 / AM-1 (pendientes)
- [ ] Para cada tarea elegida: ¿frecuencia y duración **reales** hoy?
- [ ] ¿Qué **medidas de calidad** usa hoy la empresa para saber si la tarea sale bien? (Si no usa ninguna, decidlo: también es información.)
- [ ] ¿Qué objetos **internos** maneja cada tarea?
- [ ] ¿Redactáis el objetivo de T7 de forma que **no se pueda "hacer trampa"**? (Ocupación **sujeta a** la segregación.)
- [ ] En TM-2, ¿habéis separado **"¿cuello de botella?"** de **"¿mejorable con el SBC?"** y explicado cada uno?
- [ ] En AM-1, ¿responsabilidades y **restricciones de autoridad** (quién puede admitir mercancías peligrosas, quién firma) están claras?

## G. Preguntas que os puede hacer la profesora (practicad la respuesta)
1. *¿Por qué esto necesita un SBC y no un programa normal con tablas?* → Porque además de las tablas formales hay **conocimiento heurístico y tácito** (reconocer mercancías peligrosas no declaradas, la segregación dentro de una misma clase, los criterios internos para mercancía no peligrosa, la elección del modo), se necesita **explicar** cada decisión y la base debe **mantenerse separada** del razonamiento porque cambia cada año.
2. *¿Cuál es vuestro experto?* → El técnico de mercancías peligrosas, que actúa como consejero de seguridad (y, para T3, el responsable de aduanas). Fuentes formales: los reglamentos (el ADR es público y gratuito).
3. *¿Por qué esta oportunidad es de la empresa?* → Ver O1/O2: más clientes y un activo propio; derivan de su estrategia y de factores externos.
4. *¿Por qué T5 no se automatiza si es donde se aplican las normas?* → No es intensiva: el operario ejecuta instrucciones; el conocimiento está en quien **genera** esas instrucciones (T4, que sí se automatiza).
5. *¿Qué diferencia hay entre OM-4 y TM-2?* → OM-4: visión **organizativa** del activo (¿está en la forma, el lugar, el tiempo y con la calidad correctos?). TM-2: **caracterización detallada** del elemento de conocimiento (naturaleza, forma, disponibilidad) y sus cuellos de botella **para una tarea concreta**.
6. *¿Por qué hacéis el modelo de contexto "antes del SBC"?* → Metodología en espiral; esta es la primera vuelta y describe la situación actual para poder **comparar** después.
7. *¿Cómo evaluaréis el sistema?* → Criterios de OM-5 + validación con expertos + revisión de las explicaciones.
8. *¿Qué pasa si el sistema se equivoca?* → Diseño conservador, supervisión humana, responsabilidad legal sin cambios, trazabilidad para auditar.

---

# PARTE VI — Qué he cambiado y por qué (para que no os pille por sorpresa)

| Formulario | Cambio | Motivo |
|---|---|---|
| OM-1 | Rellenado **O1** (antes vacío) y añadido **O2** | El "O1." estaba vacío; la profesora criticó una oportunidad que lo era para el grupo y no para la empresa |
| OM-1 | Añadido **P6** (agrupación compatible + espacio) | Era vuestra idea central y **no aparecía en OM-1**, aunque OM-3 tenía la T7 → la trazabilidad estaba rota |
| OM-1 | Añadido el **perfil de la empresa** en el contexto | El contexto no decía qué empresa es ni su tamaño; OM-5 lo necesita para los números |
| OM-1 | Factores externos concretados (reglamentos, sanciones, restricciones de los transportistas) | Más específico y coherente con P4 y OM-4 |
| OM-1 | Gramática de la "Solución" ("Un SBC que con conocimiento…", "adapterse") | Erratas |
| OM-1 | **Soluciones reducidas a UNA** (un SBC con tres capacidades que cubren P1–P6, O1 y O2); alternativas trasladadas a OM-5 | Corrección de la profesora: como máximo 2 soluciones, idealmente 1. Cada solución implicaría su propio OM-5 |
| OM-5 | Frase de beneficios de P2 ajustada ("fija desde la admisión los requisitos…") | Coherencia con la solución única |
| OM-2 | Técnico de MMPP = **consejero de seguridad** | Figura legal real; refuerza "responsabilidades" en AM-1 |
| OM-2 | Recursos → "Tecnología, patentes" | Antes contenía contratos, que no son tecnología; añadidas las publicaciones normativas |
| OM-3 | Nombres unificados con OM-2; "Apartamento" → "Mercancías peligrosas" | Coherencia (los agentes y los lugares deben coincidir con OM-2) |
| OM-3 | Recursos K1–K13 con **(F)/(NF)** + leyenda | El enunciado exige distinguir lo formal de lo no formal; trazabilidad con OM-4 |
| OM-3 | **Criterio de importancia** definido; T7 → Alta | El enunciado pide un criterio definido por el ingeniero de conocimiento |
| OM-3 | T4: "Resultados de las tareas previas" → K2, K3, K4, K6 | Un resultado es un objeto de entrada, no un recurso de conocimiento |
| OM-4 | Redactado entero (13 activos, comentarios numerados) | Pendiente |
| OM-5 | Redactado entero en forma narrativa, con costes, Gantt y acciones 1 y 2 | Pendiente |
| LaTeX | OM-1 y OM-3 pasan de `table`+`tabularx` a `longtable`; `\raggedbottom` | OM-1 ya no cabía en una página (se salía) |
| OM-5 | Pasado a **tabla** (como el ejemplo de Horarios), con los bloques Viabilidad Empresarial / Técnica / del Proyecto / Acciones Propuestas; texto resumido; Gantt y desgloses retirados (guardados en la Parte III, OM-5) | Seguir el formato del ejemplo y no pasar de 22 páginas |
| LaTeX | Eliminado el texto del enunciado: «Indicaciones para la realización…», los «COMENTARIO (borrar…)», las descripciones de las casillas de TM-1, TM-2 y AM-1 (quedan las tablas vacías) y la sección 3 con OTA-1 | Límite de 22 páginas (el documento pasa de 29 a 22). OTA-1 no se pide; OM-5 lo dice en «Acciones propuestas» |

**Sin tocar**: la estructura y los procesos de OM-2, la cultura y el potencial, la portada (sigue con "…(tema de la práctica)", el grupo y el horario por rellenar; sugerencia de título: *"Un SBC para la admisión y consolidación segura de envíos multimodales"*), y las plantillas TM/AM/OTA.

---

# Glosario rápido
- **Activo de conocimiento**: conocimiento que la organización usa en sus tareas, tratado como un recurso (OM-4).
- **Cuello de botella del conocimiento**: característica del conocimiento que impide hacer bien una tarea (TM-2).
- **Intensiva en conocimiento**: tarea que requiere razonar con conocimiento especializado.
- **Conocimiento tácito**: el que está en la mente del experto y es difícil de verbalizar o transferir.
- **Espiral (ciclo de vida)**: desarrollo en ciclos guiados por el riesgo; el contexto se rehace en cada ciclo.
- **Nivel de conocimiento**: describir un sistema por lo que sabe, sus objetivos y su racionalidad, no por su código.
- **Nº ONU / grupo de embalaje / LQ-EQ / segregación / ULD / LCL / VGM / DGSA**: ver la Parte IV.

# Fuentes
**De la asignatura**: Tema 1 (Introducción y CommonKADS); Tema 2 (Modelos de contexto); enunciado de la práctica; presentación del tema de prácticas (B. Guijarro); transcripciones de las clases 2, 3 y 4; ejemplo de PFC de Horarios. Bibliografía base: Schreiber et al., *Knowledge Engineering and Management. The CommonKADS Methodology*, MIT Press, 2000; Alonso Betanzos et al., *Ingeniería del conocimiento. Aspectos metodológicos*, Pearson, 2004.

**Consultadas para el dominio y los costes (octubre de 2026):**
- Salarios: [oie.es — sueldo programador 2026](https://www.oie.es/sueldo-programador-espana/), [jobrise — Software Engineer España 2026](https://jobrise.io/es/blog/cuanto-gana-software-engineer-espana-2026/), [calculadornomina — programador sénior 2026](https://calculadornomina.es/salarios/sueldo-programador-senior-espana-2026/), [KeepCoding — ingeniero de IA](https://keepcoding.io/blog/cuanto-gana-un-ingeniero-de-ia/), [aicad — sueldo ingeniero IA España](https://www.aicad.es/el-sueldo-real-de-un-ingeniero-de-inteligencia-artificial-en-espana)
- Cotizaciones 2026: [Cuatrecasas — Orden de cotización 2026](https://www.cuatrecasas.com/es/spain/laboral/art/claves-orden-cotizacion-2026), [Asepeyo — bases y tipos 2026](https://www.asepeyo.es/wp-content/uploads/Bases-tipos-cotizacion-2026-ES.pdf)
- ADR 2027: [trans.info — ADR 2027 changes](https://trans.info/en/adr-2027-changes-507286), [Certifico — ADR 2027](https://www.certifico.com/merci-pericolose/legislazione-merci-pericolose/adr-2027/adr-2027-draft-amendments-to-annexes-a-and-b-of-adr-consolidated-list)
- IATA DGR 2026 y baterías de litio: [Lion — What's new in the 2026 IATA DGR](https://www.lion.com/lion-news/august-2025/what-s-new-in-the-2026-iata-dgr), [Lion — Cheat sheet SoC 2026](https://www.lion.com/getmedia/34a1639d-f193-4c04-90b2-acc50fb051af/Cheat-Sheet-012-Lithium_SOC_2026.pdf)
- Alimentos y mercancías peligrosas: [HSE (UK) — carriage of foodstuffs (ADR 7.5.4)](https://www.hse.gov.uk/cdg/commonproblems/foodstuffs.htm), [ICHCA — Info paper (IMDG y alimentos)](https://ichca.com/wp-content/uploads/2021/04/Infopaper60.pdf), [BAM — IMDG 7.2 segregation](https://www.dgg.bam.de/dgginfo/detailinfo/imdg/befjxaxacfyz?lang=en)
- Alternativas comerciales: [IATA — DG AutoCheck](https://www.iata.org/en/publications/newsletters/iata-knowledge-hub/automation-improves-the-safety-of-dangerous-goods-acceptance/), [Hazcheck Online](https://hazcheck.com/product/hazcheck-online/)
