Lo primero que tendremos que hacer es configurar maven en java y añadir la dependencia de MySQL [[1 - Configurar Maven en Java]]

--------

Una vez hecho esto, ya podremos ejecutar el siguiente código donde se va a crear una nueva tabla dentro de la base de datos prueba:
```java
package com.example;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class App {
    public static void main(String[] args) {
        // Datos de conexión
        String url = "jdbc:mysql://192.168.0.20/prueba";
        String usuario = "root";
        String contraseña = "123123";

        // Sentencia SQL para crear la tabla
        String crearTablaSQL = "CREATE TABLE ordenadores (" +
                               "id INT AUTO_INCREMENT PRIMARY KEY," +
                               "nombre VARCHAR(50)," +
                               "marca VARCHAR(50)," +
                               "modelo VARCHAR(50)" +
                               ")";
        
        // Intentar establecer la conexión y ejecutar la consulta
        try (Connection conexion = DriverManager.getConnection(url, usuario, contraseña);
             Statement statement = conexion.createStatement()) {
            
            // Ejecutar la sentencia SQL para crear la tabla
            statement.executeUpdate(crearTablaSQL);
            
            System.out.println("Tabla creada exitosamente.");
            
        } catch (SQLException e) {
            // Manejar errores de SQL
            System.err.println("Error al crear la tabla: " + e.getMessage());
        }
    }
}
```
Funciona correctamente:
![[Pasted image 20240319114728.png]]
![[Pasted image 20240319114736.png]]
