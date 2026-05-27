# Avance 1 — Dashboard de Adopcion de IA (Stack Overflow Survey 2025)

**Fecha:** 27 de mayo de 2026  
**Materia:** Herramientas de Visualizacion para la Inteligencia de Negocios  
**Profesor:** Luis Alberto Jaimes C.  
**Audiencia objetivo:** VP de Ingenieria  

---

## 1. Resumen del Estado Actual

El dashboard esta construido en formato PBIP (Power BI Project) con 6 paginas (5 principales + 1 tooltip), 30 medidas DAX, 7 tablas de dimension, y un modelo semantico estrella funcional. Incluye interactividad completa: tooltips personalizados, drill-through, bookmarks y navegacion entre paginas.

**Estado general: ~85% completado**

---

## 2. Avances Completados

### 2.1 Modelo Semantico (100%)

#### Tabla de hechos: `survey_2025_results`
- **Fuente:** CSV del Stack Overflow Developer Survey 2025 (~37,467 profesionales)
- **Filtro:** Solo profesionales (`MainBranch = "I am a developer by profession"`)
- **Limpieza:** NA reemplazado por null en todas las columnas
- **49 columnas** seleccionadas, 4 tipadas como Int64
- **2 columnas calculadas:** `ExperienceRange` (6 rangos) y `Region` (8 regiones)

#### 30 Medidas DAX
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
| **Trust Gap** | [% AI Adoption] - [% Trust AI] | 0.0% |
| **Adoption to Trust Ratio** | DIVIDE([% AI Adoption], [% Trust AI], 0) | 0.00 |
| **Tooltip Adoption Summary** | VAR/RETURN concatenacion de KPIs | texto |
| + 9 medidas adicionales de soporte | (conteos, porcentajes auxiliares) | varios |

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

#### 7 Relaciones Bidireccionales
Todas conectan `dim_*.ResponseId -> survey_2025_results.ResponseId` con `crossFilteringBehavior: bothDirections`.

### 2.2 Paginas del Reporte (6 paginas)

#### Pagina 1: `page_ai_adoption` — Panorama de Adopcion de IA
- KPIs: Total Respondents, % AI Adoption, % Daily AI, % Trust AI
- Charts: AI models bar, sentiment bar, AI frequency donut
- Slicers: AISelect, Country, Experience, Region
- Nav buttons: navegador de paginas

#### Pagina 2: `page_paradox` — La Paradoja de la Confianza
- KPIs: Adoption (80.8%), Trust (32.4%), Distrust (45.9%), Favorable (61.2%)
- Charts: trust distribution, trust by region, sentiment by experience
- Slicers: Experience (multi-select habilitado), Region
- Tooltip vinculado: Tooltip_Paradox (Trust Gap + Ratio)
- Nav buttons: navegador de paginas

#### Pagina 3: `page_ecosystem` — Ecosistema Tecnologico
- Charts usan tablas de dimension: Languages treemap, IDEs bar, Databases bar, Platforms bar
- Nav buttons: navegador de paginas

#### Pagina 4: `page_llm_profiles` — Perfiles de Usuarios de LLMs
- Charts usan tablas de dimension: models by experience, devtype bar, OS donut
- **Drill-through configurado:** recibe desde P2 filtrado por AIAcc y Region
- Nav buttons: navegador de paginas

#### Pagina 5: `page_findings` — Hallazgos y Recomendaciones
- Texto narrativo con 3 hallazgos verificados (QUE/IMPLICACION/ACCION):
  - Hallazgo 1: Paradoja 80.8% adopcion vs 32.4% confianza (score 2.70/5.0)
  - Hallazgo 2: OpenAI GPT 10,882 > Claude 5,970 > Gemini Flash 4,568
  - Hallazgo 3: 50.6% uso diario, VS Code 15,989 usuarios, JavaScript 17,046
- Nav buttons: navegador de paginas

#### Pagina 6: `Tooltip_Paradox` (hidden, tipo Tooltip)
- 2 KPI cards: Trust Gap (48.4%) y Adoption to Trust Ratio (2.49x)
- Se muestra al hacer hover sobre charts de P2

### 2.3 Interactividad (100% configurada)

| Feature | Estado | Detalle |
|---------|--------|---------|
| Tooltip personalizado | DONE | Tooltip_Paradox con Trust Gap + Ratio, vinculado a charts P2 |
| Drill-through | DONE | P2 -> P4 por AIAcc y Region |
| Bookmarks | DONE | 3 vistas: "Senior Engineers View", "Latin America View", "Reset - Vista Global" |
| Nav buttons | DONE | Navegador de paginas en todas las paginas (excepto tooltip) |
| Cross-filtering | DONE | drillFilterOtherVisuals: true en todos los charts |
| Slicers multi-select | DONE | Experiencia permite seleccion multiple |

### 2.4 Filtros de Blanks (100%)
Aplicados via PBI Desktop UI en todos los charts que usaban columnas single-value (AISelect, AISent, AIAcc). Formato correcto: `filterConfig` a nivel root del visual.json.

### 2.5 Headers (100%)
Todos los headers de 5 paginas usan Deep Slate (#1E293B) con formato correcto PBIR (fill array con transparency + fillColor con selector default).

### 2.6 Narrativa de Datos (100%)
Pagina Findings completada con datos verificados via DAX queries. Estructura QUE/IMPLICACION/ACCION para VP de Ingenieria.

### 2.7 Tema y Paleta de Colores

| Color | Hex | Uso |
|-------|-----|-----|
| Deep Slate | #1E293B | Headers, fondo principal |
| Steel Blue | #3B82F6 | Acento primario, KPIs positivos |
| Amber | #F59E0B | Alertas, KPI Trust (paradoja) |
| Emerald | #10B981 | Positivo, adopcion alta |
| Coral Red | #EF4444 | Negativo, distrust |
| Slate Gray | #94A3B8 | Texto secundario, bordes |

### 2.8 Infraestructura Git
- Repositorio en GitHub: `Proyecto-Visualizacion`
- Branch: `main`
- Formato PBIP permite diff de JSON y TMDL

---

## 3. Errores Encontrados y Soluciones

### 3.1 PBI Desktop sobreescribe archivos TMDL (CRITICO)
**Problema:** Tablas de dimension creadas editando file system fueron eliminadas al abrir PBI Desktop.  
**Solucion:** Crear tablas via MCP Server (powerbi-modeling-mcp) mientras PBI Desktop esta abierto.  
**Leccion:** NUNCA editar archivos TMDL manualmente. Usar siempre MCP Server.

### 3.2 Error de formato MCP: "One of DaxExpression, MExpression..."
**Solucion:** Restructurar JSON con `MExpression` al nivel raiz de la definicion de tabla.

### 3.3 Schema incorrecto en visuales de texto
**Solucion:** Cambiar `$schema` a `visualContainer/2.9.0` (NO `visual/2.5.0`).

### 3.4 Visual.json con propiedades invalidas causan "No se pudo cargar el informe"
**Problema:** Textbox con formato `paragraphs` invalido y background color en page.json con formato incorrecto causaron fallo completo de carga del reporte.  
**Solucion:** Usar solo formatos probados. Para shapes: fill array con transparency + fillColor con selector. Para cards: copiar exactamente el formato de cards existentes sin propiedades extras de color.  
**Leccion:** Cualquier propiedad JSON desconocida en PBIR puede causar fallo TOTAL de carga, no solo un warning.

### 3.5 Filtros en visual.json: formato correcto
**Problema:** `filters` y `vcFilters` dentro de visual.json fueron ignorados por PBI Desktop.  
**Solucion:** Formato correcto es `filterConfig` a nivel ROOT del visual.json (fuera de `visual`), con estructura `Version: 2`, `From/Where`.  
**Recomendacion:** Aplicar filtros desde PBI Desktop UI y copiar el formato generado.

### 3.6 ActionButton navs renderizaban como cajas blancas
**Problema:** Botones de navegacion creados via JSON aparecian como rectangulos blancos sin funcionalidad.  
**Solucion:** Usar navegador de paginas nativo de PBI Desktop (Insertar -> Botones -> Navegacion).

---

## 4. Lo Que Falta

### 4.1 Estilo Visual (Prioridad 1 — ~1 hora)
- [ ] Aplicar colores a KPIs de P2: Trust=#F59E0B, Distrust=#EF4444, Adoption=#3B82F6, Favorable=#10B981
- [ ] Aplicar colores similares a KPIs de P1
- [ ] Colores en Tooltip cards: Trust Gap=#F59E0B, Ratio=#EF4444
- [ ] Verificar alineacion y espaciado consistente
- [ ] Verificar formato de numeros (comas, decimales)

### 4.2 Testing Final (Prioridad 2 — ~1 hora)
- [ ] Probar cada slicer en cada pagina
- [ ] Verificar cross-filter entre charts
- [ ] Probar drill-through P2->P4
- [ ] Probar tooltip hover en P2
- [ ] Probar bookmarks (Senior Engineers, LatAm, Reset)
- [ ] Verificar nav buttons funcionan en todas las paginas
- [ ] Exportar a PDF y verificar legibilidad
- [ ] Test de accesibilidad (contraste de colores, tamano de texto)

---

## 5. Estructura de Archivos del Proyecto

```
Proyecto-Visualizacion/
├── avances y planes/
│   └── Avance_1.md                              <- Este documento
├── Proyecto V.Report/
│   └── definition/
│       ├── bookmarks/                            (3 bookmarks)
│       ├── pages/
│       │   ├── pages.json                        (6 paginas)
│       │   ├── page_ai_adoption/                 (P1: Panorama)
│       │   ├── page_paradox/                     (P2: Paradoja)
│       │   ├── page_ecosystem/                   (P3: Ecosistema)
│       │   ├── page_llm_profiles/                (P4: Perfiles LLM + drill-through)
│       │   ├── page_findings/                    (P5: Hallazgos)
│       │   └── 76d9c188688856e3d3e9/             (Tooltip_Paradox, hidden)
│       └── report.json
├── Proyecto V.SemanticModel/
│   └── definition/
│       ├── model.tmdl                            (8 refs de tablas)
│       ├── relationships.tmdl                    (7 relaciones)
│       └── tables/
│           ├── survey_2025_results.tmdl          (49 cols, 30 measures)
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

## 6. Notas Tecnicas

### Como abrir el proyecto
1. Abrir `Proyecto V.pbip` con Power BI Desktop (version con soporte PBIP/DevMode)
2. El CSV fuente debe estar en `C:\Users\danie\Downloads\survey_2025_results.csv`

### Workflow para editar visuals programaticamente
1. Cerrar PBI Desktop
2. Editar archivos JSON de visual (usar solo formatos probados)
3. Reabrir PBI Desktop — verificar cambios

### MCP Server para modelo semantico
- Usar `powerbi-modeling-mcp` para operaciones en el modelo mientras PBI Desktop esta abierto
- NUNCA editar TMDL manualmente

### Formato PBIR clave
- Schema: `visualContainer/2.9.0`
- Filtros: `filterConfig` a nivel root (NO `filters` ni `vcFilters` dentro de `visual`)
- Shape fill: array con 2 entries (transparency + fillColor con selector default)
- Cards: copiar formato exacto de cards existentes, no agregar propiedades extras

---

## 7. Cronograma Restante

| Tarea | Estimacion |
|-------|-----------|
| Estilo visual (colores KPI, alineacion) | 1 hora |
| Testing final + export PDF | 1 hora |
| **Total estimado** | **~2 horas** |

---

*Generado con asistencia de Claude Code (Claude Opus 4.6)*
