<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    
    <title>🍦 Helados.mix - Pedidos</title>
    <meta property="og:title" content="Helados.mix - Catálogo Interactivo">
    <meta property="og:description" content="Elegí tus sabores favoritos y hacé tu pedido online.">
    <meta property="og:image" content="https://raw.githubusercontent.com/heladosmix/index.html/main/1770160675997.jpg">
    <meta property="og:url" content="https://heladosmix.github.io/index.html/">
    <meta property="og:type" content="website">

    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;700&display=swap" rel="stylesheet">
    <style>
        :root { --primario: #ff85a2; --secundario: #fce4ec; --texto: #4a4a4a; --whatsapp: #25d366; }
        body { font-family: 'Poppins', sans-serif; background-color: var(--secundario); color: var(--texto); margin: 0; padding-bottom: 160px; }
        header { background-color: var(--primario); color: white; text-align: center; padding: 2rem 1rem; border-bottom-left-radius: 40px; border-bottom-right-radius: 40px; box-shadow: 0 4px 15px rgba(0,0,0,0.15); }
        .brand-title { font-size: 2.8rem; font-weight: 700; display: block; text-shadow: 2px 2px 4px rgba(0,0,0,0.2); }
        .container { padding: 15px; max-width: 500px; margin: auto; }
        .categoria { background: white; border-radius: 20px; padding: 20px; margin-bottom: 20px; box-shadow: 0 2px 10px rgba(0,0,0,0.05); }
        h2 { color: var(--primario); font-size: 1.3rem; margin-top: 0; border-bottom: 2px solid var(--secundario); padding-bottom: 8px; }
        .selector-tamano { display: flex; flex-direction: column; gap: 10px; margin-bottom: 15px; }
        .radio-tamano { display: flex; justify-content: space-between; align-items: center; padding: 12px; border: 2px solid var(--secundario); border-radius: 12px; cursor: pointer; position: relative; }
        input[type="radio"] { position: absolute; opacity: 0; }
        input[type="radio"]:checked + .radio-content { background-color: var(--primario); color: white; width: 100%; margin: -12px; padding: 12px; border-radius: 10px; font-weight: bold; }
        .opcion { display: flex; align-items: center; padding: 10px 0; border-bottom: 1px solid #f2f2f2; cursor: pointer; }
        input[type="checkbox"] { width: 22px; height: 22px; accent-color: var(--primario); margin-right: 15px; }
        .input-direccion { width: 100%; padding: 15px; border: 2px solid var(--secundario); border-radius: 12px; font-family: 'Poppins', sans-serif; font-size: 1rem; box-sizing: border-box; outline: none; }
        
        .btn-flotante { 
            position: fixed; bottom: 20px; left: 50%; transform: translateX(-50%); 
            background-color: var(--whatsapp); color: white; padding: 18px 30px; 
            border-radius: 50px; box-shadow: 0 8px 25px rgba(37, 211, 102, 0.5); 
            width: 90%; border: none; cursor: pointer; z-index: 1000; text-align: center;
        }
        .btn-texto-principal { font-size: 1.2rem; font-weight: 700; display: block; }
        .info-seleccion { font-size: 0.9rem; opacity: 0.95; }
        .precio-tag { float: right; font-weight: bold; }
    </style>
</head>
<body>

<header>
    <span class="brand-title">Helados.mix</span>
    <p>Catálogo Interactivo</p>
</header>

<div class="container">
    <form id="pedidoForm">
        <div class="categoria">
            <h2>📏 1. Elegí el tamaño</h2>
            <div class="selector-tamano">
                <label class="radio-tamano">
                    <input type="radio" name="tamano" value="1 KG ($14.000)" onchange="actualizarInfo()">
                    <div class="radio-content">1 KG <span class="precio-tag">$14.000</span></div>
                </label>
                <label class="radio-tamano">
                    <input type="radio" name="tamano" value="1/2 KG ($8.000)" onchange="actualizarInfo()">
                    <div class="radio-content">1/2 KG <span class="precio-tag">$8.000</span></div>
                </label>
                <label class="radio-tamano">
                    <input type="radio" name="tamano" value="1/4 KG ($4.000)" onchange="actualizarInfo()">
                    <div class="radio-content">1/4 KG <span class="precio-tag">$4.000</span></div>
                </label>
            </div>
        </div>

        <div class="categoria">
            <h2>🏠 2. Dirección de entrega</h2>
            <input type="text" id="direccion" class="input-direccion" placeholder="Ej: Calle Falsa 123, Rafael Calzada">
        </div>

        <div class="categoria">
            <h2>🍨 3. Sabores a la Crema</h2>
            <div id="lista-crema"></div>
        </div>

        <div class="categoria">
            <h2>🍋 4. Sabores al Agua</h2>
            <div id="lista-agua"></div>
        </div>

        <button type="button" class="btn-flotante" onclick="enviarWhatsApp()">
            <span class="btn-texto-principal">ENVIAR POR WHATSAPP</span>
            <span id="resumen-seleccion" class="info-seleccion">Primero elegí un tamaño</span>
        </button>
    </form>
</div>

<script>
    const saboresCrema = ["Almendrado", "Tramontana", "Dulce de leche", "Chocotorta", "Banana split", "Flan con dulce", "Chocolate", "Frutilla a la crema", "Americana", "Menta granizada", "Dulce granizado", "Vainilla", "Granizado", "Banana", "Cielo", "Chocolate al rhum", "Frutos del bosque", "Rockler", "Marroc", "Dolca", "Mouse de limon", "Pistacho", "Chantilly con frutilla", "Arcoiris"];
    const saboresAgua = ["Ananá al agua", "Frutilla al agua", "Limón al agua", "Durazno"];

    function generarLista(lista, idContenedor) {
        const contenedor = document.getElementById(idContenedor);
        lista.sort().forEach(s => {
            contenedor.innerHTML += `<label class="opcion"><input type="checkbox" name="sabor" value="${s}" onchange="actualizarInfo()"> <span>${s}</span></label>`;
        });
    }

    generarLista(saboresCrema, 'lista-crema');
    generarLista(saboresAgua, 'lista-agua');

    function actualizarInfo() {
        const tamano = document.querySelector('input[name="tamano"]:checked');
        const seleccionados = document.querySelectorAll('input[name="sabor"]:checked');
        const resumen = document.getElementById('resumen-seleccion');

        if (!tamano) {
            resumen.innerText = "Primero elegí un tamaño";
            return;
        }

        const elegidos = seleccionados.length;
        resumen.innerText = elegidos === 0 ? "Seleccioná tus sabores" : `Llevás ${elegidos} ${elegidos === 1 ? 'sabor seleccionado' : 'sabores seleccionados'}`;
    }

    function enviarWhatsApp() {
        const tamano = document.querySelector('input[name="tamano"]:checked');
        const seleccionados = document.querySelectorAll('input[name="sabor"]:checked');
        const direccion = document.getElementById('direccion').value;

        if (!tamano) { alert("Por favor, seleccioná un tamaño primero."); return; }
        if (seleccionados.length === 0) { alert("Elegí al menos un sabor."); return; }
        if (direccion.trim() === "") { alert("Por favor, ingresá tu dirección."); return; }

        let lista = "";
        seleccionados.forEach(s => lista += "✅ " + s.value + "%0A");

        const miTelefono = "5491137610574"; 
        const msj = `¡Hola *Helados.mix*! 👋%0A%0A*Pedido:* ${tamano.value}%0A*Dirección:* ${direccion}%0A%0A*Sabores:*%0A${lista}%0A_Enviado desde el catálogo interactivo_`;

        window.open(`https://wa.me/${miTelefono}?text=${msj}`, '_blank');
    }
</script>
</body>
</html>
