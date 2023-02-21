Modelo Vista Contorolador 
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
> Vista
~~~

<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="utf-8">
		<meta http-equiv="X-UA-Compatible" content="IE=edge">
		<meta name="viewport" content="width=device-width, initial-scale=1">
		 <!-- The above 3 meta tags *must* come first in the head; any other head content must come *after* these tags -->

		<title>Arduinpo</title>

		<!-- icono de la pagina-->
		<link rel="icon" type="image/x-icon" href="img/logo_tl.ico" />

		<!-- Google font -->
		<link href="https://fonts.googleapis.com/css?family=Montserrat:400,500,700" rel="stylesheet">

		<!-- Bootstrap -->
		<link type="text/css" rel="stylesheet" href="css/bootstrap.min.css"/>

		<!-- Slick -->
		<link type="text/css" rel="stylesheet" href="css/slick.css"/>
		<link type="text/css" rel="stylesheet" href="css/slick-theme.css"/>

		<!-- nouislider -->
		<link type="text/css" rel="stylesheet" href="css/nouislider.min.css"/>

		<!-- Font Awesome Icon -->
		<link rel="stylesheet" href="css/font-awesome.min.css">
		<!-- Custom stlylesheet -->
		<link type="text/css" rel="stylesheet" href="css/style1.css"/>

<link type="text/css" rel="stylesheet" href="../css/bootstrap.min.css"/>

<link type="text/css" rel="stylesheet" href="../css/style1.css"/>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js%22%3E"></script>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js%22%3E"></script>

			<link href="vendor/fontawesome-free/css/all.min.css" rel="stylesheet" type="text/css">
    <link href="https://fonts.googleapis.com/css?family=Montserrat:400,700" rel="stylesheet" type="text/css">
    <link href="https://fonts.googleapis.com/css?family=Lato:400,700,400italic,700italic" rel="stylesheet" type="text/css">

			<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>



	<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min/js"></script>

	<script>
		
		function eliminar(id){
			if (confirm("¿Estas seguro de eliminar el registro?")) {

				window.location = "index.php?ideliminar=" + id;

			}
		}

		function modificar(id){
			window.location = "index.php?idmodificar=" + id;
		}

function validar()
	{
		var nombre = document.getElementById("txtnombre").value;
		var cantidad = document.getElementById("txtcantidad").value;
		var proveedor = document.getElementById("lstproveedores").value;

		if (nombre.trim().length<1) 
		{
			alert("El nombre del producto esta vacio");
			return false;
		}

		if (nombre.trim().length != nombre.length) 
		{
			alert("Tienes espacios de mas en el nombre");
			return false;
		}

		if (cantidad.trim().length<1) 
		{
			alert("La cantidad del producto esta vacia");
			return false;
		}

		if (proveedor.trim().length<1) 
		{
			alert("El proveedor esta vacios, seleccione alguno");
			return false;
		}


		return true;
	}

	    function verificar_producto(id) 
	    {
	    	$.getJSON("validaciones/verificar_producto.php?p=" + id).done(function(datos)
	    	{
	    		if (datos[0][0]>0) 
	    		{
                  alert("Producto ya existe, verifique");
	    		}
	    	});
	    } 

function check(e) {
    tecla = (document.all) ? e.keyCode : e.which;

    //Tecla de retroceso para borrar, siempre la permite
    if (tecla == 8) {
        return true;
    }

    // Patrón de entrada, en este caso solo acepta numeros y letras
    patron = /[0-9]/;
    tecla_final = String.fromCharCode(tecla);
    return patron.test(tecla_final);
}

	</script>
</head>
<body>
    <?php 
    
    ?>

<header>
			<!-- TOP HEADER -->
			<div id="top-header">
				<div class="container-fluid">
					<ul class="header-links pull-left">
					</ul>
					<ul class="header-links pull-right">
						<a href="controladores/controlador-inicio.php"><input type="submit" class=" btn btn-primary" value="inicio de sesion" ></a>
						<br><br>
						<form action="index.php" method="post">
						<input type="submit" id="btncerrar" name="btncerrar" value="Cerrar sesion" class="btn btn-danger">
					</form>
					
				</div>
			</div>
			<!-- /TOP HEADER -->

			<?php 
    
    if (@$_SESSION['Privilegio']=='Admin') 

    {

	?>
		</header>
	
	<!-- BREADCRUMB -->
    <div id="breadcrumb" class="section">
			<!-- container -->
			<div class="container">
				<!-- row -->
				<div class="row">
					<div class="col-md-12">
						<h3 class="breadcrumb-header">Productos</h3>
					</div>
				</div>
				
				<!-- /row -->
			</div>
			<!-- /container -->
		</div>
		<!-- /BREADCRUMB -->

<!-- SECTION -->
		<div class="section " >
			<!-- container -->
			<div class="container ">
				<!-- row -->
				<div class="row datos-cent">

					<div class="col-md-7">
						<!-- Billing Details -->
						<div class="billing-details">
							<div class="section-title">
								<h3 class="title">Insertar Productos</h3>
								<br><br>

	<form action="index.php" method="post" id="frminsertar" onsubmit="return validar()" name="frminsertar" >
		
		<input type="hidden" id="txtid" name="txtid"  class="input" placeholder="Numero" value="<?php echo @$prod_mod[0][0] ?>">
		<div class="form-group">
		Nombre 
		<input type="text" id="txtnombre" name="txtnombre" class="input" placeholder="Nombre"  maxlength="30" minlength="4"onblur="javascript: verificar_producto(this.value)" value="<?php echo @$prod_mod[0][1] ?>">
		</div> 
		<div class="form-group">
		Cantidad 
		<input type="text" id="txtcantidad" name="txtcantidad" class="input" placeholder="Cantidad" maxlength="7" minlength="1"  onkeypress="return check(event)" value="<?php echo @$prod_mod[0][2] ?>">
		</div>
		 <div class="form-group">
		 Proveedor 
		<select name="lstproveedores" id="lstproveedores" class="input">
			
			<option value="">Seleccione Proveedor</option>
			<?php echo $datos_proveedores; ?>
		</select>
		</div>
			<div class="container-fluid datos-cent">
		<input type="submit" id="btnregistrar" name="btnregistrar" class="btn btn-success" value="<?php 
	    if (isset($_GET['idmodificar'])){
	    	echo 'Guardar';
	       }
	    else {
	    	echo 'Insertar';
	    }?>">
	</div>

	</form>
</div>
</div>
</div>
</div>
</div>
</div>

<?php } ?>

	<!-- SECTION -->
    <div class="section">
			<!-- container -->
			<div class="container">
				<!-- row -->

						<!-- Billing Details --> 
						<div class="col-md-6">
							<div class="header-search">
								<h1>Listado de Productos</h1>
<br><br>
	<form action="index.php" id="frmbuscar" name="frmbuscar" method="post">
    	<input type="text" id="txtbuscar" name="txtbuscar" placeholder="Buscar Producto" class="input">    
    	<input type="submit" id="btnbuscar" name="btnbuscar" value="Buscar" class="btn btn-primary">
		
    	</form>
    </div>
</div>
</div>
</div>

<!-- SECTION -->
<div class="section">
			<!-- container -->
			<div class="container">
				<!-- row -->
				<div class="row datos-cent">

					<div class="col-md-12">
						<!-- Billing Details -->  
        <table border="1" class="table table-striped">

			<td>Num</td>
			<td>Nombre</td>
			<td>Cantidad</td>
			<td>Proveedor</td>
			<?php 
    
    if (@$_SESSION['Privilegio']=='Admin') 

    {

	?>
			<td colspan="2" align="center">Accion</td>

			<?php } ?>
		
        <?php echo $datos ?>
    </table>

    </div>
</div>
</div>
</div>

<!-- SECTION -->
		<div class="section">
			<!-- container -->
			<div class="container">
				<!-- row -->
				<div class="row datos-cent">

					<div class="col-md-12">
						<!-- Billing Details -->  

                        </div>
                     <!-- /Billing Details -->
   				</div>
				<!-- /row -->
			</div>
			<!-- /container -->
		</div>
		<!-- /SECTION -->
</body>
</html>
~~~
> Controlador
~~~
<?php 

	require 'modelos/modelo.php';

	$obj = new Prod();

	if (isset($_POST['btnregistrar'])) 
{

		$nombre = $_POST['txtnombre'];
		$cantidad = $_POST['txtcantidad'];
		$proveedor = $_POST['lstproveedores'];

	if ($_POST['btnregistrar']=='Insertar'){
		$obj->Insertar($nombre,$cantidad, $proveedor, $categorias);
		
    }
	elseif($_POST['btnregistrar']=='Guardar'){
		$idproducto = $_POST['txtid'];
        $prod_mod = $obj->Modificar($nombre,$cantidad, $proveedor, $idproducto);
		
	}

}

	elseif (isset($_GET['ideliminar'])) {

		$id = $_GET['ideliminar'];
		$prod_mod = $obj->Eliminar($id);
	}

	elseif (isset($_GET['idmodificar'])) {

		$id = $_GET['idmodificar'];
		$prod_mod = $obj->ModificarBuscar($id);
	}

	if (isset($_POST['btncerrar']))
	{

            session_start();

         session_destroy();

        header("Location: index.php");
		
	}
	
	if (isset($_POST['btnbuscar'])) 
	{
		$buscar = $_POST['txtbuscar'];
		$result = $obj->Buscar($buscar);
		$datos = $obj->Tabla_gen($result);
	}
	else
	{
		$result = $obj->Buscar_todo();
		$datos = $obj->Tabla_gen($result);
	}

	
	@$datos_buscar = $obj->Ejecutar_Instruccion("select productos.Id_producto,productos.Nombre,productos.Cantidad,concat (proveedores.Nombres,' ',proveedores.Apellido_p,' ',proveedores.Apellido_m) as proveedores FROM productos INNER JOIN proveedores ON productos.id_proveedor=proveedores.id_proveedor WHERE productos.Nombre LIKE '%$buscar%'");

	$datos_proveedores = $obj->listado("Select id_proveedor, concat(Nombres,' ',Apellido_p,' ',Apellido_m) as Nombre from proveedores","Select id_proveedor from productos where id_producto='".$_GET['idmodificar']."'");

	


	require 'vistas/vista.php';


?>
~~~

Fuentes de Informacion 
----------------------
[Modelo vista controlador](https://si.ua.es/es/documentacion/asp-net-mvc-3/1-dia/modelo-vista-controlador-mvc.html)

[Patrón de diseño MVC](https://www.imds.org.mx/blog/patron-de-diseno-mvc/)

