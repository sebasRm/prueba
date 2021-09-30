<?php

include 'conexion.php';
$datos = json_decode(file_get_contents('php://input'), true);
if (isset($datos['buscarEstudiantesVacunados']))
{
    $json=[];
    $buscarEstudiantes=$datos['buscarEstudiantesVacunados'];
    $sentencia=$conexion->prepare("CALL clase_covid.consultas_estudiantes_vacunados(?);");
    $sentencia->bind_param('s',$buscarEstudiantes);
    $res=$sentencia->execute();
    if($res)
    {
        $resultado = $sentencia->get_result();
        foreach($resultado as $row)
        {
            $json[] = $row;
        }
    }
    $err=mysqli_error($conexion);
    $sentencia->close();
    $conexion->close();
    echo json_encode(array( 'respuesta' => $res,'error' =>$err,'datos' => $json));
    exit();
}

?