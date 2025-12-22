# Prueba 2 – Proceso PL/SQL de Cálculo de Beneficios

Esta prueba corresponde a la segunda evaluación práctica de la asignatura
**Programación de Bases de Datos** y tiene como objetivo evaluar el
desarrollo de **procesos PL/SQL de complejidad intermedia y avanzada**,
integrando múltiples reglas de negocio y control de errores.

El ejercicio se desarrolla en el contexto de un sistema de vacunatorios,
donde se requiere calcular beneficios mensuales para los empleados.

---

## 📄 Caso desarrollado

### 🟦 Caso único – Cálculo de beneficios mensuales
Se implementó un **bloque PL/SQL anónimo** que calcula los beneficios de
todos los empleados para un mes y año determinados, generando información
detallada y resumida.

El proceso almacena los resultados en las siguientes tablas:
- `BENEFICIOS_CALC_MES`
- `RESUMEN_BENEFICIOS_MES`
- `ERROR_PROCESO`

---

## 🧠 Reglas de negocio consideradas

- Asignación por cargas familiares
- Entrega de test PCR por cada carga familiar
- Colación fija por turno
- Asignación por años continuos de trabajo
- Asignación por escolaridad
- Bono por horas extra según tramos salariales
- Valor de capacitación según vacunatorio
- Tope máximo para capacitación y horas extra
- Descuentos por AFP
- Descuentos por Salud
- Cálculo del total de haberes

Todos los cálculos se realizan con valores **redondeados a enteros**.

---

## ⚙️ Características técnicas

El proceso incluye:

- Uso de cursores
- Variables BIND para parámetros de entrada
- Uso de VARRAY para valores fijos
- Manejo de excepciones definidas por el usuario
- Manejo de excepciones predefinidas de Oracle
- Truncado dinámico de tablas antes de la ejecución
- Generación simultánea de información detallada y resumida

---

## 🧪 Ejecución

El proceso fue probado ejecutando el cálculo de beneficios para el mes de
**febrero del año 2023**, de acuerdo con lo solicitado en la evaluación.

---

## 🛠️ Tecnologías utilizadas

- Oracle SQL
- PL/SQL
- SQL Developer
- Cursores
- Excepciones
- VARRAY

---

## 🎯 Objetivo académico

Aplicar técnicas de programación en bases de datos para resolver un
problema de negocio complejo, priorizando:
- claridad del código
- correcta implementación de reglas de negocio
- control de errores
- consistencia en los resultados

---

## 📌 Estado

✔️ Evaluada  
✔️ Aprobada  
✔️ Representativa del nivel intermedio/avanzado en PL/SQL
