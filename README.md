<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>Fichado</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
body { font-family: Arial; text-align:center; padding:20px; }
input, button { padding:12px; margin:8px; font-size:16px; }
.mensaje { margin-top:20px; font-weight:bold; white-space:pre-line; }
</style>
</head>
<body>

<h2>Registro de Asistencia</h2>

<input type="text" id="nombre" placeholder="Nombre y Apellido">

<br>

<button onclick="registrar('Entrada')">🟢 Entrada</button>
<button onclick="registrar('Salida')">🔴 Salida</button>

<div class="mensaje" id="mensaje"></div>

<script>

const RADIO = 150;
const URL_SCRIPT = const URL_SCRIPT = "https://script.google.com/macros/s/AKfycbxMRm9-LHA0995SaRNfvxwKsrKgGyV5kBrpBoIx6a0W8s2n_KN_tfIQFU7nFfoZO1-CgQ/exec";


const UBICACIONES = [

{nombre:"Alvarez Thomas",lat:-34.556589,lon:-58.455239},
{nombre:"Garcia del Rio",lat:-34.555002,lon:-58.475938},
{nombre:"Serrano",lat:-34.588832,lon:-58.433279},
{nombre:"Padilla",lat:-34.591468,lon:-58.438615},
{nombre:"Arcos",lat:-34.563297,lon:-58.455874},
{nombre:"Teodoro Garcia",lat:-34.563945,lon:-58.447721},
{nombre:"Maipu Olivos",lat:-34.507911,lon:-58.486238},
{nombre:"Av Maipu Olivos",lat:-34.507633,lon:-58.485901},
{nombre:"Malvinas Argentinas",lat:-34.628173,lon:-58.393812},
{nombre:"Elcano",lat:-34.565489,lon:-58.472951},
{nombre:"Cramer 1702",lat:-34.556134,lon:-58.462843},
{nombre:"Ugarte",lat:-34.528741,lon:-58.479124},
{nombre:"Reconquista",lat:-34.601372,lon:-58.372918},
{nombre:"Varela",lat:-34.646255,lon:-58.460381},
{nombre:"Rivadavia",lat:-34.609324,lon:-58.390172},
{nombre:"Esmeralda",lat:-34.595844,lon:-58.379912},
{nombre:"La Pampa",lat:-34.553871,lon:-58.461274},
{nombre:"Cramer 1096",lat:-34.549282,lon:-58.463705},
{nombre:"Palpa",lat:-34.559991,lon:-58.479842},
{nombre:"Castex",lat:-34.575203,lon:-58.409883},
{nombre:"Roque Saenz Peña",lat:-34.470812,lon:-58.513274},
{nombre:"Cosme Beccar",lat:-34.463998,lon:-58.520391},
{nombre:"Blanco Encalada 5336",lat:-34.570411,lon:-58.510293},
{nombre:"Blanco Encalada 4541",lat:-34.568732,lon:-58.485193},
{nombre:"Barzana",lat:-34.562907,lon:-58.503281},
{nombre:"Jose Ingenieros",lat:-34.473109,lon:-58.521008},
{nombre:"Salta",lat:-34.658314,lon:-58.364991},
{nombre:"Perdriel",lat:-34.643002,lon:-58.439715},
{nombre:"Manzanares",lat:-34.553207,lon:-58.477682},
{nombre:"Ruiz Huidobro",lat:-34.546918,lon:-58.499771},
{nombre:"Pueyrredon 480",lat:-34.601900,lon:-58.397600}

];

function obtenerID(){
let id = localStorage.getItem("telefonoID");
if(!id){
id = "TEL_"+Math.random().toString(36).substr(2,9);
localStorage.setItem("telefonoID",id);
}
return id;
}

function distancia(a,b,c,d){
const R=6371000;
const dLat=(c-a)*Math.PI/180;
const dLon=(d-b)*Math.PI/180;
const x=Math.sin(dLat/2)**2+
Math.cos(a*Math.PI/180)*Math.cos(c*Math.PI/180)*
Math.sin(dLon/2)**2;
return 2*R*Math.atan2(Math.sqrt(x),Math.sqrt(1-x));
}

function registrar(tipo){

let nombre=document.getElementById("nombre").value.trim();
let mensaje=document.getElementById("mensaje");

if(nombre===""){
mensaje.innerText="❌ Ingrese su nombre";
return;
}

let telefono=obtenerID();

navigator.geolocation.getCurrentPosition(function(pos){

let sucursal="";
let permitido=false;

for(let lugar of UBICACIONES){
let d=distancia(pos.coords.latitude,pos.coords.longitude,lugar.lat,lugar.lon);
if(d<=RADIO){
permitido=true;
sucursal=lugar.nombre;
break;
}
}

if(!permitido){
mensaje.innerText="❌ Fuera de zona autorizada";
return;
}

fetch(URL_SCRIPT,{
method:"POST",
body:JSON.stringify({
fecha:new Date().toLocaleString(),
nombre:nombre,
tipo:tipo,
sucursal:sucursal,
telefono:telefono
})
})
.then(response => response.text())
.then(data => {

if(data === "BLOQUEADO"){
mensaje.innerText="❌ Este nombre ya está registrado en otro teléfono";
return;
}

mensaje.innerText="✅ "+tipo+" registrada en "+sucursal;

});

fecha:new Date().toLocaleString(),
nombre:nombre,
tipo:tipo,
sucursal:sucursal,
telefono:telefono
})
});

mensaje.innerText="✅ "+tipo+" registrada en "+sucursal;

});

}

</script>

</body>
</html>
