<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    
    <title>🍦 Helados.mix - Pedidos</title>
    <meta property="og:title" content="Helados.mix - Catálogo Interactivo">
    <meta property="og:description" content="Elegí tus sabores favoritos y hacé tu pedido online.">
    <meta property="og:image" content="https://raw.githubusercontent.com/heladosmix/index.html/main/1770160675997.jpg">
    <meta property="og:url" content="https://heladosmix.github.io/index.html/">
    <meta property="og:type" content="website">

    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;700&display=swap" rel="stylesheet">
    <style>
        :root { --primario: #ff85a2; --secundario: #fce4ec; --texto: #4a4a4a; --whatsapp: #25d366; }
        
        body { 
            font-family: 'Poppins', sans-serif; 
            background-color: var(--secundario); 
            color: var(--texto); 
            margin: 0; 
            padding-bottom: 160px; 
        }

        header { 
            background-color: var(--primario); 
            color: white; 
            text-align: center; 
            padding: 1.5rem 1rem; 
            border-bottom-left-radius: 30px; 
            border-bottom-right-radius: 30px; 
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }

        .brand-title { font-size: 2.2rem; font-weight: 700; display: block; line-height: 1.2; }
        header p { margin: 5px 0 0; font-size: 0.9rem; opacity: 0.9; }

        .container { padding: 15px; max-width: 500px; margin: auto; }
        
        .categoria { 
            background: white; 
            border-radius: 20px; 
            padding: 20px; 
            margin-bottom: 15px; 
            box-shadow: 0 2px 10px rgba(0,0,0,0.05); 
            text-align: center; 
        }

        h2 { 
            color: var(--primario); 
            font-size: 1.1rem; 
            margin: 0 0 15px 0; 
            display: flex; 
            justify-content: center; 
            align-items: center; 
            gap: 8px; 
        }
        
        /* Imagen de los potes térmicos */
        .pote-img {
            height: auto;
            margin-bottom: 10px;
            display: block;
            margin-left: auto;
            margin-right: auto;
            filter: drop-shadow(0 4px 6px rgba(0,0,0,0.1));
        }

        .radio-tamano { 
            display: flex; 
            flex-direction: column; 
            align-items: center; 
            padding: 15px; 
            border: 1px solid #eee; 
            border-radius: 12px; 
            margin-bottom: 10px;
            cursor: pointer;
            transition: 0.2s;
        }

        .radio-tamano:hover { background-color: #fff9fa; }
        input[type="radio"] { margin-bottom: 10px; transform: scale(1.3); accent-color: var(--primario); }

        .input-direccion { 
            width: 100%; 
            padding: 12px; 
            border: 1px solid #eee; 
            border-radius: 10px; 
            font-family: inherit;
            box-sizing: border-box;
            text-align: center;
        }

        .opcion { 
            display: flex; 
            align-items: center; 
            justify-content: center; 
            padding: 10px 0; 
            border-bottom: 1px solid #f9f9f9; 
            cursor: pointer;
        }
        
        input[type="checkbox"] { width: 20px; height: 20px; margin-right: 12px; accent-color: var(--primario); }

        .btn-flotante { 
            position: fixed; bottom: 20px; left: 50%; transform: translateX(-50%); 
            background-color: var(--whatsapp); color: white; padding: 15px; 
            border-radius: 25px; box-shadow: 0 5px 20px rgba(37, 211, 102, 0.4); 
            width: 90%; max-width: 450px; border: none; cursor: pointer; z-index: 1000;
            text-align: center;
        }
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
            
            <label class="radio-tamano">
                <img src="https://cdn-icons-png.flaticon.com/512/938/938063.png" alt="1 KG" class="pote-img" style="width: 100px;">
                <input type="radio" name="tamano" value="1 KG ($14.000)" onchange="actualizarInfo()">
                <span><strong>1 KG</strong> - $14.000</span>
            </label>

            <label class="radio-tamano">
                <img src="https://cdn-icons-png.flaticon.com/512/938/938063.png" alt="1/2 KG" class="pote-img" style="width: 85px;">
                <input type="radio" name="tamano" value="1/2 KG ($8.000)" onchange="actualizarInfo()">
                <span><strong>1/2 KG</strong> - $8.000</span>
            </label>

            <label class="radio-tamano">
                <img src="https://cdn-icons-png.flaticon.com/512/938/938063.png" alt="1/4 KG" class="pote-img" style="width: 70px;">
                <input type="radio" name="tamano" value="1/4 KG ($4.000)" onchange="actualizarInfo()">
                <span><strong>1/4 KG</strong> - $4.000</span>
            </label>
        </div>

        <div class="categoria">
            <h2>🏠 2. Dirección (Opcional)</h2>
            <input type="text" id="direccion" class="input-direccion" placeholder="Ej: Calle Falsa 123 (o dejar vacío para retirar)">
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
            <strong>ENVIAR POR WHATSAPP</strong><br>
            <span id="resumen-seleccion" style="font-size: 0.8rem; font-weight: normal;">Primero elegí un tamaño</span>
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

        if (!tamano) { resumen.innerText = "Primero elegí un tamaño"; return; }
        const n = seleccionados.length;
        resumen.innerText = n === 0 ? "Seleccioná tus sabores" : `Llevás ${n} ${n === 1 ? 'sabor seleccionado' : 'sabores seleccionados'}`;
    }

    function enviarWhatsApp() {
        const tamano = document.querySelector('input[name="tamano"]:checked');
        const seleccionados = document.querySelectorAll('input[name="sabor"]:checked');
        const direccion = document.getElementById('direccion').value.trim();

        if (!tamano) { alert("Elegí un tamaño."); return; }
        if (seleccionados.length === 0) { alert("Elegí al menos un sabor."); return; }

        let lista = "";
        seleccionados.forEach(s => lista += "✅ " + s.value + "%0A");

        const dirTexto = direccion === "" ? "Retiro en local / A convenir" : direccion;

        const msj = `¡Hola *Helados.mix*! 👋%0A%0A*Pedido:* ${tamano.value}%0A*Dirección:* ${dirTexto}%0A%0A*Sabores:*%0A${lista}`;
        window.open(`https://wa.me/5491137610574?text=${msj}`, '_blank');
    }
</script>
</body>
</html>
