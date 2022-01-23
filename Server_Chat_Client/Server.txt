import java.net.ServerSocket;
import java.net.Socket;
import java.io.*;

// This is the ChatServer Class that handles the ChatServer part of our Chat System

public class ChatServer {

    //main function
    public static void main(String[] args) throws IOException {
        ServerSocket serverSocket = new ServerSocket( 23657);
        ChatServer server = new ChatServer(serverSocket);
        server.chatStartServer();
    }

    //creating a server Socket which is responsible for incoming clients
    private ServerSocket serverSocket;

    //constructor that sets up the server socket
    public ChatServer (ServerSocket serverSocket) {
        this.serverSocket = serverSocket;
    }

    //this function keeps the server running
    public void chatStartServer() {
        try{
            //running server socket indefinitely
            while (!serverSocket.isClosed()) {
                // waits for a client to connect
                Socket socket = serverSocket.accept();
                System.out.println("New client Connectd!");
                ChatClientHandler clientHandler = new ChatClientHandler(socket);

                Thread thread = new Thread(clientHandler);
                thread.start();
            }
        } catch (IOException e) {

        }
    }

    // in-case an error occurs we shut down our server socket
    public void shutdown() {
        try {
            if (serverSocket != null) {
                serverSocket.close();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

}
