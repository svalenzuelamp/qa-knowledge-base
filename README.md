# 🧠 QA Knowledge Base - Fintech Apps & APIs 🇨🇱

> Archivo base para alimentar a agentes de automatización con buenas prácticas, ejemplos técnicos y escenarios relevantes para apps Fintech (Chile).

---

## 🔐 1. Buenas prácticas para pruebas de APIs REST

- Usar códigos HTTP estándar:
  - 200 OK → éxito
  - 400 Bad Request → datos inválidos
  - 401 Unauthorized → token JWT inválido/ausente
  - 403 Forbidden → sin permisos
  - 500 Internal Server Error → falla general del backend

- Validar:
  - Estructura del JSON
  - Campos obligatorios
  - Tipos de datos esperados
  - Lógica de negocio (límites, condiciones)

### ✅ Ejemplo Gherkin - Token válido:
```gherkin
Scenario: Consulta de saldo con token JWT válido
  Given que el usuario tiene un token válido
  When realiza una petición GET a "/api/v1/saldo"
  Then debe recibir un código 200
  And el campo "saldoDisponible" debe ser mayor a 0
```

### ❌ Ejemplo Gherkin - Token expirado:
```gherkin
Scenario: Token expirado
  Given que el usuario tiene un token expirado
  When accede a "/api/v1/movimientos"
  Then debe recibir un código HTTP 401
  And el mensaje debe contener "Token expirado"
```

---

## 📲 2. Buenas prácticas en testing mobile (Android/iOS)

- Usar `Appium` para interacción cross-platform
- Verificar:
  - Navegación entre pantallas
  - Validaciones visuales (error toast, loading, etc.)
  - Comportamiento offline
  - Interacciones: swipe, tap, long press, scroll
  - Performance en dispositivos de gama baja

### 📱 Ejemplo Gherkin - Validación de RUT:
```gherkin
Scenario: Usuario ingresa un RUT inválido
  Given que el usuario está en la pantalla de registro
  When ingresa el RUT "12345678-9"
  Then debería ver un mensaje de error que dice "RUT inválido"
```

---

## 💼 3. Casos comunes en apps Fintech chilenas

- Validación de RUT (usar librerías como `validar-rut`, `rut.js`)
- Montos en pesos CLP, usar `Intl.NumberFormat` o librerías contables
- Autenticación 2FA (código por SMS o correo)
- Límites de transferencia:
  - Mínimo diario: $1.000 CLP
  - Máximo diario: $500.000 CLP
- Códigos de operación únicos (para soporte o seguimiento)

---

## 🚨 4. Errores comunes a testear

| Error                        | Cómo detectarlo                          |
|-----------------------------|------------------------------------------|
| Token JWT mal firmado       | API devuelve 403                         |
| RUT en formato incorrecto   | Falla front y no llega al backend        |
| Scroll infinito no carga más | Usar espera por visibilidad del loader   |
| Timeout en servicios lentos | Forzar test con `sleep` + retry          |
| Campo monto como string     | Validar tipo numérico con decimales      |

---

## 📦 5. Recomendaciones de estructura en `.feature`

- Usa `Feature: [nombre funcionalidad]`
- Incluye al menos:
  - 1 escenario principal (flujo feliz)
  - 2-3 escenarios negativos o edge cases
  - Comentarios en pasos complejos (`# este paso verifica...`)
- Usa tags para clasificar:
  - `@api`, `@mobile`, `@critico`, `@seguridad`

---

💬 Si necesitas más fuentes de conocimiento:
- https://automationpanda.com
- https://ministryoftesting.com
- https://api.stackexchange.com
- https://cucumber.io/docs/guides/10-minute-tutorial/

---

**Última actualización**: abril 2025 - por tu agente Copilot + Brother QA Mentor 😎
