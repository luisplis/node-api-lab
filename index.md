
## Ejercicio: abrir y entender la aplicación del servidor

En el Explorador de Archivos, abre la carpeta mxpfu-nodejsLabs y visualiza index.js.

Tienes un servidor Express que ha sido configurado para ejecutarse en el puerto 5000. Cuando accedes al servidor con /user, puedes acceder a los endpoints definidos en routes/users.js.

Recuerda que GET, POST, PUT y DELETE son los métodos HTTP comúnmente utilizados para realizar operaciones CRUD. Estas operaciones recuperan y envían datos al servidor.

- GET se utiliza para solicitar datos de un recurso específico.
- POST se utiliza para enviar datos a un servidor para crear un recurso.
- PUT se utiliza para enviar datos a un servidor para actualizar un recurso.
- DELETE se utiliza para eliminar un recurso específico.
- POST Y PUT a veces se utilizan de manera intercambiable.

Este laboratorio requiere que se instalen algunos paquetes. El paquete express y nodemon para iniciar y ejecutar el servidor Express, y jsonwebtoken y  express-session para la autenticación basada en sesiones.
- express -- Esto es para crear un servidor que sirva los endpoints de la API.
- nodemon -- Esto ayudará a reiniciar el servidor cuando realices cambios en el código.
- jsonwebtoken -- Este paquete ayuda a generar un token web JSON que utilizaremos para la autenticación. Un token web JSON (JWT) es un objeto JSON utilizado para comunicar información de manera segura a través de internet (entre dos partes). Se puede utilizar para el intercambio de información y se usa típicamente en sistemas de autenticación.

express-session - Este paquete nos ayudará a mantener la autenticación para la sesión.

Estos paquetes están definidos como dependencias en packages.json.

```
"dependencies": {
    "express": "^4.18.1",
    "express-session": "^1.17.3",
    "jsonwebtoken": "^8.5.1",
    "nodemon": "^2.0.19"
}
app.use(express.json());
app.use("/user", routes);
```

Todos los puntos finales tienen una implementación básica, pero funcional en users.js. Navega a users.js en el directorio routes y observa los puntos finales definidos en él.

---

## Ejercicio: Ejecutar el servidor

El código inicial proporcionado es un servidor en funcionamiento con valores de retorno ficticios.

En la terminal, imprime el directorio de trabajo para asegurarte de que estás en /home/projects/mxpfu-nodejsLabs.

```pwd```

Instala todos los paquetes necesarios para ejecutar el servidor. Copia, pega y ejecuta el siguiente comando. Esto instalará todos los paquetes requeridos según lo definido en packages.json.


```npm install```

Inicie el servidor express.

```npm start```

Abre un Nuevo Terminal desde el menú superior. Prueba un endpoint para recuperar estos usuarios. Esto aún no se ha implementado para devolver los usuarios.

```curl localhost:5000/user```

Si ves la salida como se muestra arriba, significa que el servidor está funcionando como se esperaba.

---

### Código: Implementar endpoints GET

Implementaré los endpoints para recuperar, crear, actualizar y eliminar usuarios.

En la terminal, navega al directorio routes y observa los puntos finales definidos en users.js.

```cd routes```

```ls```

```cat users.js```

>***Ejemplo (cRud)***
>
> Navega al archivo llamado users.js en la carpeta routes. Los endpoints han sido definidos y se ha proporcionado espacio para que implementes los endpoints.
>
> R en CRUD significa recuperar. Primero agregarás un endpoint de API, utilizando el método get para obtener los detalles de todos los usuarios. Se han agregado algunos usuarios en el código inicial.
>
> Copia el código a continuación y pégalo en users.js dentro de los corchetes { } dentro del método router.get(“/“,(req,res)=>{}.
>
>```javascript
>res.send(users);
>``` 

Asegúrate de que tu servidor esté en funcionamiento. A medida que realices cambios en el código, el servidor que iniciaste en la tarea anterior debe reiniciarse. Si el servidor no está en funcionamiento, inícialo nuevamente.

```npm start```

**Haz clic en el botón Skills Network a la izquierda. Se abrirá la "Skills Network Toolbox". Luego haz clic en OTHER y después en Launch Application. Desde allí deberías poder ingresar el puerto como 5000 y lanzar el servidor de desarrollo.**

Cuando se abra la página del navegador, añade /user al final de la URL en la barra de direcciones. Verás la página a continuación. 

Verifica la salida de la solicitud GET utilizando el comando curl de la misma manera que lo hiciste en el ejercicio anterior.

```curl localhost:5000/user/```


Ahora, implementa un método get para obtener los detalles de un usuario específico basado en su ID de correo electrónico utilizando el método filter en la colección de usuarios.

>***Ejemplo (cRud)***
>
>Creación de un método GET por correo electrónico específico:
>
>```javascript
>router.get("/:email",(req,res)=>{
>    // Extract the email parameter from the request URL
>    const email = req.params.email;
>    // Filter the users array to find users whose email matches the extracted email parameter
>    let filtered_users = users.filter((user) => user.email === email);
>    // Send the filtered_users array as the response to the client
>    res.send(filtered_users);
>});
>```

En el terminal, utiliza el siguiente comando para ver la salida del usuario con el correo johnsmith@gamil.com

```curl localhost:5000/user/johnsmith@gamil.com```

### Código: Implementar endpoint POST

Implementa el endpoint /user con el método POST para crear un usuario y añadirlo a la lista. Puedes crear el objeto usuario como un diccionario. Puedes usar el objeto usuario de ejemplo que se muestra a continuación.

```
{
  "firstName":"Jon",
  "lastName":"Lovato",
  "email":"jonlovato@theworld.com",
  "DOB":"10/10/1995"
}
```

>***Ejemplo (Crud)***
>
>```javascript
>router.post("/",(req,res)=>{
>    // Push a new user object into the users array based on query parameters from the request
>    users.push({
>        "firstName": req.query.firstName,
>        "lastName": req.query.lastName,
>        "email": req.query.email,
>        "DOB": req.query.DOB
>    });
>    // Send a success message as the response, indicating the user has been added
>    res.send("The user " + req.query.firstName + " has been added!");
>});
>```

Utiliza el siguiente comando para publicar un nuevo usuario con el correo en la nueva terminal:

```
curl --request POST 'localhost:5000/user?firstName=Jon&lastName=Lovato&email=jonlovato@theworld.com&DOB=10/10/1995
```

Para verificar si el usuario con el correo electrónico ‘jonlovato@theworld.com‘ ha sido añadido, puedes enviar una solicitud GET por curl en otra terminal o en el navegador por la URL:

```
curl localhost:5000/user/jonlovato@theworld.com
http://localhost:5000/user
```


### Código: Implementar endpoint PUT

Para hacer actualizaciones en los datos, utilizarás el método PUT. Primero debes buscar al usuario con el id de correo electrónico especificado y luego modificarlo. El código a continuación muestra cómo se puede modificar la fecha de nacimiento (DOB) de un usuario. Realiza los cambios de código necesarios para permitir modificaciones a los otros atributos del usuario.

>***Ejemplo (crUd)***
>
>```javascript
>router.put("/:email", (req, res) => {
>    // Extract email parameter and find users with matching email
>    const email = req.params.email;
>    let filtered_users = users.filter((user) => user.email === email);
>    
>    if (filtered_users.length > 0) {
>        // Select the first matching user and update attributes if provided
>        let filtered_user = filtered_users[0];
>        
>         // Extract and update DOB if provided
>        
>        let DOB = req.query.DOB;    
>        if (DOB) {
>            filtered_user.DOB = DOB;
>        }        
>        /*
>        Include similar code here for updating other attributes as needed
>        */
>
>        // Replace old user entry with updated user
>        users = users.filter((user) => user.email != email);
>        users.push(filtered_user);
>        
>        // Send success message indicating the user has been updated
>        res.send(`User with the email ${email} updated.`);
>    } else {
>        // Send error message if no user found
>        res.send("Unable to find user!");
>    }
>});
>```

Utiliza el siguiente comando para actualizar el DOB a 1/1/1971 para el usuario con el correo electrónico 'johnsmith@gamil.com' en la terminal dividida:

```curl --request PUT 'localhost:5000/user/johnsmith@gamil.com?DOB=1/1/1971'```

Para verificar si la DOB del usuario con el correo electrónico ‘johnsmith@gamil.com‘ ha sido actualizada, puedes enviar una solicitud GET como se muestra a continuación:

```curl localhost:5000/user/johnsmith@gamil.com```

### Código: Implementar endpoint DELETE

Implementa el método DELETE para eliminar el correo electrónico de un usuario específico utilizando el siguiente código:

>***Ejemplo (cruD)***
>
>```javascript
>router.delete("/:email", (req, res) => {
>    // Extract the email parameter from the request URL
>    const email = req.params.email;
>    // Filter the users array to exclude the user with the specified email
>    users = users.filter((user) => user.email != email);
>    // Send a success message as the response, indicating the user has been deleted
>    res.send(`User with the email ${email} deleted.`);
>});
>```

---

## Implementando Autenticación (ejercicio práctico)

Todos estos puntos finales son accesibles por cualquier persona. Ahora verás cómo agregar autenticación a las operaciones CRUD. Este código ha sido implementado en index_withauth.js.

```javascript
app.use(session({secret:"fingerprint",resave: true, saveUninitialized: true}))
```

Esto le indica a su aplicación express que use el middleware de sesión.

- secret -- una clave única aleatoria utilizada para autenticar una sesión.
- resave -- toma un valor booleano. Permite que la sesión se almacene nuevamente en el almacenamiento de sesiones, incluso si la sesión nunca fue modificada durante la solicitud.
- saveUninitialized -- esto permite que cualquier sesión no inicializada se envíe al almacenamiento. Cuando se crea una sesión pero no se modifica, se le denomina no inicializada.

>El valor predeterminado tanto de resave como de saveUninitialized es verdadero, pero el valor predeterminado está en desuso. Por lo tanto, establezca el valor apropiado según el caso de uso.

Observe la implementación del endpoint de login. Un usuario inicia sesión en el sistema proporcionando un nombre de usuario. Se genera un token de acceso que es válido por una hora. Puede observar esta duración de validez especificada por 60 * 60, que significa el tiempo en segundos. Este token de acceso se establece en el objeto de sesión para garantizar que solo los usuarios autenticados puedan acceder a los endpoints durante ese tiempo.

```javascript
// Login endpoint
app.post("/login", (req, res) => {
    const user = req.body.user;
    if (!user) {
        return res.status(404).json({ message: "Body Empty" });
    }
    // Generate JWT access token
    let accessToken = jwt.sign({
        data: user
    }, 'access', { expiresIn: 60 * 60 });

    // Store access token in session
    req.session.authorization = {
        accessToken
    }
    return res.status(200).send("User successfully logged in");
});
```

Observe la implementación del middleware de autenticación. Todos los endpoints que comienzan con /user pasarán por este middleware. Recuperará los detalles de autorización de la sesión y los verificará. Si el token es validado, el usuario está autenticado y el control se pasa al siguiente manejador de endpoint. Si el token es inválido, el usuario no está autenticado y se devuelve un mensaje de error.

```javascript
// Middleware for user authentication
app.use("/user", (req, res, next) => {
    // Check if user is authenticated
    if (req.session.authorization) {
        let token = req.session.authorization['accessToken']; // Access Token
        
        // Verify JWT token for user authentication
        jwt.verify(token, "access", (err, user) => {
            if (!err) {
                req.user = user; // Set authenticated user data on the request object
                next(); // Proceed to the next middleware
            } else {
                return res.status(403).json({ message: "User not authenticated" }); // Return error if token verification fails
            }
        });
        // Return error if no access token is found in the session
    } else {
        return res.status(403).json({ message: "User not logged in" });
    }
});
```

---

## Pruebas de endpoints con **POSTMAN**

Has probado los endpoints de la API con cURL. Una forma más fácil y amigable de probar estos endpoints es con la herramienta de interfaz gráfica (GUI), Postman.

0. Ve a Postman. Regístrate para obtener una nueva cuenta de Postman si aún no tienes una. Inicia sesión en tu cuenta.

>https://getpostman.com/

1. Después de iniciar sesión en Postman, haz clic en Nueva Solicitud como se muestra a continuación:
afterlogin

>Nota: Si el servidor está corriendo en el laboratorio theia, por favor detén el servidor presionando CTRL + C. Ahora inicia el servidor ejecutando el siguiente comando que escuchará en el puerto 5000.

```
npm run start_auth
```

Hasta ahora hemos accedido a todos los endpoints sin autenticación, pero ahora utilizaremos autenticación para acceder a los endpoints.

Copia la URL de la aplicación de lanzamiento y añade el inicio de sesión como un endpoint para agregar los detalles del usuario en la SOLICITUD POST que se verá como a continuación:
```
https://<sn-lab-username>-5000.theiadocker-2-labs-prod-theiak8s-4-tor01.proxy.cognitiveclass.ai/login
```

Los detalles del usuario deben estar en el siguiente formato:
```
{
    "user":{
        "name":"abc",
        "id":1
    }
}
```

Ahora comencemos la prueba enviando una solicitud HTTP GET.

> NECESARIO: prueba con otro tutorial de POSTMAN porque este estñá obsoleto,...