# EVALUACIÓN DE IMPACTO EN PROTECCIÓN DE DATOS (EIPD)

## Tratamiento Específico: Análisis y Publicación de Informes sobre Threat Actors a partir de Comunicaciones Maliciosas Recibidas

Este documento constituye la Evaluación de Impacto específica para el tratamiento de mayor riesgo del proyecto: el análisis de comunicaciones maliciosas recibidas y la publicación de informes de inteligencia de amenazas en este repositorio. Establece las bases legales, el modelo de publicación graduada y las medidas de protección que amparan cada informe aquí publicado. Complementa el [Aviso Legal](AVISO_LEGAL.md) del repositorio y la documentación legal completa del framework en [CorvusEngine](https://github.com/lacrimae0rerum/CorvusEngine/tree/main/docs/legal).

| Campo | Valor |
|---|---|
| **Responsable del tratamiento** | [Nombre / Razón social] |
| **Fecha de elaboración** | Marzo 2026 |
| **Versión** | 1.0 |
| **Revisión prevista** | Septiembre 2026 |
| **Elaborado por** | [Nombre] |
| **Clasificación** | USO INTERNO — CONFIDENCIAL |
| **Tratamiento evaluado** | Recogida, análisis, correlación y publicación de datos relativos a threat actors e infraestructura maliciosa a partir de comunicaciones no solicitadas (phishing, smishing, vishing, mensajes en RRSS) |

---

## 1. Descripción del Tratamiento

### 1.1. Contexto y flujo de datos

Este tratamiento tiene una particularidad jurídicamente relevante: **el origen de los datos es una comunicación maliciosa no solicitada dirigida contra el responsable del tratamiento**. Es decir, el responsable es primero víctima (o target) de un intento de ataque, y a partir de ese hecho inicia una investigación legítima de la amenaza.

El flujo completo es el siguiente:

```
ENTRADA (pasiva, no solicitada)
│
├─ Email de phishing/spam → cabeceras, cuerpo, adjuntos, URLs, remitente
├─ SMS malicioso (smishing) → número emisor, cuerpo, URLs
├─ Llamada vishing → número emisor, grabación (si legal), contenido
└─ Mensaje en RRSS → perfil emisor, contenido, enlaces
│
▼
ANÁLISIS (activo, sobre datos recibidos + OSINT pasivo)
│
├─ Extracción de IoCs del mensaje original
├─ Análisis de adjuntos/payloads en sandbox
├─ Resolución de infraestructura (DNS, WHOIS, SSL, hosting)
├─ Correlación con threat feeds y bases de datos CTI
├─ Pivoting sobre infraestructura para mapear campaña
├─ Investigación de registrantes, operadores, actores
└─ Atribución (cuando los datos lo permitan)
│
▼
SALIDA (publicación activa)
│
├─ Informe técnico en repositorio GitHub público
├─ IoCs estructurados (STIX/JSON) para consumo comunitario
└─ Análisis narrativo con atribución (cuando proceda)
```

### 1.2. Datos de entrada: comunicaciones recibidas

| Canal | Datos recopilados | Naturaleza |
|---|---|---|
| **Email** | Cabeceras completas (Return-Path, Received, X-headers, DKIM, SPF, Message-ID), dirección de remitente, asunto, cuerpo (texto/HTML), adjuntos, URLs embebidas, metadatos MIME | Datos técnicos + posible dato personal (email del remitente, que puede ser spoofed o comprometido) |
| **SMS** | Número de teléfono emisor, cuerpo del mensaje, URLs, timestamps | Dato personal directo (número de teléfono) + datos técnicos |
| **Vishing** | Número de teléfono, contenido de la llamada, patrones de ingeniería social | Dato personal directo (número) + posibles datos biométricos si se graba voz |
| **RRSS** | Perfil del emisor (nombre, handle, avatar, bio), contenido del mensaje, enlaces, metadatos de la cuenta | Datos personales (si perfil real) o datos ficticios (si cuenta falsa/bot) |

### 1.3. Datos generados durante la investigación

| Fase | Datos generados | Naturaleza |
|---|---|---|
| **Extracción de IoCs** | Hashes de adjuntos, IPs de C2, dominios maliciosos, URLs de payload, reglas YARA | Técnicos — sin datos personales directos |
| **Análisis de infraestructura** | Registros DNS, datos WHOIS (registrante, email, organización, fechas), certificados SSL (emisor, SAN, organización), geolocalización IP, proveedor de hosting | Mixtos — pueden incluir datos personales del registrante |
| **Correlación CTI** | Vínculos con campañas conocidas, TTPs mapeadas a MITRE ATT&CK, conexiones con threat actors catalogados | Técnicos — datos personales solo si se vinculan a individuos |
| **Pivoting** | Dominios adicionales del mismo registrante, IPs del mismo bloque, infraestructura compartida, patrones de registro | Mixtos — el pivoting sobre registrante es donde más datos personales emergen |
| **Atribución** | Identidad probable del actor o grupo, handles/aliases, nacionalidad estimada, afiliación, historial de actividad | **Datos personales directos** si la atribución señala a persona física identificable |

### 1.4. Datos publicados en GitHub

| Elemento del informe | Contenido | Datos personales potenciales |
|---|---|---|
| **Resumen ejecutivo** | Descripción de la campaña, vector, objetivo | No (salvo mención del actor por alias) |
| **Análisis técnico** | Cadena de ataque, payloads, infraestructura, TTPs | Generalmente no — IPs y dominios no son datos personales per se |
| **IoCs estructurados** | Fichero STIX/JSON/CSV con indicadores | Pueden incluir emails, números de teléfono, datos WHOIS |
| **Infraestructura del actor** | Mapa de dominios, IPs, hosting, certificados | Datos WHOIS del registrante si no están redactados |
| **Atribución** | Identidad/alias del actor, evidencia de atribución | **Sí — dato personal directo si se identifica persona física** |
| **Muestras** | Hashes, reglas YARA/Sigma, screenshots | Potencialmente (screenshots pueden contener datos personales) |

---

## 2. Finalidades del Tratamiento

### 2.1. Finalidades declaradas

| ID | Finalidad | Justificación |
|---|---|---|
| F-01 | **Autodefensa y respuesta a amenaza** | Analizar un ataque recibido para proteger los propios sistemas y prevenir futuros intentos. Derecho legítimo de autodefensa digital. |
| F-02 | **Generación de inteligencia de amenazas** | Producir conocimiento accionable sobre TTPs, infraestructura y actores para contribuir a la seguridad colectiva del ecosistema. |
| F-03 | **Publicación de informes CTI** | Compartir hallazgos con la comunidad de seguridad para que otros puedan protegerse frente a las mismas amenazas. |
| F-04 | **Atribución de actores** | Identificar, cuando sea posible, al responsable del ataque para facilitar la persecución, disuasión o defensa informada. |

### 2.2. Finalidades excluidas

- Represalia, acoso o contacto directo con el actor identificado.
- Justicia por mano propia (hackback o acciones ofensivas).
- Uso comercial de los datos del actor (venta de datos, marketing).
- Elaboración de perfiles de personas no vinculadas a actividad maliciosa.
- Publicación de datos personales de víctimas del ataque.

---

## 3. Base Jurídica y Ponderación

### 3.1. Base jurídica principal: Interés legítimo (art. 6.1.f RGPD)

El tratamiento se fundamenta en el interés legítimo del responsable, reforzado por tres pilares:

**Pilar 1 — Autodefensa (Considerando 49 RGPD):** El responsable ha recibido una comunicación maliciosa dirigida contra sus sistemas o su persona. El análisis de esa comunicación y de la infraestructura detrás de ella es estrictamente necesario para garantizar la seguridad de la red y de la información. El Considerando 49 reconoce explícitamente este interés.

**Pilar 2 — Interés público en la seguridad colectiva:** La publicación de informes CTI contribuye a la protección del ecosistema digital. Organismos como ENISA, CCN-CERT e INCIBE fomentan activamente el intercambio de inteligencia de amenazas. La comunidad CTI opera sobre el principio de reciprocidad: compartir hallazgos protege a todos.

**Pilar 3 — Libertad de información y expresión (art. 11 CDFUE, art. 20 CE):** La publicación de investigaciones sobre actividades delictivas tiene un componente de interés informativo legítimo, análogo al periodismo de investigación. La identificación de actores maliciosos contribuye a la transparencia y la disuasión.

### 3.2. Bases jurídicas subsidiarias

| Supuesto | Base |
|---|---|
| Si se interpone denuncia penal con los datos recopilados | Art. 6.1.c) RGPD — Obligación legal (arts. 259, 262 LECrim) |
| Si se actúa bajo contrato con cliente que recibió el ataque | Art. 6.1.b) RGPD — Ejecución contractual |
| Si la autoridad competente solicita los datos | Art. 6.1.c) RGPD — Obligación legal |

### 3.3. Ponderación de interés legítimo

#### 3.3.1. Particularidad clave: el interesado es un atacante

Esta ponderación tiene una característica distintiva que refuerza significativamente la posición del responsable: **el interesado principal cuyos datos se tratan es el autor de una comunicación maliciosa dirigida contra el responsable**. Esta circunstancia altera radicalmente la ponderación estándar:

- El interesado ha iniciado una interacción no solicitada y potencialmente delictiva con el responsable.
- El interesado no tiene expectativa legítima de privacidad sobre la infraestructura utilizada para perpetrar un ataque.
- El interesado no puede invocar el principio de limitación de finalidad sobre datos que él mismo ha enviado al responsable en el contexto de un acto potencialmente ilícito.
- La jurisprudencia española reconoce que la expectativa de privacidad se reduce sustancialmente cuando el individuo actúa en un contexto delictivo (STS 342/2013, entre otras).

#### 3.3.2. Triple test

**Test de necesidad:**

| Pregunta | Respuesta |
|---|---|
| ¿Es necesario analizar la comunicación recibida? | **Sí.** Sin análisis, es imposible evaluar el alcance de la amenaza, prevenir ataques futuros y proteger los sistemas. |
| ¿Es necesaria la investigación OSINT sobre la infraestructura? | **Sí.** La comunicación maliciosa aislada no proporciona contexto suficiente. La correlación con fuentes abiertas es imprescindible para entender la campaña, su alcance y sus TTPs. |
| ¿Es necesaria la atribución? | **Parcialmente.** La atribución técnica (vincular infraestructura a un actor o grupo) es necesaria para la defensa informada. La atribución personal (nombrar a un individuo) solo es necesaria cuando hay evidencia sólida y aporta valor defensivo o facilita la persecución penal. |
| ¿Es necesaria la publicación en GitHub? | **Sí, para las finalidades F-02 y F-03.** La publicación de IoCs y TTPs permite a otros defensores protegerse. Alternativas (no publicar) dejan a la comunidad desprotegida frente a la misma amenaza. |
| ¿Existen alternativas menos intrusivas? | **La publicación puede graduarse:** IoCs técnicos sin atribución personal (menos intrusivo) vs. informe completo con atribución (más intrusivo pero más útil para la comunidad). Ver medidas de mitigación en §5. |

**Test de ponderación:**

| Factor | Análisis | Resultado |
|---|---|---|
| **Naturaleza de los datos** | Predominantemente técnicos. Los datos personales del actor son limitados (email, teléfono, datos WHOIS, alias). En caso de atribución, puede incluir nombre real. | Favorable al responsable |
| **Expectativa del interesado** | El actor ha enviado voluntariamente una comunicación maliciosa al responsable. Su expectativa de privacidad sobre los datos vinculados a esa actividad es mínima o nula. Sobre su identidad real, la expectativa es mayor pero reducida por el contexto delictivo. | Fuertemente favorable al responsable |
| **Impacto sobre el interesado** | La publicación de IoCs técnicos tiene impacto nulo sobre la persona. La publicación de atribución personal puede tener impacto reputacional significativo. Sin embargo, ese impacto es consecuencia directa de la actividad ilícita del propio interesado. | Favorable con matiz en atribución personal |
| **Medidas de protección** | Se implementan medidas de graduación: publicación por niveles, verificación antes de atribución, derecho de respuesta. | Favorable |
| **Interés del público** | La comunidad CTI y el público general tienen interés legítimo en conocer las amenazas activas, sus TTPs y sus responsables. Entidades como MITRE, MANDIANT, CrowdStrike y Microsoft publican regularmente atribuciones de actores. | Fuertemente favorable |

**Test de proporcionalidad:**

| Aspecto | Medida |
|---|---|
| Minimización en recogida | Solo se recopilan datos relevantes para entender la amenaza. No se investigan aspectos personales del actor no vinculados a su actividad maliciosa (familia, salud, etc.). |
| Minimización en publicación | Se aplica un modelo de publicación graduada (ver §5.1). Los datos personales solo se publican cuando son necesarios para la defensa y están suficientemente verificados. |
| Limitación temporal | Los datos se mantienen mientras la amenaza sea activa. Los informes publicados permanecen como referencia histórica (justificado por interés archivístico/investigador). |

#### 3.3.3. Resultado de la ponderación

**El interés legítimo del responsable PREVALECE** sobre los derechos del interesado para la totalidad de las finalidades declaradas (F-01 a F-04), sujeto a las medidas de mitigación de la sección 5.

La prevalencia es especialmente robusta por la combinación de: (a) el Considerando 49 RGPD, (b) la condición de atacante del interesado, (c) la contribución a la seguridad colectiva, y (d) el componente de libertad de información.

---

## 4. Riesgos Identificados

### 4.1. Matriz de riesgos

| ID | Riesgo | Prob. | Impacto | Nivel | Específico de este tratamiento |
|---|---|---|---|---|---|
| R-01 | **Atribución errónea:** publicar atribución incorrecta que señale a persona inocente | Media | Muy alto | **CRÍTICO** | Sí — riesgo principal |
| R-02 | **Datos de víctimas en el informe:** publicar inadvertidamente datos personales de víctimas del phishing/smishing contenidos en la evidencia | Media | Alto | **ALTO** | Sí |
| R-03 | **Números de teléfono de terceros inocentes:** el número de SMS/vishing puede pertenecer a una línea comprometida o suplantada, no al actor real | Alta | Alto | **ALTO** | Sí |
| R-04 | **Emails spoofed atribuidos al remitente real:** publicar email de remitente que en realidad está siendo suplantado | Alta | Alto | **ALTO** | Sí |
| R-05 | **Datos WHOIS de registrantes legítimos:** dominios comprometidos cuyo WHOIS apunta al propietario legítimo, no al actor | Media | Alto | **ALTO** | Sí |
| R-06 | **Grabación de llamadas vishing sin consentimiento** | Media | Alto | **ALTO** | Sí — implicación penal directa |
| R-07 | **Represalias del actor:** el actor identificado toma represalias contra el investigador | Media | Alto | **ALTO** | Sí |
| R-08 | **Categorías especiales emergentes:** la investigación revela datos sensibles del actor (nacionalidad → origen étnico, afiliación a grupo → convicciones) | Baja | Alto | **MEDIO** | Parcial |
| R-09 | **Derecho al olvido/supresión:** el actor solicita eliminación del informe publicado | Media | Medio | **MEDIO** | Sí |
| R-10 | **Difamación / injurias:** el actor denunciado alega que el informe constituye injurias o calumnias | Baja | Alto | **MEDIO** | Sí |

### 4.2. Riesgo específico de la publicación en GitHub

GitHub es una plataforma pública, global e indexada por buscadores. Una vez publicado, el contenido es potencialmente permanente (forks, mirrors, caches, Wayback Machine). Esto amplifica tanto el beneficio (máxima difusión de IoCs) como el riesgo (imposibilidad práctica de supresión total si hay un error).

---

## 5. Medidas de Mitigación

### 5.1. Medida principal: Modelo de publicación graduada (TLP adaptado)

Se establece un modelo de tres niveles de publicación que gradúa la exposición de datos personales en función de la verificación y la necesidad:

```
NIVEL 1 — IoCs TÉCNICOS (publicación inmediata)
════════════════════════════════════════════════
✓ Publicar libremente:
   • Hashes de malware/payloads
   • Dominios maliciosos
   • IPs de C2/infraestructura
   • URLs de phishing
   • Reglas YARA/Sigma/Snort
   • TTPs mapeadas a MITRE ATT&CK
   • Headers de email (sanitizados)

✗ NO incluir:
   • Emails de remitente (pueden ser spoofed)
   • Números de teléfono (pueden ser comprometidos)
   • Datos WHOIS de registrante sin verificar
   • Datos de víctimas


NIVEL 2 — INFRAESTRUCTURA + REGISTRANTE (publicación tras verificación)
═══════════════════════════════════════════════════════════════════════
✓ Publicar tras verificar que el registrante es el actor (no víctima):
   • Datos WHOIS de dominios maliciosos registrados por el actor
   • Emails de contacto usados para registrar infraestructura
   • Patrones de registro (naming conventions, registradores usados)
   • Números de teléfono confirmados como pertenecientes al actor

⚠️ Verificación mínima requerida:
   • Cruce de al menos 2 fuentes independientes
   • Descartar suplantación/compromiso del titular legítimo
   • Documentar internamente la cadena de evidencia


NIVEL 3 — ATRIBUCIÓN PERSONAL (publicación excepcional)
═══════════════════════════════════════════════════════
✓ Publicar SOLO cuando se cumplan TODOS estos criterios:
   • Evidencia sólida de al menos 3 fuentes independientes
   • La atribución aporta valor defensivo concreto a la comunidad
   • Se ha descartado razonablemente la posibilidad de error
   • Se ha valorado el impacto sobre el interesado vs. el beneficio
   • Se ha documentado internamente toda la cadena de atribución
   • Revisión por un segundo analista antes de publicar

⚠️ Formato de atribución:
   • Preferir alias/handles sobre nombres reales cuando sea suficiente
   • Si se publica nombre real: incluir nivel de confianza (alta/media)
   • Nunca publicar dirección física, datos familiares o datos de salud
   • Incluir disclaimer de que la atribución se basa en análisis OSINT
```

### 5.2. Mitigación R-01: Atribución errónea

- **Regla de las tres fuentes:** Nunca publicar atribución personal basada en menos de tres fuentes independientes.
- **Peer review obligatorio:** Todo informe con atribución de Nivel 3 debe ser revisado por un segundo analista antes de publicación.
- **Niveles de confianza explícitos:** Cada atribución debe indicar su nivel de confianza siguiendo el Admiralty Code o escala equivalente.
- **Protocolo de corrección:** Si se detecta un error de atribución, retirar inmediatamente el dato incorrecto, publicar corrección y notificar a comunidades que hayan consumido el informe.
- **Cláusula de salvaguarda en cada informe:** Incluir disclaimer estándar (ver §5.10).

### 5.3. Mitigación R-02: Datos de víctimas

- **Sanitización obligatoria de evidencia:** Antes de publicar cualquier screenshot, muestra de email o SMS, eliminar o redactar todos los datos personales de víctimas (destinatarios del phishing, datos capturados por el atacante).
- **Checklist de sanitización pre-publicación** (ver Anexo B).
- **Prohibición absoluta** de publicar credenciales capturadas, datos bancarios de víctimas o cualquier información exfiltrada por el actor.

### 5.4. Mitigación R-03 y R-04: Números/emails de terceros inocentes

- **Presunción de suplantación:** Todo número de teléfono y email de remitente se presume suplantado o comprometido hasta que se demuestre lo contrario.
- **No publicar como Nivel 1:** Los números y emails del remitente NUNCA se publican en la capa de IoCs técnicos sin verificación.
- **Verificación de titularidad:** Antes de atribuir un número/email al actor, verificar mediante correlación con otras fuentes que el titular es efectivamente el actor y no una víctima de SIM swap, compromiso de cuenta o spoofing.

### 5.5. Mitigación R-05: WHOIS de registrantes legítimos

- **Distinguir dominios registrados por el actor vs. dominios comprometidos:** Un dominio comprometido tendrá WHOIS de su propietario legítimo, no del actor.
- **Indicadores de registro malicioso:** Registros recientes, uso de registradores bulk/baratos, datos WHOIS genéricos o falsos, patrones de DGA.
- **En caso de duda:** Publicar solo el dominio como IoC (Nivel 1), sin datos WHOIS.

### 5.6. Mitigación R-06: Grabación de llamadas vishing

- **Marco legal en España:** La grabación de una conversación telefónica por uno de los participantes es legal sin necesidad de consentimiento del otro interlocutor (STS 248/2012, STC 114/1984). Sin embargo, su difusión pública puede vulnerar derechos de intimidad.
- **Protocolo:**
  - Se permite grabar llamadas vishing recibidas para análisis interno y como evidencia.
  - NO publicar grabaciones de audio en GitHub.
  - Se pueden publicar transcripciones anonimizadas del script de ingeniería social utilizado (sin datos identificativos de la voz).
  - Si se quiere aportar como prueba en denuncia penal, entregar directamente a FCSE.

### 5.7. Mitigación R-07: Represalias del actor

- **OPSEC del investigador:** No vincular la identidad real del investigador con el repositorio GitHub si existe riesgo de represalia.
- **Publicación bajo alias o marca del MSSP:** Los informes se publican bajo la identidad corporativa o un handle profesional, no con datos personales del analista.
- **Seguridad del repositorio:** MFA en la cuenta GitHub, revisión de permisos, monitorización de intentos de acceso.

### 5.8. Mitigación R-08: Categorías especiales

- **La nacionalidad o el origen geográfico** estimado de un actor NO constituye dato de categoría especial por sí mismo (el art. 9 RGPD protege «origen étnico o racial», no la nacionalidad).
- **La afiliación a un grupo de cibercrimen** NO constituye «convicción» protegida.
- **Si emergen datos genuinamente sensibles** (p.ej. datos de salud del actor): no publicar, no almacenar, borrar.

### 5.9. Mitigación R-09 y R-10: Derecho al olvido y difamación

**Derecho al olvido / supresión:**
- Si un actor identificado solicita la supresión del informe, evaluar caso por caso:
  - Si la atribución es correcta y el informe sirve al interés de seguridad: **denegar**, fundamentando en art. 17.3.a) RGPD (libertad de expresión e información) y art. 17.3.d) (fines de archivo en interés público).
  - Si hay error de atribución: **corregir inmediatamente**.
  - Documentar toda solicitud y la respuesta dada.

**Difamación / injurias:**
- Los informes CTI basados en hechos verificables y publicados de buena fe con finalidad informativa están protegidos por la libertad de expresión e información.
- Para minimizar riesgo:
  - Usar lenguaje factual, no valorativo («la evidencia vincula» vs. «este criminal»).
  - Incluir nivel de confianza y metodología.
  - Documentar toda la cadena de evidencia internamente.

### 5.10. Disclaimer estándar para informes publicados

Incluir en cada informe publicado en GitHub:

```markdown
## Disclaimer

This report is published for defensive cybersecurity purposes under the
principles of responsible threat intelligence sharing. The analysis is
based exclusively on open-source intelligence (OSINT) derived from
unsolicited malicious communications received by the author and
subsequent passive investigation of publicly available data.

Attribution assessments represent the analyst's professional judgment
based on available evidence and are provided with an explicit confidence
level. They do not constitute accusations of criminal conduct, which is
a determination reserved for competent judicial authorities.

If you believe any information in this report is inaccurate, please
contact [email] for prompt review and correction.

Published under the legitimate interest basis of Article 6(1)(f) GDPR,
supported by Recital 49 (network and information security) and the
right to freedom of expression and information (Article 11 EU Charter
of Fundamental Rights).
```

---

## 6. Derechos de los Interesados

### 6.1. Aplicabilidad de derechos ARSALPO en este tratamiento

| Derecho | Aplicabilidad | Fundamentación |
|---|---|---|
| **Acceso** (art. 15) | Sí, con limitaciones | El interesado puede solicitar confirmación de si se tratan sus datos y acceso a los mismos. Se puede limitar si compromete una investigación en curso (art. 23.1.d RGPD). |
| **Rectificación** (art. 16) | Sí | Si los datos son inexactos, deben corregirse. Especialmente relevante para errores de atribución. |
| **Supresión** (art. 17) | Limitada | Se puede denegar cuando el tratamiento sea necesario para el ejercicio de la libertad de expresión e información (art. 17.3.a), o con fines de archivo en interés público (art. 17.3.d). |
| **Oposición** (art. 21) | Sí, con ponderación | El responsable debe demostrar motivos legítimos imperiosos que prevalezcan sobre los del interesado. En este caso: seguridad colectiva y libertad de información. |
| **Limitación** (art. 18) | Sí | Mientras se verifica una reclamación de inexactitud, el tratamiento se limita (se mantienen los datos pero no se publican). |
| **Portabilidad** (art. 20) | No aplicable | Solo aplica cuando la base es consentimiento o contrato. Aquí la base es interés legítimo. |

### 6.2. Canal de ejercicio de derechos

- Email: [dirección de contacto para protección de datos]
- Plazo de respuesta: 30 días naturales (art. 12.3 RGPD)
- El canal se publica en el disclaimer de cada informe

---

## 7. Análisis Específico: Legalidad de la Publicación en GitHub

### 7.1. GitHub como medio de publicación

GitHub es una plataforma pública, global y permanente. La publicación en GitHub equivale funcionalmente a una publicación en medio de comunicación abierto. Esto tiene implicaciones:

**A favor de la publicación:**
- La comunidad CTI opera mayoritariamente en GitHub para compartir IoCs, reglas de detección e informes.
- La publicación permite que otros defensores se protejan frente a la misma amenaza.
- Organismos oficiales (CISA, CCN-CERT, NCSC) publican regularmente IoCs y análisis en plataformas abiertas.
- La libertad de expresión e información (art. 20 CE, art. 11 CDFUE) ampara la publicación de investigaciones sobre actividades delictivas.

**Riesgos de la publicación:**
- Permanencia: una vez publicado, el control sobre la difusión es limitado (forks, caches, indexación).
- Jurisdicción global: el contenido es accesible desde cualquier país, con marcos legales potencialmente distintos.
- Responsabilidad: el autor del informe es responsable de la exactitud del contenido publicado.

### 7.2. Licencia recomendada

Publicar los informes bajo una licencia que proteja al autor y facilite el uso defensivo:

```
Recomendación: Creative Commons Attribution 4.0 (CC BY 4.0)
- Permite compartir y adaptar el contenido
- Requiere atribución al autor
- No restringe uso comercial (permite que vendors integren los IoCs)
```

### 7.3. Estructura recomendada del repositorio

```
corvusdossiers/
├── README.md              → Disclaimer general + metodología + contacto RGPD
├── LICENSE                → CC BY 4.0
├── AVISO_LEGAL.md         → Aviso legal condensado
├── EIPD.md                → Este documento
├── reports/
│   ├── 2026-03-XX-campaign-name/
│   │   ├── report.html    → Informe narrativo (sanitizado)
│   │   ├── iocs.json      → IoCs en formato STIX 2.1
│   │   ├── iocs.csv       → IoCs en CSV para consumo rápido
│   │   └── yara/          → Reglas de detección
│   └── ...
```

---

## 8. Conclusión de la EIPD

### 8.1. Resultado global

| Aspecto | Valoración |
|---|---|
| ¿El tratamiento es necesario y proporcionado? | **Sí**, para todas las finalidades declaradas |
| ¿Los riesgos son aceptables con las medidas propuestas? | **Sí**, condicionado a la implementación del modelo de publicación graduada |
| ¿Es necesaria consulta previa a la AEPD? | **No**, siempre que se implementen todas las medidas |
| ¿Riesgo residual principal? | **Atribución errónea** — mitigado por regla de tres fuentes, peer review y protocolo de corrección |

### 8.2. Condiciones de validez

Esta EIPD es válida mientras:

1. El tratamiento se limite a comunicaciones maliciosas recibidas y la investigación OSINT pasiva derivada.
2. Se aplique el modelo de publicación graduada (§5.1) para todos los informes.
3. No se realice atribución personal (Nivel 3) sin cumplir todos los requisitos establecidos.
4. Se mantenga el disclaimer y el canal de derechos en todas las publicaciones.
5. Se revise semestralmente o ante cambios significativos.

### 8.3. Riesgos residuales aceptados

- Imposibilidad de eliminar completamente contenido publicado en GitHub una vez forkeado/cacheado (mitigado por protocolo de corrección y disclaimer).
- Posibilidad de que un actor identifique al investigador pese a las medidas OPSEC (mitigado por publicación bajo marca/alias).
- Jurisdicción global de GitHub puede exponer al responsable a marcos legales no evaluados en esta EIPD (mitigado por disclaimer y fundamentación en RGPD/CDFUE).

---

## Anexo A: Checklist Pre-Publicación

### Para cada informe, verificar antes de hacer push:

**Nivel 1 — IoCs técnicos:**
- [ ] Hashes verificados contra múltiples fuentes
- [ ] Dominios/IPs confirmados como maliciosos (no infraestructura compartida legítima)
- [ ] URLs activas probadas en sandbox
- [ ] Ningún dato personal en la capa de IoCs
- [ ] Screenshots sanitizados (datos de víctimas redactados)

**Nivel 2 — Infraestructura + registrante:**
- [ ] Datos WHOIS verificados como del actor (no de víctima/titular comprometido)
- [ ] Descartada suplantación de números/emails
- [ ] Al menos 2 fuentes independientes corroboran la vinculación
- [ ] Cadena de evidencia documentada internamente

**Nivel 3 — Atribución personal:**
- [ ] Al menos 3 fuentes independientes
- [ ] Peer review completado por segundo analista
- [ ] Nivel de confianza explícito en el informe
- [ ] Lenguaje factual, no acusatorio
- [ ] Sin datos de domicilio, familia, salud u otros no relevantes
- [ ] Disclaimer incluido
- [ ] Canal de contacto RGPD incluido
- [ ] Valoración de impacto vs. beneficio documentada

**General (todos los niveles):**
- [ ] Ningún dato de víctimas en ninguna parte del informe
- [ ] Ninguna credencial, dato bancario o información exfiltrada
- [ ] Ninguna grabación de audio publicada
- [ ] Disclaimer estándar incluido
- [ ] Licencia CC BY 4.0 referenciada

---

## Anexo B: Plantilla de Documentación Interna de Atribución

Para cada atribución de Nivel 3, documentar internamente (NO publicar):

```markdown
# Ficha de Atribución — [ID del caso]

**Fecha:** YYYY-MM-DD
**Analista principal:** [handle/nombre]
**Analista revisor:** [handle/nombre]

## Identidad atribuida
- Alias/handles:
- Nombre real (si conocido):
- Nacionalidad estimada:
- Nivel de confianza: [Alto / Medio / Bajo]

## Cadena de evidencia
1. [Fuente 1]: [descripción de la evidencia]
2. [Fuente 2]: [descripción de la evidencia]
3. [Fuente 3]: [descripción de la evidencia]

## Hipótesis alternativas descartadas
- [Hipótesis 1]: [motivo de descarte]
- [Hipótesis 2]: [motivo de descarte]

## Valoración de impacto
- Beneficio de publicar atribución:
- Riesgo de publicar atribución:
- Decisión: [Publicar / No publicar / Publicar solo alias]

## Aprobación
- [ ] Analista principal: [firma/fecha]
- [ ] Analista revisor: [firma/fecha]
```

---

## Anexo C: Registro de Actividades de Tratamiento (art. 30 RGPD)

| Campo | Contenido |
|---|---|
| **Nombre del tratamiento** | Análisis y publicación de informes CTI sobre threat actors a partir de comunicaciones maliciosas recibidas |
| **Responsable** | [Razón social], NIF [X], domicilio [X] |
| **Finalidades** | Autodefensa, generación de inteligencia de amenazas, publicación CTI, atribución de actores |
| **Base jurídica** | Art. 6.1.f) RGPD (interés legítimo — Considerando 49) |
| **Categorías de interesados** | Autores de comunicaciones maliciosas, operadores de infraestructura de ataque, terceros incidentales (víctimas, titulares suplantados) |
| **Categorías de datos** | Datos técnicos de infraestructura, datos identificativos profesionales del actor (alias, email, teléfono), datos WHOIS, datos de atribución |
| **Destinatarios** | Publicación abierta en GitHub (comunidad CTI global). FCSE si se interpone denuncia. |
| **Transferencias internacionales** | GitHub (Microsoft) — servidores en EE.UU. — cubierto por EU-US Data Privacy Framework |
| **Plazos de supresión** | Datos internos: 180 días. Informes publicados: permanentes (justificado por interés archivístico CTI) |
| **Medidas de seguridad** | Modelo de publicación graduada, sanitización, peer review, cifrado interno, RBAC, logging, MFA |

---

> **NOTA:** Este documento debe completarse con los datos específicos del responsable (campos entre [corchetes]) y validarse con asesoramiento jurídico antes de su uso como documento de cumplimiento formal. La EIPD debe revisarse semestralmente o ante cambios significativos en el tratamiento.
