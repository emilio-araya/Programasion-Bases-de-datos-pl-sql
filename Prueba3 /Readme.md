# Prueba 3 – Automatización y Programación Avanzada en PL/SQL

Esta prueba corresponde a la tercera evaluación práctica de la asignatura
**Programación de Bases de Datos** y tiene como objetivo evaluar el uso de
**programación avanzada en PL/SQL**, orientada a la automatización de
procesos y reingeniería de un sistema de remuneraciones.

El contexto del ejercicio se basa en un sistema de atención de salud
mental, donde se requiere automatizar cálculos de haberes y beneficios
asociados a atenciones médicas.

---

## 📄 Partes desarrolladas

### 🟦 Parte 1 – Automatización de asignación por atenciones médicas
Se implementó un **disparador (TRIGGER)** que permite calcular
automáticamente la asignación por atenciones médicas cada vez que se
realiza una operación sobre la tabla `BOLETA`.

El disparador considera:
- Inserción de nuevas boletas
- Actualización del monto de boletas (solo si el monto aumenta)
- Eliminación de boletas

La asignación corresponde al **15% del monto de la boleta**, redondeado
a valores enteros.

---

### 🟦 Parte 2 – Cálculo de haberes mediante subprogramas
Se desarrollaron **funciones almacenadas**, un **procedimiento
almacenado** y un **PACKAGE**, los cuales permiten calcular los haberes
mensuales de los profesionales médicos.

Componentes implementados:
- Función para asignación por carga familiar
- Función para asignación por atenciones médicas
- Package con función para cálculo de bonificación por años trabajados
- Procedimiento almacenado principal para el cálculo de haberes
- Uso de SQL dinámico (`EXECUTE IMMEDIATE`)

Los resultados del proceso se almacenan en la tabla `HABER_CALC_MES`.

---

## 🧠 Reglas de negocio consideradas

- Asignación por carga familiar (monto fijo por carga)
- Asignación por atenciones médicas según boletas del mes
- Colación fija mensual
- Movilización calculada como porcentaje del sueldo base
- Pagos adicionales de movilización para ciertos profesionales no médicos
- Asignación por años de servicio según tramos
- Cálculo del total de haberes como suma de sueldo base y asignaciones

Todos los cálculos se realizan con valores **redondeados a enteros**.

---

## ⚙️ Características técnicas

La solución implementa:
- TRIGGERS
- FUNCIONES almacenadas
- PROCEDIMIENTOS almacenados
- PACKAGE
- SQL dinámico (`EXECUTE IMMEDIATE`)
- Manejo de errores mediante tabla de control

---

## 🧪 Ejecución

El proceso fue probado ejecutando el cálculo de haberes correspondiente
al mes de **junio del año 2021**, de acuerdo con lo solicitado en la
evaluación.

---

## 🛠️ Tecnologías utilizadas

- Oracle SQL
- PL/SQL
- SQL Developer
- Triggers
- Packages
- Funciones y Procedimientos almacenados
- SQL dinámico

---

## 🎯 Objetivo académico

Aplicar técnicas avanzadas de programación en bases de datos para:
- automatizar procesos
- mantener consistencia de datos
- implementar lógica de negocio compleja
- mejorar la eficiencia del sistema de remuneraciones

---

## 📌 Estado

✔️ Evaluada  
✔️ Aprobada  
✔️ Representativa del nivel avanzado en PL/SQL
