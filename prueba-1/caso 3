/*
  PRUEBA 1 – CASO 3
  Listado de camiones y valor de garantía diaria

  Autor: Emilio Araya Ordoñez
  Asignatura: Programación de Bases de Datos
  Descripción:
  Bloque PL/SQL que utiliza un cursor explícito para recorrer
  la tabla CAMION y mostrar por pantalla la patente de cada
  camión junto a su valor de garantía diaria. Si el camión no
  posee garantía asignada, se indica explícitamente.
*/

DECLARE
  -- Cursor explícito para recorrer los camiones
  CURSOR c_camiones IS
    SELECT
      nro_patente,
      valor_garantia_dia
    FROM camion;

  v_valor_garantia camion.valor_garantia_dia%TYPE;

BEGIN
  DBMS_OUTPUT.PUT_LINE('Iniciando ejecución...');
  DBMS_OUTPUT.PUT_LINE('---------------------------------------------');
  DBMS_OUTPUT.PUT_LINE('        PATENTE        | VALOR GARANTIA');
  DBMS_OUTPUT.PUT_LINE('---------------------------------------------');

  -- Recorrido del cursor
  FOR cam IN c_camiones LOOP
    v_valor_garantia := cam.valor_garantia_dia;

    IF v_valor_garantia IS NULL THEN
      DBMS_OUTPUT.PUT_LINE(
        LPAD(cam.nro_patente, 20) || ' | NO TIENE GARANTIA'
      );
    ELSE
      DBMS_OUTPUT.PUT_LINE(
        LPAD(cam.nro_patente, 20) || ' | ' ||
        TO_CHAR(v_valor_garantia, '999,999')
      );
    END IF;
  END LOOP;

  DBMS_OUTPUT.PUT_LINE('---------------------------------------------');
  DBMS_OUTPUT.PUT_LINE('Ejecución completada.');
END;
/
