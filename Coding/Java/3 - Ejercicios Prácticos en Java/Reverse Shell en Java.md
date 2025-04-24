# Cliente:
```java
import java.io.*;
import java.net.*;

public class codigo {

    public static void main(String[] args) throws IOException {
        String serverAddress = "192.168.0.20"; // Cambia a la dirección del servidor
        int port = 8080;
        try (Socket socket = new Socket(serverAddress, port);
             BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
             PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
             BufferedReader stdIn = new BufferedReader(new InputStreamReader(System.in))) {

            String serverResponse = in.readLine();
            System.out.println("Server: " + serverResponse);

            String userInput;
            while ((userInput = stdIn.readLine()) != null) {
                out.println(userInput);

                // Leer la salida hasta encontrar el marcador "EndOfOutput" o "EndOfCdCommand"
                while (true) {
                    String line = in.readLine();
                    if (line.equals("EndOfOutput") || line.equals("EndOfCdCommand")) {
                        break;
                    }
                    System.out.println(line);
                }

                if (userInput.equalsIgnoreCase("exit")) {
                    break;
                }
            }
        }
    }
}

```
# Servidor:
```java
import java.io.*;
import java.net.*;

public class Server {

    public static void main(String[] args) {
        try (ServerSocket serverSocket = new ServerSocket(8080)) {
            System.out.println("Server is ready and waiting for connections...");

            while (true) {
                Socket clientSocket = serverSocket.accept();
                new Thread(new ClientHandler(clientSocket)).start();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    private static class ClientHandler implements Runnable {
        private final Socket clientSocket;
        private String currentDirectory;

        public ClientHandler(Socket clientSocket) {
            this.clientSocket = clientSocket;
            this.currentDirectory = System.getProperty("user.home");
        }

        @Override
        public void run() {
            try (
                PrintWriter out = new PrintWriter(clientSocket.getOutputStream(), true);
                BufferedReader in = new BufferedReader(new InputStreamReader(clientSocket.getInputStream()))
            ) {
                out.println("Server is ready");

                String inputLine;
                while ((inputLine = in.readLine()) != null) {
                    if (inputLine.equals("exit")) {
                        break;
                    } else if (inputLine.startsWith("cd")) {
                        handleCdCommand(inputLine, out);
                    } else {
                        runCommand(inputLine, out);
                    }
                }
            } catch (IOException e) {
                e.printStackTrace();
            } finally {
                try {
                    clientSocket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

        private void handleCdCommand(String cdCommand, PrintWriter out) {
            String[] commandParts = cdCommand.split(" ", 2);
            if (commandParts.length == 2) {
                String newDirectory = commandParts[1].trim();
                File newDir = new File(currentDirectory, newDirectory);
                if (newDir.isDirectory()) {
                    currentDirectory = newDir.getAbsolutePath();
                    out.println("Changed directory to: " + currentDirectory);
                    out.println("EndOfCdCommand"); // Marcador especial después de cambiar de directorio
                } else {
                    out.println("Directory not found: " + newDir.getAbsolutePath());
                }
            } else {
                out.println("Invalid cd command format");
            }
        }

        private void runCommand(String command, PrintWriter out) {
            try {
                Process process = Runtime.getRuntime().exec(command, null, new File(currentDirectory));
                try (BufferedReader br = new BufferedReader(new InputStreamReader(process.getInputStream()))) {
                    String line;
                    while ((line = br.readLine()) != null) {
                        out.println(line); // Enviar cada línea de la salida al cliente
                    }
                    out.println("EndOfOutput"); // Marcar el final de la salida después de enviar toda la salida
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```
![[Pasted image 20240307191403.png]]
