<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Problema 6.10 - Sifón</title>
    <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
    <script id="MathJax-script" async
        src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
    </script>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        .line { margin: 10px 0; }
        .eq { color: darkblue; font-size: 18px; }
        .resultado { font-weight: bold; color: green; margin-top: 15px; }
        #nextBtn {
            margin-top: 15px;
            padding: 10px 20px;
            background-color: #007BFF;
            border: none;
            color: white;
            border-radius: 5px;
            cursor: pointer;
        }
        #nextBtn:hover { background-color: #0056b3; }
        .problema {
            background: #f8f9fa;
            border: 1px solid #ccc;
            padding: 15px;
            margin-bottom: 20px;
            border-radius: 8px;
        }
    </style>
</head>
<body>
    <h2>Problema de ejemplo 6.10 - Sifón</h2>
    <div class="problema">
        <p>
        La figura 6.8 muestra un sifón que se utiliza para sacar agua de una piscina. 
        El tubo que forma el sifón tiene un diámetro interior de 40 mm y termina en una boquilla de 25 mm de diámetro. 
        Si se supone que no hay pérdidas de energía en el sistema, calcule la rapidez del flujo de volumen a través del sifón, 
        <strong>Q</strong>, y la presión en los puntos B a E. 
        </p>
        <p>
        El primer paso para la solución consiste en calcular la rapidez del flujo de volumen, 
        <strong>Q</strong>, mediante la ecuación de Bernoulli. Los dos puntos más convenientes a utilizar son A y F. 
        </p>
    </div>

    <div id="solution"></div>
    <button id="nextBtn">Mostrar siguiente paso</button>

    <script>
        const pasos = [
            {tipo:"intro", texto:"1) Partimos de la ecuación de Bernoulli entre A y F:"},
            {tipo:"eq", linea:0, partes:["\\frac{p_A}{\\gamma} + z_A + \\frac{v_A^2}{2g} = \\frac{p_F}{\\gamma} + z_F + \\frac{v_F^2}{2g}"]},

            {tipo:"intro", texto:"2) Como p_A = 0 Pa, p_F = 0 Pa y v_A ≈ 0, se cancelan términos:"},
            {tipo:"eq", linea:1, partes:["z_A = z_F + \\frac{v_F^2}{2g}"]},

            {tipo:"intro", texto:"3) Despejamos la velocidad en F:"},
            {tipo:"eq", linea:2, partes:["v_F = \\sqrt{2g(z_A - z_F)}"]},
            {tipo:"add", linea:2, texto:"= \\sqrt{(2)(9.81)(3.0)} = 7.67 \\, m/s"},

            {tipo:"intro", texto:"4) Área de la boquilla de diámetro 25 mm:"},
            {tipo:"eq", linea:3, partes:["A_F = \\frac{\\pi (25 \\, mm)^2}{4} = 491 \\, mm^2"]},

            {tipo:"intro", texto:"5) Caudal volumétrico:"},
            {tipo:"eq", linea:4, partes:["Q = A_F v_F"]},
            {tipo:"add", linea:4, texto:"= (491 \\, mm^2)(7.67 \\, m/s) \\left(\\frac{1 \\, m^2}{10^6 \\, mm^2}\\right) = 3.77 \\times 10^{-3} \\, m^3/s"},

            {tipo:"final", texto:"Resultado final: <br><strong>Q = 3.77 × 10⁻³ m³/s</strong>"}
        ];

        const contenedor = document.getElementById('solution');
        const lineas = [];
        let pasoActual = 0;

        document.getElementById('nextBtn').addEventListener('click', () => {
            if(pasoActual >= pasos.length){
                alert("Ya se mostró toda la solución.");
                document.getElementById('nextBtn').style.display = 'none';
                return;
            }
            const paso = pasos[pasoActual];

            if(paso.tipo === "intro"){
                const p = document.createElement('p');
                p.className = 'line';
                p.textContent = paso.texto;
                contenedor.appendChild(p);
            } else if(paso.tipo === "eq"){
                const p = document.createElement('p');
                p.className = 'line eq';
                p.dataset.linea = paso.linea;
                p.innerHTML = `\\[ ${paso.partes.join(' ')} \\]`;
                contenedor.appendChild(p);
                lineas[paso.linea] = {elemento: p, partes:[...paso.partes]};
            } else if(paso.tipo === "add"){
                const linea = lineas[paso.linea];
                if (linea) {
                    linea.partes.push(paso.texto);
                    linea.elemento.innerHTML = `\\[ ${linea.partes.join(' ')} \\]`;
                }
            } else if (paso.tipo === "final") {
                const p = document.createElement('p');
                p.className = 'resultado';
                p.innerHTML = paso.texto;
                contenedor.appendChild(p);
            }
            MathJax.typesetPromise();
            pasoActual++;
        });
    </script>
</body>
</html>
