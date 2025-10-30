```
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Simulador Bolsa de Valores — Web</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.1/dist/chart.umd.min.js"></script>
  <style>
    body { background:#f4f5f7; color:#1e293b; font-family:Arial, sans-serif; margin:0; padding:20px; }
    table { width:100%; border-collapse:collapse; margin-bottom:20px; background:#ffffff; border-radius:10px; overflow:hidden; box-shadow:0 2px 10px rgba(0,0,0,0.05); }
    th, td { border-bottom:1px solid #e2e8f0; padding:10px; text-align:left; }
    th { color:#475569; background:#f8fafc; }
    .pill { display:inline-block; padding:4px 10px; border-radius:15px; border:1px solid #0ea5e9; color:#0ea5e9; font-size:12px; background:#e0f2fe; }
    .btn { background:#0ea5e9; border:none; color:#fff; padding:10px 20px; border-radius:10px; cursor:pointer; margin-right:10px; font-weight:bold; }
    .btn:hover { background:#0284c7; }
    input, select { padding:8px; border-radius:8px; border:1px solid #cbd5e1; margin-right:10px; }
    input:focus, select:focus { outline:none; border-color:#0ea5e9; box-shadow:0 0 3px #bae6fd; }
    .panel { margin-bottom:20px; background:#ffffff; padding:15px; border-radius:10px; box-shadow:0 2px 8px rgba(0,0,0,0.05); }
    .contenedor { display:flex; flex-wrap:wrap; gap:30px; align-items:flex-start; }
    .tabla-container { flex:1; min-width:500px; }
    .grafica-container { flex:1; min-width:400px; background:#ffffff; padding:15px; border-radius:10px; box-shadow:0 2px 8px rgba(0,0,0,0.05); }
    h1 { color:#0f172a; }
    h2 { color:#334155; }
  </style>
</head>
<body>
  <h1>Simulador de Bolsa de Valores</h1>

  <div class="panel">
    <label>Ticker: </label><input type="text" id="ticker" placeholder="AAPL">
    <label>Cantidad: </label><input type="number" id="cantidad" placeholder="10" min="1">
    <label>Divisa: </label>
    <select id="divisa" onchange="actualizarTablaDivisa()">
      <option value="MXN">MXN</option>
      <option value="USD">USD</option>
      <option value="EUR">EUR</option>
    </select>
    <button class="btn" onclick="comprarAccion()">Comprar</button>
  </div>

  <div class="contenedor">
    <div class="tabla-container">
      <table>
        <thead>
          <tr><th>Ticker</th><th>Nombre</th><th>Precio</th><th>Moneda</th></tr>
        </thead>
        <tbody id="tabla"></tbody>
      </table>
      <h2>Valor del Portafolio: <span id="valor">$0.00 MXN</span></h2>
    </div>
    <div class="grafica-container">
      <canvas id="grafica" width="500" height="400"></canvas>
    </div>
  </div>

  <script>
    const tipoCambio = { USD:18.5, EUR:19.8, MXN:1 };

    const acciones = {
      AAPL:{ nombre:'Apple Inc.', precio:190.0, moneda:'USD' },
      TSLA:{ nombre:'Tesla Motors', precio:300.0, moneda:'USD' },
      BMW:{ nombre:'Bayerische Motoren Werke AG', precio:95.0, moneda:'EUR' },
      AMXL:{ nombre:'América Móvil', precio:50.0, moneda:'MXN' },
      CEMEX:{ nombre:'Cemex S.A.B. de C.V.', precio:12.5, moneda:'MXN' },
      GOOG:{ nombre:'Alphabet Inc.', precio:140.0, moneda:'USD' },
      MSFT:{ nombre:'Microsoft Corp.', precio:380.0, moneda:'USD' },
      BIMBO:{ nombre:'Grupo Bimbo S.A.B. de C.V.', precio:90.0, moneda:'MXN' },
      WALMEX:{ nombre:'Walmart de México', precio:75.0, moneda:'MXN' },
      AMZN:{ nombre:'Amazon.com Inc.', precio:130.0, moneda:'USD' },
      KO:{ nombre:'Coca-Cola Co.', precio:60.0, moneda:'USD' },
      DIS:{ nombre:'Walt Disney Co.', precio:92.0, moneda:'USD' },
      PEMEX:{ nombre:'Pemex Energía', precio:45.0, moneda:'MXN' },
      BBVA:{ nombre:'BBVA México', precio:108.0, moneda:'MXN' }
    };

    const portafolio = { posiciones:{}, historial:[] };
    const ctx = document.getElementById('grafica').getContext('2d');
    const grafica = new Chart(ctx, {
      type:'line',
      data:{ labels:[], datasets:[{ label:'Evolución del Portafolio', data:[], borderColor:'#0ea5e9', backgroundColor:'rgba(14,165,233,0.2)', borderWidth:3, tension:0.3 }]},
      options:{ scales:{ y:{ beginAtZero:true }}, animation:{ duration:800, easing:'easeOutBounce' } }
    });

    function renderTabla(divisaBase){
      const tbody = document.getElementById('tabla');
      tbody.innerHTML='';
      for(const [tk, acc] of Object.entries(acciones)){
        const conversion = tipoCambio[acc.moneda] / tipoCambio[divisaBase];
        const precioConvertido = acc.precio * conversion;
        const fila = document.createElement('tr');
        fila.innerHTML = `<td>${tk}</td><td>${acc.nombre}</td><td>$${precioConvertido.toFixed(2)} ${divisaBase}</td><td><span class='pill'>${divisaBase}</span></td>`;
        tbody.appendChild(fila);
      }
    }

    function actualizarTablaDivisa(){
      const divisa = document.getElementById('divisa').value;
      renderTabla(divisa);
      actualizarValor(divisa);
    }

    function comprarAccion(){
      const tk = document.getElementById('ticker').value.trim().toUpperCase();
      const cantidad = parseInt(document.getElementById('cantidad').value);
      const divisaSeleccionada = document.getElementById('divisa').value;
      if(!acciones[tk]){ alert('Ticker no encontrado'); return; }
      if(isNaN(cantidad) || cantidad<=0){ alert('Cantidad inválida'); return; }
      portafolio.posiciones[tk] = (portafolio.posiciones[tk]||0)+cantidad;
      actualizarValor(divisaSeleccionada);
    }

    function actualizarValor(divisa){
      let total=0;
      for(const [tk, cant] of Object.entries(portafolio.posiciones)){
        const acc=acciones[tk];
        const conversion = tipoCambio[acc.moneda] / tipoCambio[divisa];
        const precioConvertido = acc.precio * conversion;
        total += precioConvertido*cant;
      }
      portafolio.historial.push(total);
      document.getElementById('valor').textContent = `${total.toFixed(2)} ${divisa}`;
      grafica.data.labels.push('Op '+portafolio.historial.length);
      grafica.data.datasets[0].data.push(total);
      grafica.data.datasets[0].label = `Evolución del Portafolio (${divisa})`;
      grafica.update();
    }

    // Render inicial
    renderTabla('MXN');
  </script>
</body>
</html>

```
