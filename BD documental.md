Aquí tienes un ejemplo claro y práctico de cómo podrías diseñar una base de datos tipo **NoSQL (por ejemplo, MongoDB)** para un **reloj pastillero inteligente**. Incluye base de datos, colecciones (tablas) y documentos con sus atributos y tipos de datos.

---

# 🧠 Base de datos: `reloj_pastillero`

## 📁 Colección: `usuarios`

Guarda la información de los usuarios del dispositivo.

### 📄 Documento de ejemplo:

```json
{
  "_id": ObjectId("6612abc123"),
  "nombre": "Juan Pérez",
  "edad": 68,
  "correo": "juan.perez@email.com",
  "telefono": "6561234567",
  "fecha_registro": ISODate("2026-04-20T10:30:00Z"),
  "activo": true
}
```

### 🔎 Atributos:

* `_id`: ObjectId → identificador único
* `nombre`: String
* `edad`: Number (int)
* `correo`: String
* `telefono`: String
* `fecha_registro`: Date
* `activo`: Boolean

---

## 💊 Colección: `medicamentos`

Información de medicamentos registrados.

### 📄 Documento:

```json
{
  "_id": ObjectId("6612def456"),
  "nombre": "Paracetamol",
  "descripcion": "Analgésico",
  "dosis_mg": 500,
  "fabricante": "Farmacéutica XYZ"
}
```

### 🔎 Atributos:

* `_id`: ObjectId
* `nombre`: String
* `descripcion`: String
* `dosis_mg`: Number
* `fabricante`: String

---

## ⏰ Colección: `recordatorios`

Define cuándo debe tomar el medicamento el usuario.

### 📄 Documento:

```json
{
  "_id": ObjectId("6612ghi789"),
  "usuario_id": ObjectId("6612abc123"),
  "medicamento_id": ObjectId("6612def456"),
  "hora": "08:00",
  "frecuencia": "diaria",
  "fecha_inicio": ISODate("2026-04-25"),
  "fecha_fin": ISODate("2026-05-25"),
  "activo": true
}
```

### 🔎 Atributos:

* `_id`: ObjectId
* `usuario_id`: ObjectId (referencia a usuarios)
* `medicamento_id`: ObjectId (referencia a medicamentos)
* `hora`: String (HH:mm)
* `frecuencia`: String ("diaria", "semanal", etc.)
* `fecha_inicio`: Date
* `fecha_fin`: Date
* `activo`: Boolean

---

## 📊 Colección: `historial_tomas`

Registra si el usuario tomó o no el medicamento.

### 📄 Documento:

```json
{
  "_id": ObjectId("6612xyz987"),
  "usuario_id": ObjectId("6612abc123"),
  "medicamento_id": ObjectId("6612def456"),
  "fecha_hora_programada": ISODate("2026-04-27T08:00:00Z"),
  "fecha_hora_toma": ISODate("2026-04-27T08:05:00Z"),
  "estado": "tomado",
  "observaciones": "Tomado a tiempo"
}
```

### 🔎 Atributos:

* `_id`: ObjectId
* `usuario_id`: ObjectId
* `medicamento_id`: ObjectId
* `fecha_hora_programada`: Date
* `fecha_hora_toma`: Date
* `estado`: String ("tomado", "omitido", "tarde")
* `observaciones`: String

---

## 🔔 Colección: `notificaciones`

Controla alertas del reloj.

### 📄 Documento:

```json
{
  "_id": ObjectId("6612notif001"),
  "usuario_id": ObjectId("6612abc123"),
  "mensaje": "Es hora de tomar tu medicamento",
  "fecha_envio": ISODate("2026-04-27T08:00:00Z"),
  "leido": false,
  "tipo": "alarma"
}
```

### 🔎 Atributos:

* `_id`: ObjectId
* `usuario_id`: ObjectId
* `mensaje`: String
* `fecha_envio`: Date
* `leido`: Boolean
* `tipo`: String ("alarma", "recordatorio", "urgente")

---

# 🧩 Relación general

* Un **usuario** puede tener muchos **recordatorios**
* Un **medicamento** puede estar en muchos **recordatorios**
* Cada **recordatorio** genera múltiples registros en **historial_tomas**
* Las **notificaciones** están asociadas a usuarios

---

# 💡 Sugerencias extra

* Puedes agregar geolocalización si el reloj detecta ubicación
* Incluir sensores (ej. ritmo cardíaco)
* Manejar múltiples cuidadores (familiares)

---

Si quieres, puedo darte la **versión en SQL (MySQL/PostgreSQL)** o incluso el **modelo entidad-relación (ERD)** para este sistema.
