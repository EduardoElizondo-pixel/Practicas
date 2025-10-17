
<h1 align="center">🔐 Practica: Bloqueo con Contraseña en micro:bit</h1>

<p align="center">
  
</p>

<p align="center">
  <b>Autor:</b> José Eduardo Elizondo Romero<br>
  <b>Numero de Control:</b> 22210303<br>
  <b>Fecha:</b> 16 de Octubre 2025<br>
  <b>Materia:</b> Lenguajes de Programacion<br>
  <b>Institución:</b> Instituto Tecnologico de Tijuana
</p>

---

##  Descripción

El programa permite establecer una **contraseña de combinación de botones** (por defecto `A, B, A`).  
Si la secuencia se introduce correctamente, el dispositivo muestra un ícono feliz  (desbloqueado).  
Si la secuencia es incorrecta, muestra un ícono triste  y aumenta el contador de intentos fallidos.

Después de **3 intentos fallidos**, el sistema se bloquea temporalmente por **10 segundos**.

---

##  Funcionalidades principales

| Función | Descripción |
|----------|-------------|
| `showProgress(n)` | Muestra el progreso de entrada (n puntos en la fila superior). |
| `showUnlocked()` | Muestra el ícono de desbloqueo. |
| `showLocked()` | Muestra el ícono de bloqueo. |
| `resetInput()` | Reinicia la secuencia ingresada. |

---

##  Controles

- **Botón A** → Parte de la secuencia de contraseña  
- **Botón B** → Parte de la secuencia de contraseña  
- **A + B simultáneos** → Bloqueo manual inmediato  

---

##  Parámetros principales

| Variable | Descripción | Valor inicial |
|-----------|--------------|----------------|
| `PASSWORD` | Contraseña configurada | `["A", "B", "A"]` |
| `MAX_ATTEMPTS` | Máximo de intentos | `3` |
| `LOCKOUT_TIME` | Tiempo de bloqueo | `10,000 ms` |
| `TIMEOUT` | Tiempo máximo entre pulsaciones | `2,000 ms` |

---

##  Código fuente

El siguiente código debe pegarse en [MakeCode micro:bit](https://makecode.microbit.org/#editor):

```typescript

function showProgress(n: number) {
    basic.clearScreen()
    for (let i = 0; i < n && i < 5; i++) {
        led.plot(i, 0)
    }
}

function showUnlocked() {
    basic.showIcon(IconNames.Happy)
}

function showLocked() {
    basic.showIcon(IconNames.Sad)
}

input.onButtonPressed(Button.A, function () {
    if (control.millis() < lockoutUntil) return
    inputSeq.push("A")
    lastPress = control.millis()
    showProgress(inputSeq.length)
    basic.pause(150)

    if (inputSeq.length >= PASSWORD.length) {
        if (inputSeq.join("") == PASSWORD.join("")) {
            LOCKED = false
            attempts = 0
            showUnlocked()
            basic.pause(1000)
            basic.clearScreen()
        } else {
            attempts++
            basic.showIcon(IconNames.No)
            basic.pause(600)
            resetInput()
            showLocked()
            if (attempts >= MAX_ATTEMPTS) {
                lockoutUntil = control.millis() + LOCKOUT_TIME
                basic.showString("LOCK")
                showLocked()
            }
        }
    }
})

input.onButtonPressed(Button.B, function () {
    if (control.millis() < lockoutUntil) return
    inputSeq.push("B")
    lastPress = control.millis()
    showProgress(inputSeq.length)
    basic.pause(150)

    if (inputSeq.length >= PASSWORD.length) {
        if (inputSeq.join("") == PASSWORD.join("")) {
            LOCKED = false
            attempts = 0
            showUnlocked()
            basic.pause(1000)
            basic.clearScreen()
        } else {
            attempts++
            basic.showIcon(IconNames.No)
            basic.pause(600)
            resetInput()
            showLocked()
            if (attempts >= MAX_ATTEMPTS) {
                lockoutUntil = control.millis() + LOCKOUT_TIME
                basic.showString("LOCK")
                showLocked()
            }
        }
    }
})

function resetInput() {
    inputSeq = []
    lastPress = 0
}

let attempts = 0
let lastPress = 0
let lockoutUntil = 0
let LOCKOUT_TIME = 0
let LOCKED = false
let MAX_ATTEMPTS = 0
let PASSWORD: string[] = []
let inputSeq: string[] = []

PASSWORD = ["A", "B", "A"]
let TIMEOUT = 2000
MAX_ATTEMPTS = 3
LOCKED = true
LOCKOUT_TIME = 10000

showLocked()

basic.forever(function () {
    if (control.millis() < lockoutUntil) return
    if (lastPress && control.millis() - lastPress > TIMEOUT) {
        resetInput()
        showLocked()
    }

    if (input.buttonIsPressed(Button.A) && input.buttonIsPressed(Button.B)) {
        LOCKED = true
        resetInput()
        showLocked()
        basic.pause(600)
    }
})
```
##  Captura del Funcionamiento
<img width="1594" height="783" alt="3" src="https://github.com/user-attachments/assets/5d2fa530-15f6-4d87-9818-4ce4c701f427" />

https://github.com/user-attachments/assets/17ae502a-5213-48f1-b342-ae1078acdf1e





