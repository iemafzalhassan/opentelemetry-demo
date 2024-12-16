# OpenTelemetry Demo Guide for Beginners

## What is OpenTelemetry?

OpenTelemetry is a tool that helps developers see what's happening inside their applications. Imagine it as a magnifying glass that lets you look at the inner workings of a program to understand how it behaves and performs.

## What is this Project About?

This project is a demo (a demonstration) of how OpenTelemetry works. It uses a pretend online store to show how you can track and monitor different parts of an application. This helps developers find and fix problems more easily.

## Telemetry Components

These components help monitor and visualize the application's performance:

- **Jaeger**: A tool for tracing requests as they move through the system.

- **Grafana**: Displays graphs and charts of the collected data.

- **OpenTelemetry Collector**: Gathers data from all services and sends it to other tools for analysis.

- **Prometheus**: Collects and stores metrics data.

- **OpenSearch**: Stores logs and allows for searching through them.

## Getting Started with Docker

# Docker Compose File Explanation

This file is a blueprint for setting up and running a collection of services that make up the OpenTelemetry demo application. Each service is like a mini-program that does a specific job. Let's break down what each part does:

## General Settings

- **Logging**: This section sets up how logs (records of what the services are doing) are stored. Logs are saved in files with a maximum size of 5 megabytes, and only two files are kept at a time.

- **Networks**: All services are connected through a network named `opentelemetry-demo`, which allows them to communicate with each other.

## Core Demo Services

These are the main parts of the demo application:

1. **Accounting Service**: Keeps track of financial transactions. It uses a database called Kafka to store data and sends information to a central collector for analysis.

2. **AdService**: Manages advertisements. It listens on a specific port (like a door for data to come in) and sends logs and metrics to the collector.

3. **Cart Service**: Handles shopping cart operations. It depends on another service called Valkey to work properly.

4. **Checkout Service**: Manages the checkout process when you buy something. It talks to several other services like Cart, Currency, and Payment to complete a purchase.

5. **Currency Service**: Converts money from one currency to another. It sends data to the collector for monitoring.

6. **Email Service**: Sends emails, such as order confirmations. It also reports its activity to the collector.

7. **Fraud Detection Service**: Looks for suspicious activity to prevent fraud. It uses Kafka and sends data to the collector.

8. **Frontend**: The user interface that people interact with. It connects to many other services to display information and perform actions.

9. **Frontend Proxy (Envoy)**: Acts as a middleman to manage traffic between the frontend and other services.

10. **Image Provider**: Supplies images for the frontend to display.

11. **Load Generator**: Simulates user activity to test how the application performs under pressure.

12. **Payment Service**: Processes payments. It communicates with the collector and other services.

13. **Product Catalog Service**: Manages the list of products available in the store.

14. **Quote Service**: Provides price quotes for shipping.

15. **Recommendation Service**: Suggests products to users based on their activity.

16. **Shipping Service**: Calculates shipping costs and manages delivery information.

## Dependent Services

These services support the core services:

- **Flagd**: Manages feature flags, which are like switches to turn features on or off.

- **Flagd UI**: A user interface to configure feature flags.

- **Kafka**: A messaging system used by several services to exchange data.

- **Valkey**: Supports the Cart service by managing data storage.



### Prerequisites

Before you start, make sure you have:

- Docker installed on your computer.
- Docker Compose version 2.0.0.
- At least 6 GB of RAM (memory) available.

### Steps to Run the Demo

1. **Clone the Repository**: This means copying the project files to your computer. Open a terminal and type:

   ```bash
   git clone https://github.com/iemafzalhassan/opentelemetry-demo.git
   ```

2. **Navigate to the Project Folder**: Change into the directory where the files are located:

   ```bash
   cd opentelemetry-demo/
   ```

3. **Start the Demo**: Use Docker Compose to start the demo. This command will set up everything for you:

   ```bash
   docker compose up --force-recreate --remove-orphans --detach
   ```

   - **`--force-recreate`**: Rebuilds the containers from scratch.
   - **`--remove-orphans`**: Removes any containers that are not defined in the current Docker Compose file.
   - **`--detach`**: Runs the containers in the background.

4. **Access the Demo**: Once everything is running, you can visit the following in your web browser:

   - **Web Store**: [http://localhost:8080/](http://localhost:8080/)
   - **Grafana**: [http://localhost:8080/grafana/](http://localhost:8080/grafana/)
   - **Jaeger**: [http://localhost:8080/jaeger/ui/](http://localhost:8080/jaeger/ui/)
   - **Load Generator UI**: [http://localhost:8080/loadgen/](http://localhost:8080/loadgen/)
   - **Tracetest UI**: [http://localhost:11633/](http://localhost:11633/)
   - **Flagd configurator UI**: [http://localhost:8080/flagd-configurator/](http://localhost:8080/feature/)


### Changing the Port Number

By default, the demo runs on port 8080. If you need to change it, you can set a new port number like this:

```bash
ENVOY_PORT=8081 docker compose up --force-recreate --remove-orphans --detach
```

### Adding Your Own Backend

If you want to connect the demo to another tool you already use, you can do that by editing a configuration file:

1. Open `src/otelcollector/otelcol-config-extras.yml` in a text editor.
2. Add your backend details under the `exporters` section.

### Running Tests

To test the application, you can run:

```bash
docker compose -f docker-compose-tests.yml run traceBasedTests
```