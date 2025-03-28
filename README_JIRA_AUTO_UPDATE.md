
# 🚀 Jira Auto Integration Workflow

Este repositorio contiene un **GitHub Actions Workflow** que automatiza la **actualización de campos personalizados en Jira**, así como la **inserción de comentarios con resumen en español e inglés**, generados automáticamente con ayuda de **Gemini (Google AI)**.

Este flujo está diseñado para ser reutilizado en diferentes repositorios y equipos de desarrollo. Solo necesitas ajustar la rama en la que trabajas y seguir el formato correcto en tus Pull Requests.

---

## ⚙️ ¿Qué hace este workflow?

Cuando se hace **merge de un Pull Request**, el flujo:

1. Extrae la información del cuerpo del PR (formato estructurado).
2. Detecta el ticket de Jira referenciado (por URL).
3. Verifica si los campos clave ya fueron actualizados:
   - **Qué se hizo** (`customfield_10211`)
   - **Bitácora de piezas** (`customfield_10212`)
4. Si están vacíos, los actualiza automáticamente.
5. Siempre agrega 2 comentarios al ticket de Jira:
   - Un resumen **en español**
   - Un resumen **en inglés**
6. Todo el resumen se genera automáticamente usando **Gemini AI**.

---

## ✅ Requisitos

- Tener configurados los siguientes **Secrets globales o locales** en GitHub:
  - `GENINI_IT_DEV_AGENT`: API Key de Gemini (Google AI)
  - `JIRA_IT_DEV_AGENT`: API Key de Jira (`it.dev.agent@maxillc.com`)

---

## 🛠 Configuración rápida

### 1. 📁 Archivo de workflow

Agrega el archivo en la siguiente ruta dentro del repositorio:

```
.github/workflows/jira-auto-update.yml
```

Puedes renombrarlo si lo deseas, pero recomendamos algo descriptivo como:

```
jira-auto-update.yml
```

---

### 2. 🌱 Configuración de ramas

Por defecto, el flujo está configurado para ejecutarse solo cuando se hace **merge a `release/2025-3`**.

```yaml
on:
  pull_request:
    branches:
      - release/2025-3
    types:
      - closed
```

#### Puedes personalizar las ramas así:

```yaml
branches:
  - release/*
  - develop
  - main
  - staging
```

> Esto permite que diferentes áreas puedan aplicarlo en sus entornos.

---

## 📝 Formato requerido del Pull Request

El cuerpo del PR **debe seguir este formato estructurado** para que el flujo funcione correctamente:

```text
Qué se hizo:
[Descripción breve y clara del cambio realizado]

Objetos Modificados:
[Listado de proyectos, capas o archivos principales impactados]

Etiqueta relacionada:
https://maxims.atlassian.net/browse/HRMS-XXXX

PRs Relacionados:
[Opcional - puedes poner "NA" si no aplica]
```

> ⚠️ Si cualquiera de las tres secciones obligatorias está vacía o tiene solo “N/A”, los campos en Jira **no se actualizarán**, pero **sí se insertarán los comentarios con resumen**.

---

### ✅ Ejemplo de uso

```text
Qué se hizo:
Se ajustó el servicio de validateTransfer para volver a habilitar el log ya que se resolvió por BD.

Objetos Modificados:
AgentServices.MoneyTransfer.API

Etiqueta relacionada:
https://maxims.atlassian.net/browse/HRMS-3439

PRs Relacionados:
NA
```

---

## 📌 Consideraciones y notas adicionales

- El resumen generado por Gemini se limita a los primeros **3000 caracteres** del `diff` de código.
- El resumen se enfoca en el **impacto funcional y valor de negocio**, no en detalles técnicos.
- El flujo **nunca sobreescribe campos de Jira si ya tienen contenido.**
- Los comentarios en Jira se generan **siempre**, incluso si los campos no se actualizan.
- El autor que hace el merge aparece citado automáticamente en los comentarios.
- Si el PR no contiene una URL de Jira, el flujo se salta completamente.
- El cuerpo del PR puede tener tildes, mayúsculas o redacción variable. El flujo normaliza y detecta los encabezados de forma flexible (`Qué se hizo`, `Que se hizo`, `Objetos DB`, `Objetos Modificados`, etc.)

---

> Automatización que conecta código, contexto y trazabilidad.  
> — **GitHub + Jira + Gemini** 💡
