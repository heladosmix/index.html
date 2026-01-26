<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Helados.mix - Pedidos</title>
    <style>
        :root { --pink: #e31b6d; --light-pink: #fff0f6; --green: #25d366; }
        body { font-family: 'Segoe UI', sans-serif; background: #fdf2f8; margin: 0; padding: 15px; display: flex; justify-content: center; }
        .app-card { background: white; width: 100%; max-width: 400px; border-radius: 24px; box-shadow: 0 10px 30px rgba(0,0,0,0.1); overflow: hidden; }
        
        .header { background: white; padding: 20px; text-align: center; border-bottom: 2px solid var(--light-pink); }
        .header h1 { color: var(--pink); margin: 0; font-size: 26px; font-weight: 800; }

        .section { padding: 15px 20px; border-bottom: 1px solid #f0f0f0; }
        .section-title { font-weight: bold; font-size: 14px; color: #888; text-transform: uppercase; margin-bottom: 10px; display: block; }

        .size-grid { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 8px; }
        .size-option { display: none; }
        .size-label { background: #f8f9fa; border: 2px solid transparent; padding: 10px; border-radius: 12px; text-align: center; cursor: pointer; font-weight: bold; font-size: 13px; transition: 0.2s; }
        .size-option:checked + .size-label { border-color: var(--pink); color: var(--pink); background: var(--light-pink); }

        .flavor-list { max-height: 300px; overflow-y: auto; padding-right: 5px; }
        .flavor-item { display: flex; align-items: center; padding: 10px 0; border-bottom: 1px solid #fafafa; cursor: pointer; }
        .flavor-item input { width: 18px; height: 18px; margin-right: 12px; accent-color: var(--pink); }

        .input-address { width: 100%; padding: 12px; border: 1px solid #ddd; border-radius: 10px; box-sizing: border-box; font-size: 15px; outline: none; background: #fdfdfd; }
        .input-address:focus { border-color: var(--pink); background: white; }

        .footer { padding: 20px; background: white; }
        .btn-whatsapp { background: var(--green); color: white; border: none; width: 100%; padding: 16px; border-radius: 15px; font-size: 17px; font-weight: bold; cursor: pointer; box-shadow: 0 4px 12px rgba(37, 211, 102, 0.3); }
    </style>
</head>
<body>

<div class="app-card">
    <div class="header">
        <h1>Helados.mix 🍦</h1>
    </div>

    <div class="section">
        <span class="section-title">1. Elegí el tamaño</span>
        <div class="size-grid">
            <input type="radio" name="size" id="s1" value="1 KG" data-limit="4" checked class="size-option" onclick="resetSabores()">
            <label for="s1" class="size-label">1 KG<br><small>4 Sabores</small></label>

            <input type="radio" name="size" id="s2" value="1/2 KG" data-limit="3" class="size-option" onclick="resetSabores()">
            <label for="s2" class="size-label">1/2 KG<br><small>3 Sabores</small></label>

            <input type="radio" name="size" id="s3" value="1/4 KG" data-limit="2" class="size-option" onclick="resetSabores()">
            <label for="s3" class="size-label">1/4 KG<br><small>2 Sabores</small></label>
        </div>
    </div>

    <div class="section">
        <span class="section-title">2. Sabores (<span id="count">0</span> seleccionados)</span>
        <div class="flavor-list" id="listaSabores"></div>
    </div>

    <div class="section">
        <span class="section-title">3. Datos de entrega (Opcional)</span>
        <input type="text" id="direccion" class="input-address" placeholder="Ej: Calle 123, Depto 2B...">
    </div>

    <div class="footer">
        <button class="btn-whatsapp" onclick="enviarPedido()">
            PEDIR POR WHATSAPP 🚀
        </button>
    </div>
</div>

<script>
    const sabores = [
        "Almendrado", "Anana al agua", "Mouse de limón", "Crema Americana", "Banana", "B. Dolca",
        "Banana Split", "C. Cielo", "Lemon Pie", "D. Leche", "D.L. Granizado", "Durazno al agua",
        "Vainilla", "Arcoiris", "Ferrero Rocher", "Chocolate", "Marroc", "Chocolate al rhum",
        "Chocotorta", "Oreo", "Chantilly con frutilla", "Frutilla al agua", "Frutilla a la crema",
        "Limón al agua", "Pistacho", "Frutos del bosque", "Rockler", "Menta granizada",
        "Granizado", "Tramontana", "Flan con dulce"
    ];

    const lista = document.getElementById('listaSabores');

    sabores.forEach(s => {
        const item = document.createElement('label');
        item.className = 'flavor-item';
        item.innerHTML = `<input type="checkbox" name="sabor" value="${s}" onchange="checkLimit(this)"> <span>${s}</span>`;
        lista.appendChild(item);
    });

    function checkLimit(el) {
        const limit = parseInt(document.querySelector('input[name="size"]:checked').dataset.limit);
        const selected = document.querySelectorAll('input[name="sabor"]:checked');
        if (selected.length > limit) {
            el.checked = false;
            alert("¡Límite alcanzado para este tamaño!");
        }
        document.getElementById('count').innerText = document.querySelectorAll('input[name="sabor"]:checked').length;
    }

    function resetSabores() {
        document.querySelectorAll('input[name="sabor"]').forEach(c => c.checked = false);
        document.getElementById('count').innerText = "0";
    }

    function enviarPedido() {
        const num = "5491137610574";
        const size = document.querySelector('input[name="size"]:checked').value;
        const seleccionados = Array.from(document.querySelectorAll('input[name="sabor"]:checked')).map(s => s.value);
        const dir = document.getElementById('direccion').value.trim();

        if (seleccionados.length === 0) return alert("Seleccioná tus sabores");

        let msg = `*NUEVO PEDIDO - Helados.mix*%0A`;
        msg += `--------------------------%0A`;
        msg += `*Tamaño:* ${size}%0A`;
        
        // Solo agrega la dirección si el usuario escribió algo
        if (dir !== "") {
            msg += `*Dirección:* ${dir}%0A`;
        } else {
            msg += `*Entrega:* A coordinar / Retiro%0A`;
        }

        msg += `*Sabores:*%0A`;
        seleccionados.forEach(s => msg += `• ${s}%0A`);
        
        window.open(`https://wa.me/${num}?text=${msg}`, '_blank');
    }
</script>

</body>
</html>
