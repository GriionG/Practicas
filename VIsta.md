MVC (Vista)
==========================

**Vistas** En el patrón de controlador de vista de modelos (MVC), la vista se encarga de la presentación de los datos y de la interacción del usuario. Una vista es una plantilla HTML con marcado incrustadoRazor. Razor el marcado es código que interactúa con el marcado HTML para generar una página web que se envía al cliente.

### Caracterristicas de vistas
Recibir datos del modelo y los muestra al usuario.
Tienen un registro de su controlador asociado (normalmente porque además lo instancia).
Pueden dar el servicio de "Actualización()", para que sea invocado por el controlador o por el modelo (cuando es un modelo activo que informa de los cambios en los datos producidos por otros agentes).



![Imagen MVC](https://www.tiracodigo.com/images/mvc/uso-de-vistas-parciales/vistaparcial.jpg)

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

Fuentes de Informacion 
----------------------
[Modelo vista controlador](https://si.ua.es/es/documentacion/asp-net-mvc-3/1-dia/modelo-vista-controlador-mvc.html)


