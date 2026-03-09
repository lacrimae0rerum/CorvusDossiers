# AVISO LEGAL — CorvusDossiers

**Repositorio de Informes de Inteligencia**

Marzo 2026 | Versión 1.0

---

## 1. Naturaleza del Repositorio

CorvusDossiers es un repositorio de informes de inteligencia en formato HTML generados mediante análisis OSINT (Open Source Intelligence) con CorvusEngine. Contiene exclusivamente productos analíticos finalizados — no almacena datos brutos, bases de datos intermedias ni resultados de correlación sin procesar.

---

## 2. Marco Legal Aplicable

Los informes contenidos en este repositorio están sujetos a:

| Normativa | Aplicación |
|---|---|
| **RGPD** (Reglamento (UE) 2016/679) | Los informes pueden contener datos personales obtenidos de fuentes abiertas |
| **LOPDGDD** (LO 3/2018) | Adaptación española del RGPD |
| **Código Penal** (LO 10/1995, arts. 197 y ss.) | Delitos contra la intimidad — no aplicable al uso estándar (correlación pasiva de fuentes públicas) |
| **EU AI Act** (Reglamento (UE) 2024/1689) | Si los informes incorporan análisis generados por IA |

---

## 3. Base Jurídica del Tratamiento

La generación y almacenamiento de estos informes se ampara en:

- **Art. 6.1.f) RGPD — Interés legítimo** para CTI defensiva (Considerando 49 RGPD: seguridad de la red y de la información).
- **Art. 6.1.b) RGPD — Ejecución contractual** cuando los informes se producen bajo contrato con un cliente.

La ponderación completa del interés legítimo se documenta en la EIPD del proyecto CorvusEngine.

---

## 4. Protección de Datos en los Informes

### 4.1. Contenido de los informes

Los informes pueden contener:

- Datos técnicos de infraestructura (IPs, dominios, hashes, certificados).
- Datos identificativos profesionales (nombre, cargo, email corporativo) obtenidos de fuentes públicas.
- Datos mercantiles de registros oficiales (BORME, BOE, Registro Mercantil).
- Indicadores de compromiso (IoCs) y análisis de TTPs.

### 4.2. Datos que NO deben aparecer en los informes

- Datos de categorías especiales (art. 9 RGPD): origen étnico, opiniones políticas, salud, orientación sexual.
- Datos obtenidos mediante acceso no autorizado a sistemas protegidos.
- Perfiles comprensivos de personas físicas sin vinculación directa a una amenaza de seguridad.

### 4.3. Seudonimización

Los datos personales incidentales en informes compartidos deben seudonimizarse salvo que su inclusión sea estrictamente necesaria para la finalidad del informe.

---

## 5. Distribución y Acceso

| Regla | Detalle |
|---|---|
| **Destinatarios autorizados** | Cliente contratante, equipo CTI interno autorizado |
| **Prohibición de redistribución** | Los informes no pueden compartirse con terceros sin autorización expresa del responsable |
| **Anonimización para comunidades CTI** | Si se comparten IoCs en MISP/ISACs, deben anonimizarse los datos personales |
| **Clasificación** | Cada informe debe llevar su marcado de clasificación correspondiente |

---

## 6. Política de Retención

| Tipo de informe | Retención máxima | Acción al expirar |
|---|---|---|
| Informes de cliente | Según contrato (máx. 2 años) | Borrado o devolución |
| Informes internos de amenazas | 180 días desde cierre de investigación | Borrado |
| Informes publicados (sin datos personales) | Sin límite | — |

---

## 7. Derechos de los Interesados

Cualquier persona cuyos datos aparezcan en un informe puede ejercer sus derechos de acceso, rectificación, supresión, oposición, limitación y portabilidad (ARSALPO) contactando a: [email de protección de datos].

Plazo de respuesta: 30 días naturales (art. 12.3 RGPD).

---

## 8. Documentación Completa

Este repositorio se rige por la documentación legal que se detalla a continuación. Toda publicación de informes está sujeta a las bases legales y procedimientos aquí establecidos:

**En este repositorio:**
- [`EIPD.md`](EIPD.md) — Evaluación de Impacto específica para el análisis y publicación de informes sobre threat actors (modelo de publicación graduada, checklist pre-publicación, disclaimer estándar)

**En el repositorio CorvusEngine:**
- [`docs/legal/EIPD.md`](https://github.com/lacrimae0rerum/CorvusEngine/blob/main/docs/legal/EIPD.md) — Evaluación de Impacto general del framework
- [`docs/legal/ANALISIS_JURIDICO.md`](https://github.com/lacrimae0rerum/CorvusEngine/blob/main/docs/legal/ANALISIS_JURIDICO.md) — Marco legal completo (RGPD, LOPDGDD, CP, AI Act)

---

> **NOTA:** Este documento constituye un aviso legal informativo. Debe adaptarse a las circunstancias concretas de cada organización y validarse con asesoramiento jurídico profesional. Las secciones marcadas con [corchetes] deben completarse con la información específica del responsable.
