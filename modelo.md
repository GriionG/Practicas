Modelo 
==========================

**Modelo Vista Controlador** (MVC) es un estilo de arquitectura de software que separa los datos de una aplicación, la interfaz de usuario, y la lógica de control en tres componentes distintos.
Se trata de un modelo muy maduro y que ha demostrado su validez a lo largo de los años en todo tipo de aplicaciones, y sobre multitud de lenguajes y plataformas de desarrollo.

### Describir el modelo de representación de los datos.
El Modelo que contiene una representación de los datos que maneja el sistema, su lógica de negocio, y sus mecanismos de persistencia.

###  Enlistar las funcionalidades de la aplicación.
1. Acceder a la capa de almacenamiento de datos. Lo ideal es que el modelo sea independiente del sistema de almacenamiento.
2. Define las reglas de negocio (la funcionalidad del sistema). Un ejemplo de regla puede ser: "Si la mercancía pedida no está en el almacén, consultar el tiempo de entrega estándar del proveedor".
3. Lleva un registro de las vistas y controladores del sistema.
4. Si estamos ante un modelo activo, notificará a las vistas los cambios que en los datos pueda producir un agente externo (por ejemplo, un fichero por lotes que actualiza los datos, un temporizador que desencadena una inserción, etc.).

![Imagen MVC](https://si.ua.es/es/documentacion/asp-net-mvc-3/imagenes/introduccion/flujo-mvc.png)

### Definir la infraestructura para el almacenamiento y recuperación de datos
~~~
public ActionResult Index()
        {
            ViewBag.idcombo = new SelectList(db.TblCatalogoCombos, "IdCombo", "Nombre");

            ViewBag.idpizza = new SelectList(db.TblCatalogoCombos, "Idpizza", "NombrePizza");
            return View();
        }


Asi recibo los datos en la vista


    @Html.DropDownList("idcombo ", string.Empty)

    @Html.DropDownList("idpizza ", string.Empty)
~~~~
 
 
![mvc2](https://www.imds.org.mx/blog/wp-content/uploads/2020/03/patron-mvc-diagrama.png)

## Ejemplos de codgios MVC

>Modelo
~~~
<?php 

session_start();

@$_SESSION['Privilegio'];

?>

<?php

require 'bd/conexion_bd.php';

class Prod extends BD_PDO
{
        function Insertar($nombre, $cantidad, $proveedor)
        {
            $this->Ejecutar_Instruccion("Insert into productos(Nombre,Cantidad,id_proveedor) values ('$nombre','$cantidad','$proveedor')");
        }

        function Buscar($buscar){
            $result = $this->Ejecutar_Instruccion("select productos.Id_producto,productos.Nombre,productos.Cantidad,concat (proveedores.Nombres,' ',proveedores.Apellido_p,' ',proveedores.Apellido_m) as proveedores FROM productos INNER JOIN proveedores ON productos.id_proveedor=proveedores.id_proveedor WHERE productos.Nombre LIKE '%$buscar%'");
            return $result;
        }

        function Buscar_todo()
        {
            $result = $this ->Ejecutar_Instruccion("Select productos.id_producto,productos.Nombre,productos.Cantidad,concat(proveedores.Nombres,' ',proveedores.Apellido_p,' ',proveedores.Apellido_m) as Nombre_prov,categorias.Nombre from productos INNER JOIN proveedores ON productos.id_proveedor=proveedores.id_proveedor INNER JOIN categorias ON productos.id_categoria=categorias.id_categoria;");
            return $result;
        }
                function Eliminar($id)
        {
            $this->Ejecutar_Instruccion("Delete from productos where id_producto = '$id' ");
        }

         function Modificar( $nombre, $cantidad, $proveedor, $idproducto)
        {
            $this->Ejecutar_Instruccion("Update productos set Nombre='$nombre', Cantidad='$cantidad', id_proveedor='$proveedor' where id_producto='$idproducto'");
        }
        function ModificarBuscar($id)
        {
            $result = $this->Ejecutar_Instruccion("Select * from productos where id_producto = '$id' ");
            return $result;
        }
          public function proveedor($id)
          {
            $datos_proveedores = $this->Ejecutar_Instruccion("Select id_proveedor, concat(Nombres,' ',Apellido_p,' ',Apellido_m) as Nombre from Proveedores","Select id_proveedor from productos where id_producto='".$_GET['idmodificar']."'");
            return $datos_proveedores;
          }
        function Tabla_gen($result)
    {
        $tabla="";
        foreach ($result as $renglon) 
        {
            $tabla.="<tr>";
            $tabla.='<td>'.$renglon[0].'</td>';
            $tabla.='<td>'.$renglon[1].'</td>';
            $tabla.='<td>'.$renglon[2].'</td>';
            $tabla.='<td>'.$renglon[3].'</td>';
            
            if (@$_SESSION['Privilegio']=='Admin') 

            {
            $tabla.='<td><input type="button" id="btneliminar" class="btn btn-danger" name="btneliminar" value="Eliminar"  onclick="javascript: eliminar('.$renglon[0].');"></td>';
            $tabla.='<td><input type="button" id="btnmodificar"  class="btn btn-info" name="btnmodificar" value="Modificar" onclick="javascript: modificar('.$renglon[0].');"></td>
        
            </tr>';
        }

             }
        return $tabla;
    }
}
 
?> 
~~~

Fuentes de Informacion 
----------------------
[Modelo vista controlador](https://si.ua.es/es/documentacion/asp-net-mvc-3/1-dia/modelo-vista-controlador-mvc.html)

[Patrón de diseño MVC](https://www.imds.org.mx/blog/patron-de-diseno-mvc/)

