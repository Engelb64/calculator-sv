# Calculadora Frontend en Vue.js

Esta aplicación es una pequeña calculadora desarrollada en Vue.js, HTML, CSS y JavaScript. La calculadora permite a los usuarios realizar operaciones simples, como sumar o concatenar valores.

## Funcionalidades principales:

1. Campo de Nombre de Usuario: Permite a los usuarios ingresar su nombre para registrar la operación que realizan.

2. Campos de Operandos: Dispone de tres campos para introducir los operandos (Operando_1, Operando_2, Operando_3), que pueden contener valores numéricos o alfanuméricos.

3. Botón de Operación: Al hacer clic en el botón de operación, la aplicación realizará la suma de los operandos si todos son numéricos. Si alguno es alfanumérico, los concatenará.

4. Validación de Operandos: Los operandos Operando_1 y Operando_2 son obligatorios. Si no se proporcionan valores en ambos campos, se mostrará un mensaje de error indicando que deben completarse.

5. Registro de Operaciones: Cada operación realizada se registrará en el session store, junto con el nombre del usuario que la ejecutó.

6. Comprobación de Operaciones: Si una operación ya ha sido realizada por otro usuario anteriormente, además de mostrar el resultado, se mostrará también el nombre del usuario que la realizó previamente.

7. Persistencia de Datos: La memoria se perderá al cerrar la página por completo.

# Tecnologías Utilizadas:

- Vue.js
- HTML
- CSS
- JavaScript

# Instalación y Configuración

Instala las dependencias:

```bash
 npm install 
```
Compila y carga en caliente para desarrollo:

```bash
npm run serve
```
Este comando compilará el proyecto y abrirá una ventana del navegador donde podrás ver la aplicación en desarrollo. La página se recargará automáticamente si realizas cambios en los archivos del código fuente.

Compila y minimiza para producción:

```bash
npm run build
```

Este comando compilará y minimizará los archivos para producción, generando una versión optimizada de la aplicación lista para ser desplegada en un servidor.
