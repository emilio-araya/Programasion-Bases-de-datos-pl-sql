/*
  PRUEBA 2 – CÁLCULO DE BENEFICIOS MENSUALES
  Autor: Emilio Araya Ordoñez
  Asignatura: Programación de Bases de Datos

  Descripción:
  Bloque PL/SQL anónimo que calcula los beneficios mensuales
  de los empleados de un sistema de vacunatorios, aplicando
  múltiples reglas de negocio y generando información detallada,
  resumida y de control de errores.
*/

DECLARE
  -- Parámetros del proceso
  p_fecha_proceso        DATE   := TO_DATE('01/02/2023', 'DD/MM/YYYY');
  p_limite_capacitacion  NUMBER := 450000;

  -- VARRAY de valores fijos
  TYPE t_valores_fijos IS VARRAY(2) OF NUMBER;
  v_valores_fijos t_valores_fijos;

  -- Cursor principal de empleados
  CURSOR c_empleados IS
    SELECT
      e.numrun_emp,
      e.dvrun_emp,
      e.appaterno_emp,
      e.apmaterno_emp,
      e.nombre_emp,
      e.fecing_emp,
      e.sueldo_base_emp,
      e.id_vacunatorio,
      e.id_escolaridad,
      e.cod_afp,
      e.cod_salud,
      (SELECT COUNT(*)
       FROM carga_familiar cf
       WHERE cf.numrun_emp = e.numrun_emp) AS total_cargas_familiares
    FROM empleado e
    ORDER BY e.id_vacunatorio;

  -- Excepción personalizada
  e_limite_excedido EXCEPTION;
  PRAGMA EXCEPTION_INIT(e_limite_excedido, -20001);

  -- Variables generales
  v_mes_proceso  NUMBER;
  v_anno_proceso NUMBER;

  v_valor_asig_annos     NUMBER;
  v_valor_cargas_fam    NUMBER;
  v_test_pcr             NUMBER;
  v_valor_capacitacion  NUMBER;
  v_valor_colacion      NUMBER;
  v_valor_asig_escolar  NUMBER;
  v_valor_tot_haberes   NUMBER;
  v_descuento_afp       NUMBER;
  v_descuento_salud     NUMBER;

  v_annos_trabajados NUMBER;
  v_porc_bonif       NUMBER;
  v_porc_escolaridad NUMBER;
  v_porc_descto_afp NUMBER;
  v_porc_descto_salud NUMBER;
  v_porc_hr          NUMBER;

  v_valor_hora_extra NUMBER;
  v_total_bonif_capac NUMBER;

  v_numrun_emp_txt VARCHAR2(15);

  -- Variables de resumen por vacunatorio
  v_current_vacunatorio_id NUMBER := -1;
  v_total_emp_vac NUMBER := 0;
  v_total_emp_asig_annos_vac NUMBER := 0;
  v_total_emp_asig_cfam_vac NUMBER := 0;
  v_total_emp_test_pcr_vac NUMBER := 0;
  v_valor_tot_haberes_vac NUMBER := 0;

BEGIN
  DBMS_OUTPUT.ENABLE(NULL);

  -- Limpieza de tablas
  EXECUTE IMMEDIATE 'TRUNCATE TABLE BENEFICIOS_CALC_MES';
  EXECUTE IMMEDIATE 'TRUNCATE TABLE RESUMEN_BENEFICIOS_MES';
  EXECUTE IMMEDIATE 'TRUNCATE TABLE ERROR_PROCESO';

  -- Inicialización de valores fijos
  v_valores_fijos := t_valores_fijos(85000, 9500);

  v_mes_proceso  := TO_NUMBER(TO_CHAR(p_fecha_proceso, 'MM'));
  v_anno_proceso := TO_NUMBER(TO_CHAR(p_fecha_proceso, 'YYYY'));
  v_valor_colacion := ROUND(v_valores_fijos(2));

  -- Procesamiento de empleados
  FOR r_empleado IN c_empleados LOOP

    -- Reset variables
    v_valor_asig_annos := 0;
    v_valor_cargas_fam := 0;
    v_test_pcr := 0;
    v_valor_capacitacion := 0;
    v_valor_asig_escolar := 0;
    v_valor_tot_haberes := 0;
    v_descuento_afp := 0;
    v_descuento_salud := 0;
    v_valor_hora_extra := 0;
    v_total_bonif_capac := 0;

    v_numrun_emp_txt := r_empleado.numrun_emp || '-' || r_empleado.dvrun_emp;

    -- Cambio de vacunatorio
    IF r_empleado.id_vacunatorio != v_current_vacunatorio_id THEN
      IF v_current_vacunatorio_id != -1 THEN
        INSERT INTO RESUMEN_BENEFICIOS_MES
        VALUES (
          v_current_vacunatorio_id,
          v_mes_proceso,
          v_anno_proceso,
          v_total_emp_vac,
          v_total_emp_asig_annos_vac,
          v_total_emp_asig_cfam_vac,
          v_total_emp_test_pcr_vac,
          v_valor_tot_haberes_vac
        );
      END IF;

      v_current_vacunatorio_id := r_empleado.id_vacunatorio;
      v_total_emp_vac := 0;
      v_total_emp_asig_annos_vac := 0;
      v_total_emp_asig_cfam_vac := 0;
      v_total_emp_test_pcr_vac := 0;
      v_valor_tot_haberes_vac := 0;
    END IF;

    v_total_emp_vac := v_total_emp_vac + 1;

    -- Cargas familiares
    v_valor_cargas_fam := ROUND(r_empleado.total_cargas_familiares * v_valores_fijos(1));
    v_test_pcr := r_empleado.total_cargas_familiares;

    IF r_empleado.total_cargas_familiares > 0 THEN
      v_total_emp_asig_cfam_vac := v_total_emp_asig_cfam_vac + 1;
      v_total_emp_test_pcr_vac := v_total_emp_test_pcr_vac + r_empleado.total_cargas_familiares;
    END IF;

    -- Asignación por años
    BEGIN
      v_annos_trabajados := FLOOR(MONTHS_BETWEEN(p_fecha_proceso, r_empleado.fecing_emp) / 12);

      SELECT porc_bonif
      INTO v_porc_bonif
      FROM porc_bonif_annos_continuos
      WHERE v_annos_trabajados BETWEEN annos_min AND annos_max;

      v_valor_asig_annos := ROUND(r_empleado.sueldo_base_emp * v_porc_bonif / 100);
      v_total_emp_asig_annos_vac := v_total_emp_asig_annos_vac + 1;

    EXCEPTION
      WHEN NO_DATA_FOUND THEN
        v_valor_asig_annos := 0;
        INSERT INTO ERROR_PROCESO
        VALUES (
          SEQ_ERROR_PROCESO.NEXTVAL,
          'Asignacion annos emp ' || v_numrun_emp_txt,
          'No hay datos para años trabajados'
        );
    END;

    -- Asignación escolaridad
    BEGIN
      SELECT porc_asig_escolaridad
      INTO v_porc_escolaridad
      FROM asig_escolaridad
      WHERE id_escolaridad = r_empleado.id_escolaridad;

      v_valor_asig_escolar := ROUND(r_empleado.sueldo_base_emp * v_porc_escolaridad / 100);

    EXCEPTION
      WHEN NO_DATA_FOUND THEN
        v_valor_asig_escolar := 0;
    END;

    -- Capacitación y horas extra
    v_valor_capacitacion := CASE r_empleado.id_vacunatorio
      WHEN 10 THEN 100000
      WHEN 20 THEN 150000
      WHEN 30 THEN 200000
      WHEN 40 THEN 250000
      ELSE 0
    END;

    SELECT porc_hr
    INTO v_porc_hr
    FROM porc_hora_extra
    WHERE r_empleado.sueldo_base_emp BETWEEN sueldo_base_inf AND sueldo_base_sup;

    v_valor_hora_extra := ROUND(r_empleado.sueldo_base_emp * v_porc_hr / 100);
    v_total_bonif_capac := v_valor_hora_extra + v_valor_capacitacion;

    IF v_total_bonif_capac > p_limite_capacitacion THEN
      INSERT INTO ERROR_PROCESO
      VALUES (
        SEQ_ERROR_PROCESO.NEXTVAL,
        'Capacitacion/Horas extra emp ' || v_numrun_emp_txt,
        'Exceso de tope permitido'
      );

      v_valor_capacitacion := p_limite_capacitacion - v_valor_hora_extra;
      IF v_valor_capacitacion < 0 THEN
        v_valor_capacitacion := 0;
      END IF;
    END IF;

    -- Descuentos
    SELECT porc_descto_afp
    INTO v_porc_descto_afp
    FROM afp
    WHERE cod_afp = r_empleado.cod_afp;

    v_descuento_afp := ROUND(
      (r_empleado.sueldo_base_emp + v_valor_cargas_fam + v_valor_asig_escolar)
      * v_porc_descto_afp / 100
    );

    SELECT porc_descto_salud
    INTO v_porc_descto_salud
    FROM salud
    WHERE cod_salud = r_empleado.cod_salud;

    v_descuento_salud := ROUND(
      (r_empleado.sueldo_base_emp + v_valor_asig_annos)
      * v_porc_descto_salud / 100
    );

    -- Total haberes
    v_valor_tot_haberes := ROUND(
      r_empleado.sueldo_base_emp
      + v_valor_cargas_fam
      + v_valor_colacion
      + v_valor_asig_annos
      + v_valor_asig_escolar
      + v_valor_capacitacion
      + v_valor_hora_extra
      - v_descuento_afp
      - v_descuento_salud
    );

    INSERT INTO BENEFICIOS_CALC_MES
    VALUES (
      v_mes_proceso,
      v_anno_proceso,
      v_numrun_emp_txt,
      r_empleado.id_vacunatorio,
      ROUND(r_empleado.sueldo_base_emp),
      v_valor_asig_annos,
      v_valor_cargas_fam,
      v_test_pcr,
      v_valor_capacitacion,
      v_valor_colacion,
      v_valor_asig_escolar,
      v_valor_tot_haberes
    );

    v_valor_tot_haberes_vac := v_valor_tot_haberes_vac + v_valor_tot_haberes;

  END LOOP;

  -- Inserción último resumen
  INSERT INTO RESUMEN_BENEFICIOS_MES
  VALUES (
    v_current_vacunatorio_id,
    v_mes_proceso,
    v_anno_proceso,
    v_total_emp_vac,
    v_total_emp_asig_annos_vac,
    v_total_emp_asig_cfam_vac,
    v_total_emp_test_pcr_vac,
    v_valor_tot_haberes_vac
  );

  COMMIT;

  DBMS_OUTPUT.PUT_LINE('Proceso de cálculo de beneficios finalizado correctamente.');

END;
/
