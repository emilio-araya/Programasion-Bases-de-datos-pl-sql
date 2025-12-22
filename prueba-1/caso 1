/*
  PRUEBA 1 – CASO 1
  Sistema de cálculo de movilización por empleado

  Autor: Emilio Araya Ordoñez
  Asignatura: Programación de Bases de Datos
  Descripción:
  Proceso PL/SQL que calcula el valor de movilización de los empleados
  según su sueldo base y comuna de residencia, generando registros en
  la tabla PROY_MOVILIZACION.
*/

BEGIN
  -- Limpia la tabla de resultados para permitir múltiples ejecuciones
  DELETE FROM proy_movilizacion;

  -- Cursor implícito para recorrer empleados
  FOR emp IN (
    SELECT
      e.id_emp,
      e.pnombre_emp || ' ' || e.appaterno_emp AS nombre_empleado,
      e.sueldo_base,
      c.nombre_comuna,
      e.numrun_emp,
      e.dvrun_emp
    FROM empleado e
    JOIN comuna c ON e.id_comuna = c.id_comuna
    ORDER BY e.id_emp
  ) LOOP

    DECLARE
      v_porcentaje       NUMBER;
      v_monto_normal     NUMBER;
      v_monto_extra      NUMBER := 0;
      v_monto_total      NUMBER;
    BEGIN
      -- Porcentaje base de movilización
      v_porcentaje   := TRUNC(emp.sueldo_base / 100000);
      v_monto_normal := ROUND(emp.sueldo_base * v_porcentaje / 100);

      -- Monto extra según comuna
      IF emp.nombre_comuna = 'María Pinto' THEN
        v_monto_extra := 20000;
      ELSIF emp.nombre_comuna = 'Curacaví' THEN
        v_monto_extra := 25000;
      ELSIF emp.nombre_comuna = 'Talagante' THEN
        v_monto_extra := 30000;
      ELSIF emp.nombre_comuna = 'El Monte' THEN
        v_monto_extra := 35000;
      ELSIF emp.nombre_comuna = 'Buin' THEN
        v_monto_extra := 40000;
      END IF;

      -- Total movilización
      v_monto_total := v_monto_normal + v_monto_extra;

      -- Inserción del resultado
      INSERT INTO proy_movilizacion (
        anno_proceso,
        id_emp,
        numrun_emp,
        dvrun_emp,
        nombre_empleado,
        nombre_comuna,
        sueldo_base,
        porc_movil_normal,
        valor_movil_normal,
        valor_movil_extra,
        valor_total_movil
      ) VALUES (
        2025,
        emp.id_emp,
        emp.numrun_emp,
        emp.dvrun_emp,
        emp.nombre_empleado,
        emp.nombre_comuna,
        emp.sueldo_base,
        v_porcentaje,
        v_monto_normal,
        v_monto_extra,
        v_monto_total
      );

    END;
  END LOOP;

  COMMIT;

  DBMS_OUTPUT.PUT_LINE('Proceso de cálculo de movilización finalizado correctamente.');
END;
/




