# <!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Página de Recompensas</title>
    <link rel="stylesheet" href="style.css">
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: flex-start; /* Cambiado para alinear a la parte superior */
            height: 100vh;
            background-color: #f0f0f0;
            font-family: Arial, sans-serif;
            padding-top: 50px; /* Agregado espacio en la parte superior */
        }
        #app {
            background: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            width: 300px;
            text-align: left; /* Cambiado para alinear texto a la izquierda */
        }
        button {
            margin-top: 10px;
            padding: 10px;
            background-color: #007bff;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        button:hover {
            background-color: #0056b3;
        }
        ul {
            list-style-type: none;
            padding: 0;
        }
        li {
            margin: 10px 0;
        }
    </style>
</head>
<body>
    <div id="app">
        <h1 id="welcome">Bienvenido</h1>
        <div id="user-form">
            <label for="name">Ingresa tu nombre:</label>
            <input type="text" id="name">
            <button onclick="createUser()">Crear usuario</button>
        </div>

        <div id="points-system" style="display: none;">
            <h2 id="greeting"></h2>
            <p id="timer">Tiempo restante: <span id="time-left">5:00:00</span></p>
            <p>Puntos actuales: <span id="points">0</span></p>
            <button onclick="redeemPoints()" id="redeem-button" style="display: none;">Reclamar puntos</button>
        </div>

        <div id="rewards-section" style="display: none;">
            <h2>Canjear Recompensas</h2>
            <ul id="rewards-list">
                <li>1. Cita Especial - <strong>500 puntos</strong> <button onclick="redeemReward(500)">Canjear</button></li>
                <li>2. Día de Películas - <strong>300 puntos</strong> <button onclick="redeemReward(300)">Canjear</button></li>
                <li>3. Actividad al Aire Libre - <strong>300 puntos</strong> <button onclick="redeemReward(300)">Canjear</button></li>
                <li>4. Regalo Sorpresa - <strong>1500 puntos</strong> <button onclick="redeemReward(1500)">Canjear</button></li>
                <li>5. Masaje o Spa en Casa - <strong>250 puntos</strong> <button onclick="redeemReward(250)">Canjear</button></li>
                <li>6. Día de Juegos - <strong>150 puntos</strong> <button onclick="redeemReward(150)">Canjear</button></li>
                <li>7. Suscripción a un Servicio - <strong>2000 puntos</strong> <button onclick="redeemReward(2000)">Canjear</button></li>
                <li>8. Desayuno en la Cama - <strong>500 puntos</strong> <button onclick="redeemReward(500)">Canjear</button></li>
                <li>9. Experiencia de Cocina - <strong>800 puntos</strong> <button onclick="redeemReward(800)">Canjear</button></li>
                <li>10. Planificación de un Viaje - <strong>10000 puntos</strong> <button onclick="redeemReward(10000)">Canjear</button></li>
            </ul>
        </div>
    </div>
    <script src="script.js"></script>
<a href="http://www.google.com.ar">enlace google</a>
</body>
</html>

let points = 0;
let lastLogin = null;
let timerInterval;

// Función para crear el usuario
function createUser() {
    const name = document.getElementById('name').value;
    if (name) {
        localStorage.setItem('userName', name);
        localStorage.setItem('points', points);
        lastLogin = new Date();
        localStorage.setItem('lastLogin', lastLogin);
        localStorage.setItem('timeRemaining', 5 * 60 * 60); // 5 horas en segundos
        startApp();
    }
}

// Función para iniciar la aplicación
function startApp() {
    const userName = localStorage.getItem('userName');
    if (userName) {
        document.getElementById('user-form').style.display = 'none';
        document.getElementById('points-system').style.display = 'block';
        document.getElementById('greeting').innerText = `Hola, ${userName}`;
        points = parseInt(localStorage.getItem('points')) || 0;
        document.getElementById('points').innerText = points;
        lastLogin = new Date(localStorage.getItem('lastLogin'));
        startTimer();
        showRewardsSection();
    }
}

// Función para iniciar el temporizador
function startTimer() {
    let timeRemaining = parseInt(localStorage.getItem('timeRemaining')) || (5 * 60 * 60); // 5 horas en segundos

    // Iniciar el temporizador solo si hay tiempo restante
    if (timeRemaining > 0) {
        timerInterval = setInterval(function () {
            if (timeRemaining > 0) {
                timeRemaining--;
                localStorage.setItem('timeRemaining', timeRemaining);
                const hours = Math.floor(timeRemaining / 3600);
                const minutes = Math.floor((timeRemaining % 3600) / 60);
                const seconds = timeRemaining % 60;
                document.getElementById('time-left').innerText = `${hours}:${minutes < 10 ? '0' : ''}${minutes}:${seconds < 10 ? '0' : ''}${seconds}`;
            } else {
                clearInterval(timerInterval);
                givePoints();
            }
        }, 1000);
    } else {
        givePoints(); // Si el tiempo ya pasó, otorgar los puntos
    }
}

// Función para otorgar puntos
function givePoints() {
    points += 3; // Se otorgan 3 puntos por cada 5 horas
    localStorage.setItem('points', points);
    localStorage.setItem('lastLogin', new Date());
    document.getElementById('points').innerText = points;
    document.getElementById('redeem-button').style.display = 'block'; // Mostrar botón para reclamar puntos
    localStorage.setItem('timeRemaining', 5 * 60 * 60); // Reiniciar el temporizador a 5 horas
    startTimer(); // Reiniciar el temporizador
    showRewardsSection(); // Asegurarse de que la sección de recompensas se muestre
}

// Función para reclamar puntos
function redeemPoints() {
    document.getElementById('redeem-button').style.display = 'none'; // Ocultar el botón una vez se reclamen los puntos
}

// Función para canjear recompensas
function redeemReward(cost) {
    if (points >= cost) {
        points -= cost;
        alert(`¡Canjeaste ${cost} puntos!`);
        localStorage.setItem('points', points);
        document.getElementById('points').innerText = points;
        showRewardsSection(); // Volver a verificar la sección de recompensas
    } else {
        alert("No tienes suficientes puntos para canjear.");
    }
}

// Función para mostrar la sección de recompensas
function showRewardsSection() {
    document.getElementById('rewards-section').style.display = points > 0 ? 'block' : 'none';
}

// Iniciar la aplicación si ya hay un usuario registrado
window.onload = function() {
    if (localStorage.getItem('userName')) {
        startApp();
    }
};
body {
    font-family: Arial, sans-serif;
    text-align: center;
    padding: 20px;
}

#user-form {
    margin-bottom: 20px;
}

#points-system {
    margin-bottom: 20px;
}

#rewards-section {
    display: none;
    margin-top: 20px;
}

.reward-item {
    margin-bottom: 10px;
    display: flex;
    justify-content: center;
    align-items: center;
}

.reward-item span {
    margin-right: 10px;
}

button {
    padding: 10px 20px;
    background-color: #007BFF;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

button:hover {
    background-color: #0056b3;
}

#redeem-button {
    display: none;
    background-color: #28a745;
}

#redeem-button:hover {
    background-color: #218838;
}

