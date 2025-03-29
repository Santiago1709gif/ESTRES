
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Diagnóstico de Estrés</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background-color: #e0f7fa;
            margin: 0;
            padding: 0;
            color: #333;
        }
        header {
            background-color: #00796b;
            color: #ffffff;
            padding: 15px 0;
            text-align: center;
        }
        nav {
            margin: 20px 0;
            text-align: center;
        }
        nav a {
            margin: 0 15px;
            color: #00796b;
            text-decoration: none;
            font-weight: bold;
        }
        nav a:hover {
            text-decoration: underline;
        }
        .container {
            background-color: #ffffff;
            border-radius: 10px;
            padding: 30px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
            max-width: 800px;
            margin: auto;
        }
        h1 {
            color: #00796b;
            margin-bottom: 30px;
        }
        .question {
            margin: 15px 0;
        }
        label {
            font-weight: bold;
        }
        .emoji-scale {
            display: flex;
            justify-content: space-around;
            margin-top: 5px;
        }
        .emoji {
            font-size: 2em;
            cursor: pointer;
            opacity: 0.5;
        }
        .emoji.selected {
            opacity: 1;
        }
        button {
            background-color: #00796b;
            color: #ffffff;
            border: none;
            padding: 10px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
            width: 100%;
            margin-top: 20px;
        }
        button:hover {
            background-color: #004d40;
        }
        .result-container {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(255, 255, 255, 0.9);
            z-index: 1000;
            padding: 30px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
            overflow-y: auto;
        }
        #result {
            font-size: 2em;
            text-align: center;
            margin: 20px 0;
        }
        .recommendations {
            margin-top: 15px;
            background-color: #c8e6c9;
            padding: 10px;
            border-radius: 5px;
            text-align: center;
            max-width: 600px;
            margin-left: auto;
            margin-right: auto;
        }
    </style>
</head>
<body>

    <header>
        <h1>Diagnóstico de Estrés</h1>
    </header>
    
    <nav>
        <a href="#form">Inicio</a>
        <a href="#about">Sobre el Estrés</a>
        <a href="#contact">Contacto</a>
    </nav>

    <div class="container" id="form">
        <form id="stressForm">
            <div class="question"><label for="q1">1. ¿Cuánto estrés has sentido en la última semana?</label>
                <div class="emoji-scale">
                    <span class="emoji" data-value="0">0😃</span>
                    <span class="emoji" data-value="1">1😊</span>
                    <span class="emoji" data-value="2">2😐</span>
                    <span class="emoji" data-value="3">3😟</span>
                    <span class="emoji" data-value="4">4😣</span>
                    <span class="emoji" data-value="5">5😫</span>
                    <span class="emoji" data-value="6">6😥</span>
                    <span class="emoji" data-value="7">7😩</span>
                    <span class="emoji" data-value="8">8😢</span>
                    <span class="emoji" data-value="9">9😭</span>
                    <span class="emoji" data-value="10">10💔</span>
                </div>
                <input type="hidden" id="q1" required>
            </div>
            <div class="question"><label for="q2">2. ¿Con qué frecuencia sientes que no puedes manejar tus responsabilidades?</label>
                <div class="emoji-scale">
                    <span class="emoji" data-value="0">0😃</span>
                    <span class="emoji" data-value="1">1😊</span>
                    <span class="emoji" data-value="2">2😐</span>
                    <span class="emoji" data-value="3">3😟</span>
                    <span class="emoji" data-value="4">4😣</span>
                    <span class="emoji" data-value="5">5😫</span>
                    <span class="emoji" data-value="6">6😥</span>
                    <span class="emoji" data-value="7">7😩</span>
                    <span class="emoji" data-value="8">8😢</span>
                    <span class="emoji" data-value="9">9😭</span>
                    <span class="emoji" data-value="10">10💔</span>
                </div>
                <input type="hidden" id="q2" required>
            </div>
            <!-- Repite aquí las preguntas hasta la 30 -->
            <button type="submit">Calcular Nivel de Estrés</button>
        </form>
    </div>

    <div class="result-container" id="resultContainer">
        <div id="result"></div>
        <div class="recommendations" id="recommendations"></div>
        <button id="newTestButton">Realizar Nuevo Test</button>
    </div>

    <script>
        // Añadir evento de selección de emojis
        document.querySelectorAll('.emoji').forEach(emoji => {
            emoji.addEventListener('click', function() {
                const value = this.getAttribute('data-value');
                const inputId = this.closest('.question').querySelector('input[type="hidden"]');
                inputId.value = value;

                // Resetear la selección de otros emojis en la misma pregunta
                const emojiContainer = this.parentElement;
                emojiContainer.querySelectorAll('.emoji').forEach(e => e.classList.remove('selected'));
                this.classList.add('selected');
            });
        });

        document.getElementById('stressForm').addEventListener('submit', function(event) {
            event.preventDefault();
            let total = 0;
            for (let i = 1; i <= 2; i++) {  // Cambia '2' por '30' al agregar más preguntas
                total += parseInt(document.getElementById('q' + i).value || 0);
            }
            let average = total / 2;  // Cambia '2' por '30' al agregar más preguntas
            let stressLevel = Math.round(average);
            let message = '';
            let recommendations = '';

            switch (stressLevel) {
                case 0:
                    message = 'Nivel de Estrés: Sin Estrés. ¡Excelente!';
                    recommendations = 'Te recomendaría continuar con tus hábitos saludables y disfrutar de tus actividades favoritas. Mantén esta mentalidad positiva y busca momentos de alegría en tu vida diaria.';
                    break;
                case 1:
                    message = 'Nivel de Estrés: Muy Bajo. ¡Sigue así!';
                    recommendations = 'Practica la gratitud y sigue haciendo cosas que te alegren. Considera realizar actividades que fomenten tu bienestar emocional, como la meditación o el ejercicio.';
                    break;
                case 2:
                    message = 'Nivel de Estrés: Bajo. Buen trabajo!';
                    recommendations = 'Disfruta de tu tiempo libre y busca nuevas actividades que te interesen. Mantén una rutina que incluya ejercicio y tiempo para ti mismo, lo cual es fundamental para tu salud mental.';
                    break;
                case 3:
                    message = 'Nivel de Estrés: Bajo a Moderado. Mantente alerta.';
                    recommendations = 'Aprovecha para meditar o practicar mindfulness. Estas prácticas pueden ayudarte a manejar situaciones que te generen estrés y a mantener la calma en momentos difíciles.';
                    break;
                case 4:
                    message = 'Nivel de Estrés: Moderado. Considera relajarte.';
                    recommendations = 'Incorpora ejercicios de respiración en tu rutina diaria. Dedica unos minutos cada día para respirar profundamente y desconectarte de las preocupaciones.';
                    break;
                case 5:
                    message = 'Nivel de Estrés: Moderado. Busca equilibrio.';
                    recommendations = 'Dedica tiempo a tus pasiones y hobbies para liberar tensiones. Considera establecer límites saludables entre el trabajo y la vida personal.';
                    break;
                case 6:
                    message = 'Nivel de Estrés: Moderado a Alto. ¡Cuidado!';
                    recommendations = 'Es importante hablar sobre tus preocupaciones. Busca apoyo en amigos o familiares y considera la posibilidad de practicar técnicas de relajación más profundas.';
                    break;
                case 7:
                    message = 'Nivel de Estrés: Alto. Tómate un respiro.';
                    recommendations = 'Prioriza tu autocuidado. Puedes beneficiarte de hacer ejercicio regularmente, meditar o incluso practicar yoga para reducir el estrés acumulado.';
                    break;
                case 8:
                    message = 'Nivel de Estrés: Muy Alto. Actúa ahora.';
                    recommendations = 'Es crucial que hables con un profesional de la salud mental. Ellos pueden ofrecerte herramientas y estrategias para manejar el estrés de manera efectiva.';
                    break;
                case 9:
                    message = 'Nivel de Estrés: Crítico. No estás solo.';
                    recommendations = 'Busca grupos de apoyo o terapia. Compartir tus experiencias con otros puede ser un paso importante hacia la sanación.';
                    break;
                case 10:
                    message = 'Nivel de Estrés: Crítico. Prioriza tu bienestar.';
                    recommendations = 'Es vital buscar ayuda profesional de inmediato. No dudes en acudir a un terapeuta o consejero que pueda guiarte en este momento difícil.';
                    break;
                default:
                    message = 'Nivel de Estrés: No determinado. Verifica tus respuestas.';
                    recommendations = '';
            }

            document.getElementById('result').innerText = `${message} (Puntuación: ${stressLevel})`;
            document.getElementById('recommendations').innerText = recommendations;

            // Mostrar la pantalla de resultados
            document.getElementById('form').style.display = 'none';
            document.getElementById('resultContainer').style.display = 'block';
        });

        document.getElementById('newTestButton').addEventListener('click', function() {
            // Reiniciar el formulario
            document.getElementById('stressForm').reset();
            document.getElementById('form').style.display = 'block';
            document.getElementById('resultContainer').style.display = 'none';
            document.querySelectorAll('.emoji').forEach(e => e.classList.remove('selected')); // Resetear selección de emojis
        });
    </script>

</body>
</html>
