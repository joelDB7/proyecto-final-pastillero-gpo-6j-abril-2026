¡Excelente iniciativa! Vamos a estructurar este ecosistema de automatización basado en la metodología de agentes. Primero, configuraremos el orquestador global `.agents` y luego procederemos con la implementación técnica del **Proyecto Pastillero** en Flutter.

---

## 1. Estructura de la Habilidad Agente Global: `.agents`

Esta carpeta servirá como el "cerebro" y repositorio de herramientas para tus futuras automatizaciones.

### Archivo `SKILL.md`
Este archivo define la identidad y capacidades de tus agentes.

```markdown
# Global Agent Skill: .agents
**Propósito:** Automatización de flujos de trabajo en Diseño, Código y Scraping.

## Roles Definidos
* **Skill Diseño:** Generación de assets, paletas de colores y estructuras UI/UX.
* **Skill Código:** Generación de lógica de negocio, integración de APIs y CRUDs.
* **Skill Scraping:** Extracción de datos para alimentación de catálogos de medicamentos.

## Estructura de Directorios
* /scripts: Automatizaciones en Dart/Python para tareas repetitivas.
* /ejemplos: Templates de pantallas y modelos de datos.
* /resources: Documentación de APIs, activos gráficos y esquemas Firestore.
```

---

## 2. Prerrequisitos y Configuración del Entorno

Antes de tocar el código, aseguremos que tu terminal esté lista.

### Instalación de Herramientas
1.  **Flutter:** Descarga el SDK y añade `/bin` a tu PATH.
2.  **Firebase CLI:** Ejecuta en tu terminal:
    `npm install -g firebase-tools`
3.  **Verificación:**
    ```bash
    flutter --version
    firebase --version
    ```
4.  **Login y Conectividad:**
    `firebase login` (Sigue el enlace en el navegador).

### Configuración en Firebase Console
1.  Crea un proyecto llamado `proyectopastillero`.
2.  Habilita **Firestore Database** en modo de prueba.
3.  Registra una **App Android** con el paquete `com.ejemplo.pastillero`.
4.  Descarga el archivo `google-services.json` y colócalo en `android/app/`.

---

## 3. Estructura del Proyecto: `proyectopastillero`

Ejecuta: `flutter create proyectopastillero`. A continuación, la estructura lógica de archivos:

### Configuración `pubspec.yaml`
Añade las dependencias necesarias:
```yaml
dependencies:
  flutter:
    sdk: flutter
  firebase_core: ^latest_version
  cloud_firestore: ^latest_version
  cupertino_icons: ^1.0.2
```

---

## 4. Código del Proyecto (CRUD Pastillero)

### A. Modelo de Datos (`lib/models/medicamento.dart`)
```dart
class Medicamento {
  String id;
  String nombre;
  String dosis;
  String hora;

  Medicamento({required this.id, required this.nombre, required this.dosis, required this.hora});

  Map<String, dynamic> toMap() => {
    'nombre': nombre,
    'dosis': dosis,
    'hora': hora,
  };
}
```

### B. Servicio Firestore (`lib/services/firestore_service.dart`)
```dart
import 'package:cloud_firestore/cloud_firestore.dart';

class FirestoreService {
  final CollectionReference meds = FirebaseFirestore.instance.collection('pastillero');

  Future<void> addMed(String nombre, String dosis, String hora) {
    return meds.add({'nombre': nombre, 'dosis': dosis, 'hora': hora});
  }

  Stream<QuerySnapshot> getMeds() => meds.snapshots();

  Future<void> updateMed(String id, String nombre, String dosis, String hora) {
    return meds.doc(id).update({'nombre': nombre, 'dosis': dosis, 'hora': hora});
  }

  Future<void> deleteMed(String id) => meds.doc(id).delete();
}
```

### C. UI Principal (`lib/screens/home_screen.dart`)


```dart
import 'package:flutter/material.dart';
import '../services/firestore_service.dart';

class HomeScreen extends StatelessWidget {
  final FirestoreService firestore = FirestoreService();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("Mi Pastillero 💊")),
      body: StreamBuilder(
        stream: firestore.getMeds(),
        builder: (context, snapshot) {
          if (!snapshot.hasData) return Center(child: CircularProgressIndicator());
          var docs = snapshot.data!.docs;
          return ListView.builder(
            itemCount: docs.length,
            itemBuilder: (context, index) {
              var med = docs[index];
              return ListTile(
                title: Text(med['nombre']),
                subtitle: Text("${med['dosis']} - ${med['hora']}"),
                trailing: IconButton(
                  icon: Icon(Icons.delete, color: Colors.red),
                  onPressed: () => firestore.deleteMed(med.id),
                ),
                onLongPress: () => _showEditDialog(context, med),
              );
            },
          );
        },
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () => _showAddDialog(context),
        child: Icon(Icons.add),
      ),
    );
  }

  // Aquí irían las funciones _showAddDialog y _showEditDialog para completar el CRUD
}
```

---

## 5. Próximos Pasos con ".agents"

1.  **Skill Scraping:** Puedes crear un script en la carpeta `.agents/scripts` que extraiga información de vademécums online para autocompletar nombres de medicamentos.
2.  **Skill Diseño:** Usa la carpeta `.agents/resources` para guardar el sistema de diseño (colores suaves para salud, tipografía legible).
3.  **Skill Código:** El agente puede encargarse de generar automáticamente las validaciones de formularios en Dart.

¿Deseas que profundice en el código de los diálogos de entrada de datos o en la configuración específica del Agente de Scraping?
