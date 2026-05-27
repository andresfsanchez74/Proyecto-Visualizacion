# Avance 1 — Dashboard de Adopcion de IA (Stack Overflow Survey 2025)

**Fecha:** 26 de mayo de 2026  
**Materia:** Herramientas de Visualizacion para la Inteligencia de Negocios  
**Profesor:** Luis Alberto Jaimes C.  
**Audiencia objetivo:** VP de Ingenieria  

---

## 1. Resumen del Estado Actual

El dashboard esta construido en formato PBIP (Power BI Project) con 5 paginas, 18 medidas DAX, 7 tablas de dimension, 54 visuales totales, y un modelo semantico estrella funcional. La estructura programatica permite control de versiones con Git.

**Estado general: ~65% completado**

---

## 2. Avances Completados

### 2.1 Modelo Semantico (100% estructura, pendiente validacion visual)

#### Tabla de hechos: `survey_2025_results`
- **Fuente:** CSV del Stack Overflow Developer Survey 2025
- **Filtro:** Solo profesionales (`MainBranch = "I am a developer by profession"`)
- **Limpieza:** NA reemplazado por null en todas las columnas
- **47 columnas** seleccionadas, 4 tipadas como Int64 (ResponseId, ConvertedCompYearly, YearsCode, WorkExp)
- **2 columnas calculadas:**
  - `ExperienceRange`: Clasifica YearsCode en 6 rangos (0-2 Junior, 3-5 Mid, 6-10 Senior, 11-20 Staff, 20+ Principal, Unknown)
  - `Region`: Clasifica Country en 8 regiones geograficas (North America, Latin America, Europe, Asia, Oceania, Africa, Middle East, Other)

#### 18 Medidas DAX
| Medida | Formula | Formato |
|--------|---------|---------|
| Total Respondents | COUNTROWS | #,##0 |
| % AI Adoption | Usuarios AI / No blank AISelect | 0.0% |
| % Daily AI Use | Daily users / No blank AISelect | 0.0% |
| % Favorable Sentiment | Favorable+Very favorable / No blank AISent | 0.0% |
| % Trust AI | Somewhat+Highly trust / No blank AIAcc | 0.0% |
| % Distrust AI | Somewhat+Highly distrust / No blank AIAcc | 0.0% |
| % Unfavorable Sentiment | Unfavorable+Very unfavorable / No blank AISent | 0.0% |
| Median Salary USD | MEDIAN(ConvertedCompYearly) | $#,##0 |
| AI Users Count | CALCULATE con filtro AISelect IN usos | #,##0 |
| Non-AI Users Count | CALCULATE con filtro AISelect IN no uso | #,##0 |
| Users ChatGPT | CONTAINSSTRING "openAI GPT" | #,##0 |
| Users Claude | CONTAINSSTRING "Anthropic: Claude" | #,##0 |
| Users Gemini | CONTAINSSTRING "Gemini" | #,##0 |
| Users Llama | CONTAINSSTRING "Llama" | #,##0 |
| Users DeepSeek | CONTAINSSTRING "DeepSeek" | #,##0 |
| Users Mistral | CONTAINSSTRING "Mistral" | #,##0 |
| Users Phi4 | CONTAINSSTRING "Microsoft Phi" | #,##0 |
| Users Grok | CONTAINSSTRING "Grok" | #,##0 |

#### 7 Tablas de Dimension (creadas via MCP Server)
Cada una resuelve el problema de columnas multi-seleccion (valores separados por `;`):

| Tabla | Columna principal | Columna origen | Filas aprox |
|-------|------------------|----------------|-------------|
| dim_AIModels | AIModel | AIModelsHaveWorkedWith | 45,022 |
| dim_Languages | Language | LanguageHaveWorkedWith | 151,550 |
| dim_Databases | Database | DatabaseHaveWorkedWith | 73,788 |
| dim_Platforms | Platform | PlatformHaveWorkedWith | 145,504 |
| dim_IDEs | IDE | DevEnvsHaveWorkedWith | 75,866 |
| dim_DevType | DevTypeValue | DevType | 33,686 |
| dim_OS | OS | OpSysProfessional use | 49,272 |

**Logica M de cada tabla:** Split por ";", expandir lista, trim, renombrar columna, filtrar nulls/vacios.

#### 7 Relaciones Bidireccionales
Todas conectan `dim_*.ResponseId → survey_2025_results.ResponseId` con `crossFilteringBehavior: bothDirections`.

### 2.2 Paginas del Reporte (5 paginas creadas)

#### Pagina 1: `page_ai_adoption` — Panorama de Adopcion de IA
- 12 visuales: header, titulo, subtitulo, 4 KPIs (Total Respondents, % AI Adoption, % Daily AI, % Trust AI), 4 slicers (AISelect, Country, Experience, Region), chart_ai_models_bar, chart_sentiment_bar, chart_ai_frequency_donut
- chart_ai_models_bar **actualizado** para usar `dim_AIModels.AIModel`

#### Pagina 2: `page_paradox` — La Paradoja de la Confianza
- 10 visuales: header, titulo, 4 KPIs (Adoption, Trust, Distrust, Favorable), 2 slicers (Experience, Region), chart_trust_by_region (clustered bar por AISent y Region), chart_sentiment_by_experience, chart_trust_distribution

#### Pagina 3: `page_ecosystem` — Ecosistema Tecnologico
- 9 visuales: header, titulo, 2 KPIs (Total, Salary), 2 slicers (Experience, Region), chart_languages_treemap, chart_ides_bar, chart_databases_bar, chart_platforms_bar
- 4 charts **actualizados** para usar tablas de dimension:
  - chart_languages_treemap → `dim_Languages.Language`
  - chart_ides_bar → `dim_IDEs.IDE`
  - chart_databases_bar → `dim_Databases.Database`
  - chart_platforms_bar → `dim_Platforms.Platform`

#### Pagina 4: `page_llm_profiles` — Perfiles de Usuarios de LLMs
- 8 visuales: header, titulo, 2 KPIs (AI Users, Salary), 2 slicers (Experience, Region), chart_models_by_experience (clustered bar), chart_devtype_bar, chart_os_donut
- 3 charts **actualizados** para usar tablas de dimension:
  - chart_models_by_experience → `dim_AIModels.AIModel`
  - chart_devtype_bar → `dim_DevType.DevTypeValue`
  - chart_os_donut → `dim_OS.OS`

#### Pagina 5: `page_findings` — Hallazgos y Conclusiones
- 9 visuales: header, titulo, findings_text, 5 KPIs (Adoption, Daily, Favorable, Trust, Salary), 2 slicers (Experience, Region)

### 2.3 Tema y Paleta de Colores (definido, aplicacion parcial)

| Color | Hex | Uso |
|-------|-----|-----|
| Deep Slate | #1E293B | Headers, fondo principal |
| Steel Blue | #3B82F6 | Acento primario, KPIs positivos |
| Amber | #F59E0B | Alertas, KPI Trust (paradoja) |
| Emerald | #10B981 | Positivo, adopcion alta |
| Coral Red | #EF4444 | Negativo, distrust |
| Slate Gray | #94A3B8 | Texto secundario, bordes |

### 2.4 Infraestructura Git
- Repositorio en GitHub: `Proyecto-Visualizacion`
- Branch: `main`
- 4 commits previos
- Formato PBIP permite diff de JSON y TMDL

---

## 3. Errores Encontrados y Soluciones

### 3.1 PBI Desktop sobreescribe archivos TMDL (CRITICO)
**Problema:** Se crearon 7 archivos .tmdl de tablas de dimension editando directamente el file system. Al abrir PBI Desktop, elimino TODOS los archivos nuevos y revirtio model.tmdl.  
**Causa:** PBI Desktop es la fuente de verdad del modelo; al abrir regenera el TMDL desde su estado interno.  
**Solucion:** Crear tablas via MCP Server (powerbi-modeling-mcp) mientras PBI Desktop esta abierto. El MCP habla directamente con el motor de Analysis Services.  
**Leccion:** NUNCA editar archivos TMDL manualmente para agregar tablas. Usar siempre el MCP Server o la interfaz de PBI Desktop.

### 3.2 Error de formato MCP: "One of DaxExpression, MExpression..."
**Problema:** Al crear tablas via MCP, el formato tenia `partitions[].source.type/expression` anidado.  
**Causa:** El MCP espera `MExpression` como propiedad de nivel superior, no dentro de partitions.  
**Solucion:** Restructurar el JSON enviado al MCP con `MExpression` al nivel raiz de la definicion de tabla.

### 3.3 Schema incorrecto en 7 visuales de texto
**Problema:** PBI Desktop reporto errores de schema en 7 visuales: `subtitle_text`, `title_text`, `title_p3`, `findings_text`, `title_p5`, `title_p4`, `title_p2`.  
**Error:** "La propiedad /$schema no coincide con el patron especificado"  
**Causa:** Se usaba el schema `visual/2.5.0` pero PBI Desktop espera `visualContainer/2.9.0`.  
**Solucion:** Cambiar `$schema` en los 7 archivos a `https://developer.microsoft.com/json-schemas/fabric/item/report/definition/visualContainer/2.9.0/schema.json`.

### 3.4 PBI Desktop revierte referencias de Entity en visuals
**Problema:** Al editar visual.json para cambiar Entity de `survey_2025_results` a `dim_AIModels` (por ejemplo), PBI Desktop al abrir revierta los cambios si las tablas de dimension no existen en el modelo.  
**Solucion:** Primero crear las tablas via MCP → guardar y cerrar PBI Desktop → editar los visual.json → reabrir PBI Desktop.

---

## 4. Areas de Mejora Identificadas

### 4.1 Filtros de valores en blanco (PENDIENTE - Impacto Alto)
Algunos graficos basados en columnas de valor unico (no multi-seleccion) muestran categorias "(Blank)" que distorsionan los datos:
- `slicer_aiselect` — Slicer de AISelect muestra opcion en blanco
- `chart_sentiment_bar` (P1) — AISent bar chart incluye blanks
- `chart_trust_by_region` (P2) — Clustered bar incluye blanks en AISent
- `chart_sentiment_by_experience` (P2) — Incluye blanks
- `chart_trust_distribution` (P2) — Incluye blanks en AIAcc

**Solucion requerida:** Agregar filtros a nivel de visual en los JSON (`filters` array) con condicion `Is Not Blank` para las columnas relevantes. Alternativa: agregar manualmente en PBI Desktop.

### 4.2 Header styling (PENDIENTE)
Los headers aparecen en azul brillante en lugar del Deep Slate (#1E293B) definido en la paleta. Necesitan ajuste de colores de fondo y texto.

### 4.3 KPI Trust deberia ser Amber
El KPI de % Trust AI deberia usar color amber (#F59E0B) para comunicar la paradoja (confianza baja vs adopcion alta), no el azul default.

### 4.4 Verificacion visual pendiente
Los 8 visual.json editados para apuntar a tablas de dimension NO han sido verificados abriendo PBI Desktop. Falta confirmar que PBI Desktop los acepta correctamente.

---

## 5. Lo Que Falta (Detallado)

### 5.1 Verificacion Inmediata (Prioridad 1)
- [ ] Abrir PBI Desktop y verificar que los 8 graficos con tablas de dimension funcionan
- [ ] Verificar que las 7 relaciones bidireccionales estan activas
- [ ] Confirmar que los slicers filtran correctamente a traves de las tablas de dimension
- [ ] Si hay errores, tomar nota y reportar

### 5.2 Filtros de Blanks (Prioridad 2)
- [ ] Agregar filtro "Is Not Blank" a los graficos que usan AISelect, AISent, AIAcc directamente
- [ ] Verificar que los graficos con dim tables no muestran blanks (deberian estar limpios por la logica M)

### 5.3 Interactividad (Prioridad 3 — Plan Dia 4)
- [ ] **Tooltips personalizados:** Agregar tooltips a graficos clave con informacion adicional (ej: hover sobre un modelo LLM muestra desglose por experiencia)
- [ ] **Cross-filtering:** Verificar que hacer clic en un grafico filtra los demas (drill filter ya habilitado en todos los charts con `drillFilterOtherVisuals: true`)
- [ ] **Drill-through:** Configurar al menos una pagina de drill-through (ej: desde un modelo LLM navegar a detalle de perfil)
- [ ] **Bookmarks:** Crear bookmarks para vistas predefinidas (ej: "Solo Senior Engineers", "Latin America Only")
- [ ] **Botones de navegacion:** Agregar botones para navegar entre paginas del dashboard

### 5.4 Estilo y Pulido Visual (Prioridad 4 — Plan Dia 5)
- [ ] **Aplicar paleta de colores** correctamente a todos los visuales:
  - Headers: fondo #1E293B, texto blanco
  - KPIs positivos: #3B82F6 (Steel Blue) o #10B981 (Emerald)
  - KPI Trust: #F59E0B (Amber) para senalar paradoja
  - KPIs negativos: #EF4444 (Coral Red) para distrust
- [ ] **Alineacion y espaciado:** Verificar que todos los visuales estan alineados consistentemente segun la grilla (20px margen izquierdo, 430px segunda columna)
- [ ] **Titulos de graficos:** Verificar que todos los graficos tienen titulos descriptivos en espanol
- [ ] **Formato de datos:** Verificar formato de numeros (comas para miles, % con un decimal)
- [ ] **Responsividad:** Verificar que el dashboard se ve bien en diferentes resoluciones

### 5.5 Narrativa de Datos (Prioridad 5)
- [ ] **Pagina Findings:** Completar el texto de hallazgos con la narrativa "What / So What / Now What":
  - What: 80% adopcion, 32% confianza
  - So What: Paradoja indica riesgo de shadow AI y baja calidad de outputs
  - Now What: Recomendaciones para VP (training, guidelines, governance)
- [ ] **Anotaciones:** Agregar texto explicativo en graficos clave
- [ ] **Callouts:** Destacar los insights mas importantes con formato visual diferenciado

### 5.6 Testing Final
- [ ] Verificar todas las medidas DAX con datos conocidos
- [ ] Probar cada slicer en cada pagina
- [ ] Verificar que cross-filter funciona entre paginas
- [ ] Exportar a PDF y verificar legibilidad
- [ ] Test de accesibilidad (contraste de colores, tamano de texto)

---

## 6. Estructura de Archivos del Proyecto

```
Proyecto-Visualizacion/
├── avances/
│   └── Avance_1.md          ← Este documento
├── Proyecto V.Report/
│   └── definition/
│       ├── pages/
│       │   ├── pages.json                    (5 paginas)
│       │   ├── page_ai_adoption/             (12 visuales)
│       │   ├── page_paradox/                 (10 visuales)
│       │   ├── page_ecosystem/               (9 visuales)
│       │   ├── page_llm_profiles/            (8 visuales)
│       │   └── page_findings/                (9 visuales)
│       └── report.json
├── Proyecto V.SemanticModel/
│   └── definition/
│       ├── model.tmdl                        (8 refs de tablas)
│       ├── relationships.tmdl                (7 relaciones)
│       └── tables/
│           ├── survey_2025_results.tmdl      (47 cols, 18 measures)
│           ├── dim_AIModels.tmdl
│           ├── dim_Languages.tmdl
│           ├── dim_Databases.tmdl
│           ├── dim_Platforms.tmdl
│           ├── dim_IDEs.tmdl
│           ├── dim_DevType.tmdl
│           └── dim_OS.tmdl
└── Proyecto V.pbip
```

---

## 7. Notas Tecnicas para el Equipo

### Como abrir el proyecto
1. Abrir `Proyecto V.pbip` con Power BI Desktop (version con soporte PBIP/DevMode)
2. El CSV fuente debe estar en `C:\Users\danie\Downloads\survey_2025_results.csv`
3. Si el CSV esta en otra ruta, editar la ruta en Power Query (Transform Data → Advanced Editor)

### Workflow para editar visuals programaticamente
1. Abrir PBI Desktop y hacer los cambios al modelo semantico
2. **Guardar** en PBI Desktop
3. **Cerrar** PBI Desktop
4. Editar los archivos JSON de visual
5. Reabrir PBI Desktop — verificar cambios

### Formato PBIR de visuals
- Schema: `visualContainer/2.9.0` (NO usar `visual/2.5.0` ni `visual/2.9.0`)
- Propiedad `drillFilterOtherVisuals: true` habilita cross-filter

### MCP Server para modelo semantico
- Usar `powerbi-modeling-mcp` para operaciones en el modelo mientras PBI Desktop esta abierto
- Tablas con M Expression: usar propiedad `MExpression` de nivel superior (NO dentro de partitions)

---

## 8. Cronograma Restante

| Tarea | Estimacion | Responsable |
|-------|-----------|-------------|
| Verificar dim tables en PBI Desktop | 30 min | Quien tenga el CSV |
| Filtros de blanks | 1 hora | Cualquiera |
| Interactividad (tooltips, drill-through, bookmarks) | 3-4 horas | TBD |
| Estilo visual (paleta, headers, KPIs) | 2-3 horas | TBD |
| Narrativa de datos (findings) | 1-2 horas | TBD |
| Testing final | 1-2 horas | Todo el equipo |
| **Total estimado** | **~10-12 horas** | |

---

*Generado con asistencia de Claude Code (Claude Opus 4.6)*
