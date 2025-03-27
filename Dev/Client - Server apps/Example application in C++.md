---
tags:
  - CPP
  - SERVER
---
## Server.cpp

```cpp
#include <boost/asio.hpp>
#include <iostream>

using boost::asio::ip::tcp;

int main() {
    try {
        // Контекст ввода-вывода (обновленное название)
        boost::asio::io_context io;

        // Создание TCP-акцептора на порту 8080
        tcp::acceptor acceptor(io, tcp::endpoint(tcp::v4(), 8080));
        std::cout << "Server started. Waiting for connection..." << std::endl;

        while (true) {
            tcp::socket socket(io);

            acceptor.accept(socket);
            std::cout << "Client connected: " 
                      << socket.remote_endpoint().address().to_string() 
                      << std::endl;

            boost::system::error_code ec;
            boost::asio::streambuf buffer;
            boost::asio::read_until(socket, buffer, '\n', ec);
            
            if (!ec) {
                std::istream is(&buffer);
                std::string message;
                std::getline(is, message);
                std::cout << "Received: " << message << std::endl;

                std::string response = "The server received: " + message + "\n";
                boost::asio::write(socket, boost::asio::buffer(response), ec);
            } else {
                std::cerr << "Error: " << ec.message() << std::endl;
            }
        }
    } catch (const std::exception& e) {
        std::cerr << "Server error: " << e.what() << std::endl;
    }
    return 0;
}

```

## Client.cpp

```cpp
#include <boost/asio.hpp>
#include <iostream>

using boost::asio::ip::tcp;

int main() {
    try {
        boost::asio::io_context io;

        tcp::socket socket(io);
        socket.connect(tcp::endpoint(
            boost::asio::ip::make_address("127.0.0.1"), 
            8080
        ));

        std::string message;
        std::cout << "Enter your message: ";
        std::getline(std::cin, message);
        message += "\n";

        boost::asio::write(socket, boost::asio::buffer(message));

        boost::asio::streambuf response_buffer;
        boost::asio::read_until(socket, response_buffer, '\n');
        std::istream is(&response_buffer);
        std::string response;
        std::getline(is, response);

        std::cout << "Server response: " << response << std::endl;
    } catch (const std::exception& e) {
        std::cerr << "Client error: " << e.what() << std::endl;
    }
    return 0;
}

```