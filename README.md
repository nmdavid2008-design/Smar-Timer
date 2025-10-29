<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <title>Smart Timer | El Tiempo es tu Maestro</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700;800&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary-color: #00e0ff; /* Cian brillante */
            --secondary-color: #63b3ff; /* Azul claro */
            --dark-bg: #030b1c; /* Fondo más oscuro */
            --light-bg: #f0f6ff;
            --text-color-dark: #052239;
            --text-color-light: #fff;
            --card-bg: #f6f9ff;
            --nav-bg: rgba(3, 11, 28, 0.9); /* Barra de navegación semitransparente */
        }

        * {margin:0;padding:0;box-sizing:border-box}
        html {scroll-behavior:smooth}
        body {
            font-family:'Poppins', sans-serif;
            background:var(--dark-bg);
            color:var(--text-color-light);
            overflow-x:hidden;
        }

        /* ===== BARRA DE NAVEGACIÓN FIJA (NUEVO) ===== */
        .navbar {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            background: var(--nav-bg);
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.5);
            z-index: 1000;
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px 40px;
            backdrop-filter: blur(5px);
            border-bottom: 1px solid rgba(0, 224, 255, 0.3);
        }
        .navbar .nav-logo {
            font-size: 1.5rem;
            font-weight: 700;
            color: var(--primary-color);
            text-decoration: none;
            text-shadow: 0 0 10px rgba(0, 224, 255, 0.5);
        }
        .navbar nav a {
            color: var(--text-color-light);
            text-decoration: none;
            margin-left: 30px;
            font-weight: 500;
            transition: color 0.3s ease, text-shadow 0.3s ease;
        }
        .navbar nav a:hover {
            color: var(--primary-color);
            text-shadow: 0 0 15px var(--primary-color);
        }

        /* ===== PORTADA - HERO SECTION (MEJORADO) ===== */
        .portada {
            position:relative;
            height:100vh;
            display:flex;
            flex-direction:column;
            justify-content:center;
            align-items:center;
            text-align:center;
            background:linear-gradient(135deg, var(--dark-bg), #142a5c);
            overflow:hidden;
            padding-top: 70px; /* Espacio para la barra de navegación */
            border-radius:0 0 50px 50px;
            box-shadow:0 15px 40px rgba(0, 0, 0, 0.4);
            animation:brillo 8s ease-in-out infinite alternate;
        }
        @keyframes brillo {
            0% {box-shadow:0 0 20px rgba(99,179,255,0.2)}
            50% {box-shadow:0 0 60px rgba(99,179,255,0.6)}
            100% {box-shadow:0 0 20px rgba(99,179,255,0.2)}
        }
        .portada::before { /* Vórtice de tiempo animado */
            content:"";
            position:absolute;
            width:200%;
            height:200%;
            top: -50%;
            left: -50%;
            background:
                radial-gradient(circle at 50% 50%, rgba(0,224,255,0.2) 1px, transparent 1%) 0 0 / 50px 50px;
            animation:vortex 30s infinite linear;
            z-index:0;
            opacity: 0.5;
        }
        @keyframes vortex {
            from {transform: rotate(0deg) scale(1)}
            to {transform: rotate(360deg) scale(1.5)}
        }

        .hero-content{
            position:relative; z-index:3; color:var(--text-color-light); max-width:900px; padding:20px;
        }
        .hero-content h1{
            font-size:clamp(3rem, 8vw, 5rem); background:linear-gradient(90deg, #ffffff, var(--secondary-color), var(--primary-color)); -webkit-background-clip:text; background-clip:text; color:transparent; font-weight:900; text-shadow:0 0 35px rgba(0, 224, 255, 0.6); margin-bottom:15px; letter-spacing: 2px;
        }
        .hero-content p{
            font-style:normal; font-size:clamp(1.1rem, 2.5vw, 1.5rem); color:#dbe8ff; margin-bottom:50px; font-weight:300;
        }
        
        /* ESTILO PARA EL RELOJ DIGITAL (NUEVO) */
        #digital-clock {
            font-family: 'Poppins', sans-serif;
            font-size: clamp(2rem, 6vw, 4rem);
            font-weight: 800;
            color: var(--primary-color);
            margin-bottom: 30px;
            padding: 10px 20px;
            border: 3px solid var(--primary-color);
            border-radius: 15px;
            background: rgba(0, 0, 0, 0.3);
            box-shadow: 0 0 30px rgba(0, 224, 255, 0.8);
            text-shadow: 0 0 10px rgba(0, 224, 255, 1);
            letter-spacing: 2px;
            animation: glow 1.5s infinite alternate;
        }
        @keyframes glow {
            from {box-shadow: 0 0 30px rgba(0, 224, 255, 0.8);}
            to {box-shadow: 0 0 40px rgba(0, 224, 255, 1);}
        }

        .btn-inicio{
            padding:16px 45px; border:none; border-radius:999px; background:linear-gradient(90deg, var(--primary-color), var(--secondary-color)); color:var(--text-color-light); font-weight:700; font-size:1.2rem; cursor:pointer; transition:all .3s ease; box-shadow:0 10px 30px rgba(0, 224, 255, 0.4); position:relative; overflow:hidden;
        }
        .btn-inicio:hover{
            transform:translateY(-5px) scale(1.05); box-shadow:0 15px 45px rgba(0, 224, 255, 0.8); background:linear-gradient(90deg, #96d0ff, #00e0ff);
        }

        /* Burbujas se mantienen igual */
        .burbuja{position:absolute; bottom:-100px; background:rgba(255,255,255,0.15); border-radius:50%; animation:flotar 20s linear infinite; z-index:1;}
        @keyframes flotar{
            0%{transform:translateY(0) scale(1);opacity:.7} 50%{transform:translateY(-400px) scale(1.2);opacity:1} 100%{transform:translateY(-800px) scale(.9);opacity:0}
        }
        .burbuja:nth-child(1){left:10%;width:60px;height:60px;animation-duration:18s;}
        .burbuja:nth-child(2){left:30%;width:40px;height:40px;animation-duration:22s;background:rgba(99,179,255,0.2);}
        .burbuja:nth-child(3){left:50%;width:90px;height:90px;animation-duration:26s;background:rgba(30,144,255,0.15);}
        .burbuja:nth-child(4){left:70%;width:50px;height:50px;animation-duration:19s;}
        .burbuja:nth-child(5){left:85%;width:75px;height:75px;animation-duration:23s;background:rgba(30,144,255,0.18);}


        /* ===== CONTENIDO y Estilos Secundarios ===== */
        .contenido {
            background:var(--light-bg); color:var(--text-color-dark); padding:80px 20px 40px; text-align:center; padding-top: 50px;
        }
        header .logo-container {
            width: 160px; height: auto; margin: 0 auto 15px; transition: transform 0.5s ease-in-out;
        }
        header .logo-container:hover {
            transform: scale(1.05);
        }
        header .logo-container img {
            width: 100%; height: auto; object-fit: contain; border-radius: 10px; box-shadow: 0 5px 15px rgba(30,144,255,0.2);
        }
        header h2 {
            font-size:clamp(1.8rem, 4vw, 2.5rem); margin-bottom:60px; font-weight:800; color: #142a5c;
        }
        .card {
            background:var(--card-bg); border-radius:25px; box-shadow:0 10px 30px rgba(30,144,255,0.15); padding:45px; margin:0 auto 40px; max-width:900px; text-align:left; border: 2px solid #e0e8f5; transition:all .4s ease;
        }
        .card:hover {
            transform:translateY(-5px) scale(1.01); border-color: var(--primary-color); box-shadow:0 15px 40px rgba(30,144,255,0.3);
        }
        .card h3 {
            text-align:left; margin-bottom:20px; font-size:clamp(1.4rem, 3vw, 1.8rem); font-weight:700; display: flex; align-items: center; gap: 15px;
        }
        .card h3 i {
            color: var(--primary-color); font-size: 1.8rem;
        }
        /* Coloración de iconos */
        .card:nth-of-type(1) h3 i {color:#00e0ff}
        .card:nth-of-type(2) h3 i {color:#00aaff}
        .card:nth-of-type(3) h3 i {color:#0077cc}
        .card:nth-of-type(4) h3 i {color:#004080}
        .card:nth-of-type(5) h3 i {color:#003366}

        .card p {
            line-height: 1.7; font-size: 1.05rem;
        }
        .card b {
            font-weight: 700; color: var(--primary-color);
        }

        /* ===== SERVICIO AL CLIENTE (Contacto) ===== */
        .servicio {
            background:var(--dark-bg); color:var(--text-color-light); padding:70px 20px; text-align:center; border-radius: 50px 50px 0 0;
        }
        .servicio h2 {color:var(--text-color-light); margin-bottom:10px; font-weight:800;}
        .servicio p {color:#a0c4ff; margin-bottom:30px; font-weight:300;}
        form {
            max-width:600px; margin:0 auto; background:#142a5c; padding:35px; border-radius:15px; box-shadow:0 10px 30px rgba(0,0,0,0.3); border: 1px solid var(--primary-color);
        }
        label {
            display:block; text-align:left; font-weight:600; margin-bottom:5px; color:#fff;
        }
        input,textarea {
            width:100%; padding:14px; margin-bottom:20px; border:2px solid #3d6098; border-radius:10px; font-size:1rem; background: #0b1d3a; color: #fff; transition: border-color .3s;
        }
        input:focus, textarea:focus {
            border-color: var(--secondary-color); outline: none;
        }
        .btn-send {
            background:linear-gradient(90deg, var(--primary-color), var(--secondary-color)); color:var(--text-color-light); border:none; padding:14px 30px; border-radius:30px; font-weight:700; cursor:pointer; transition:all .3s; box-shadow: 0 5px 15px rgba(0, 224, 255, 0.4);
        }
        .btn-send:hover {
            background:linear-gradient(90deg, #63b3ff, #00e0ff); transform: translateY(-2px);
        }

        /* ===== FOOTER ===== */
        footer {
            background:#071221; color:#a0c4ff; padding:40px 20px; text-align:center;
        }
        .social-row {
            display:flex; justify-content:center; gap:20px; margin-bottom:20px;
        }
        .social-row a {
            display:inline-flex; align-items:center; justify-content:center; width:50px; height:50px; border-radius:50%; background:var(--primary-color); color: var(--text-color-light); font-size: 1.8rem; box-shadow:0 4px 12px rgba(0, 224, 255, 0.4); transition:all .3s ease;
        }
        .social-row a:hover {
            transform:translateY(-3px) scale(1.1); background: var(--secondary-color);
        }
        .social-row a:hover i {
            color: #0b1d3a;
        }

        /* MEDIA QUERIES para responsividad */
        @media (max-width: 768px) {
            .navbar {
                flex-direction: column;
                padding: 10px 20px;
                gap: 10px;
            }
            .navbar nav a {
                margin: 0 10px;
                font-size: 0.9rem;
            }
            .navbar .nav-logo {
                margin-bottom: 5px;
            }
        }
    </style>
</head>
<body onload="updateClock()">

    <div class="navbar">
        <a href="#" class="nav-logo">SMART TIMER</a>
        <nav>
            <a href="#inicio">Inicio</a>
            <a href="#proyecto">Proyecto</a>
            <a href="#uso">Uso</a>
            <a href="#servicio">Contacto</a>
        </nav>
    </div>

    <section class="portada">
        <div class="burbuja"></div><div class="burbuja"></div><div class="burbuja"></div>
        <div class="burbuja"></div><div class="burbuja"></div><div class="burbuja"></div>

        <div class="hero-content">
            <h1>SMART TIMER</h1>
            <p>"El tiempo es tu maestro, la **eficiencia** tu legado."</p>

            <div id="digital-clock">00:00:00 AM</div>
            
            <a href="#inicio"><button class="btn-inicio"><i class="fas fa-rocket"></i> Empezar Ahora</button></a>
        </div>
    </section>

    <section id="inicio" class="contenido">
        <header>
            <div class="logo-container">
                <img src="SmartTimer.png" alt="Logo Smart Timer" class="logo-img">
            </div>
            <h2>Impulsa tu Productividad con Tecnología</h2>
        </header>

        <div class="card" id="proyecto">
            <h3><i class="fas fa-clock"></i> Tu Herramienta Inteligente de Tiempo</h3>
            <p>
                **Smart Timer** te ayuda a controlar tu tiempo de manera eficaz, mejorar tu productividad
                y alcanzar tus metas. Usa nuestras herramientas para **organizar tareas**, medir el rendimiento
                y **aprovechar cada segundo** del día. No es solo un temporizador, es un asistente de eficiencia.
            </p>
        </div>

        <div class="card">
            <h3><i class="fas fa-lightbulb"></i> Objetivo de Nuestro Proyecto: Sostenibilidad</h3>
            <p>
                Este proyecto busca diseñar e implementar un sistema de temporización para el control de **ventiladores y luces** en ambientes educativos, como salones de clase. La idea surge de la necesidad de evitar el uso innecesario de electricidad, que no solo genera un desperdicio de recursos, sino que también **incrementa los costos operativos**.<br><br>
                El sistema propuesto consiste en un temporizador programable que permite encender y apagar
                automáticamente los dispositivos, de acuerdo con la duración de las clases programadas. Esto asegura un **uso eficiente de la energía** y promueve la cultura ecológica.
            </p>
        </div>

        <div class="card" id="uso">
            <h3><i class="fas fa-list-ol"></i> Cómo Usar el Temporizador Online</h3>
            <p>
                Usar este temporizador es fácil y lo pones en marcha en solo tres pasos:
                <br><br>
                <span>**1️⃣ Establece el Tiempo:** Configura las Horas, Minutos y Segundos que necesites.</span>
                <span>**2️⃣ Controla desde tu mano:** Desde tu celular puedes darle control con nuestra aplicación.</span>
                <span>**3️⃣ ¡Inicia!** Haz clic en <b style="color: #28a745;">Iniciar temporizador</b> para comenzar.</span>
                <br>
                Mientras el temporizador está en marcha puedes: Pausar, Continuar, Reiniciar o Detener por completo.
            </p>
        </div>

        <div class="card">
            <h3><i class="fas fa-graduation-cap"></i> ¿Por Qué Deberían Usarlo los Estudiantes?</h3>
            <p>
                El uso de un temporizador digital en los salones de clase ofrece múltiples beneficios a nivel educativo, ambiental y económico.
                <br><br>
                **💰 Ahorro Energético y Económico:** Evita que los dispositivos queden encendidos innecesariamente, reduciendo el consumo eléctrico y los costos de la institución.
                <br><br>
                **⚙️ Automatización Inteligente:** El sistema se encarga de encender y apagar según el horario, liberando a estudiantes y docentes de esta tarea manual.
                <br><br>
                **🌱 Educación Ambiental:** Este proyecto impulsa el aprendizaje práctico sobre sostenibilidad y cómo la tecnología contribuye al cuidado del medio ambiente.
                <br><br>
                **🚀 Innovación en el Aula:** Demuestra cómo la tecnología moderna puede optimizar los espacios de aprendizaje, despertando el interés por la ingeniería y la automatización.
            </p>
        </div>

        <div class="card">
            <h3><i class="fas fa-city"></i> ¿Quiénes Somos?</h3>
            <p>
                En **Smart Timer** somos una empresa comprometida con la **innovación tecnológica** y la **sostenibilidad**. Nos especializamos en el diseño de sistemas digitales automatizados para optimizar el consumo energético en espacios educativos, empresariales y domésticos.<br><br>
                Nuestro principal objetivo es mejorar la eficiencia energética mediante soluciones prácticas. Creemos que la tecnología debe estar al servicio del progreso y del medio ambiente. Trabajamos para ofrecer herramientas **accesibles, eficientes y seguras**, que faciliten la vida y contribuyan al cuidado del planeta.
            </p>
        </div>
    </section>

    <section class="servicio" id="servicio">
        <h2>Servicio al Cliente: Conéctate con Nosotros</h2>
        <p>¿Tienes una pregunta, queja o sugerencia? Escríbenos y te responderemos de forma ágil por WhatsApp.</p>
        <form onsubmit="sendMessage(event)">
            <label for="name">Nombre</label>
            <input id="name" type="text" placeholder="Tu nombre" required>
            <label for="message">Mensaje</label>
            <textarea id="message" placeholder="Escribe tu queja o sugerencia..." required></textarea>
            <button type="submit" class="btn-send"><i class="fab fa-whatsapp"></i> Enviar por WhatsApp</button>
        </form>
    </section>

    <footer>
        <div class="social-row">
            <a href="https://www.instagram.com/smarttimer_?igsh=ZjIzZWVuZWZhZTN5" target="_blank" aria-label="Instagram">
                <i class="fab fa-instagram"></i>
            </a>
            <a href="https://www.facebook.com/share/15hmEpW5ih/?mibextid=wwXIfr" target="_blank" aria-label="Facebook">
                <i class="fab fa-facebook-f"></i>
            </a>
            <a href="#" target="_blank" aria-label="LinkedIn (Ejemplo de red social)">
                <i class="fab fa-linkedin-in"></i>
            </a>
        </div>
        <p>© 2025 Smart Timer — Todos los derechos reservados</p>
    </footer>

    <script>
        // Función para actualizar el reloj digital
        function updateClock() {
            const now = new Date();
            let hours = now.getHours();
            let minutes = now.getMinutes();
            let seconds = now.getSeconds();
            let ampm = hours >= 12 ? 'PM' : 'AM';

            // Convertir a formato de 12 horas
            hours = hours % 12;
            hours = hours ? hours : 12; // La hora '0' debe ser '12'

            // Añadir ceros iniciales si es necesario
            minutes = minutes < 10 ? '0' + minutes : minutes;
            seconds = seconds < 10 ? '0' + seconds : seconds;

            const timeString = `${hours}:${minutes}:${seconds} ${ampm}`;
            document.getElementById('digital-clock').textContent = timeString;
            
            // Llama a la función de nuevo cada 1 segundo
            setTimeout(updateClock, 1000);
        }

        // Función para enviar mensaje por WhatsApp
        function sendMessage(e){
            e.preventDefault();
            const name=document.getElementById("name").value.trim();
            const msg=document.getElementById("message").value.trim();
            if(!name||!msg)return alert("Por favor, completa tu Nombre y Mensaje.");

            const whatsappNumber = "573042973021";
            const text=`Hola, soy ${name}.%0A%0A${encodeURIComponent(msg)}`;
            
            window.open(`https://wa.me/${whatsappNumber}?text=${text}`,"_blank");
        }
    </script>
</body>
</html>
