# Difference Between ENTRYPOINT and CMD in Docker

Both `ENTRYPOINT` and `CMD` define what command a container runs, but they serve different purposes.

## 1. Overview

| Feature        | `ENTRYPOINT` | `CMD` |
|---------------|-------------|-------|
| **Purpose**   | Defines a fixed command that always runs | Provides a default command that can be overridden |
| **Override**  | Arguments passed to `docker run` are used as parameters to `ENTRYPOINT` | Arguments passed to `docker run` completely override `CMD` |
| **Flexibility** | Less flexible, enforces a specific behavior | More flexible, allows command modification |
| **Usage** | Used when a container should always run a specific executable | Used to provide default behavior that can be changed |

## 2. Examples

### Example 1: Using `CMD`
```dockerfile
FROM alpine
CMD ["echo", "Hello, World!"]
```
Running the container:
```sh
docker build -t cmd-example .
docker run cmd-example
```
**Output:**
```
Hello, World!
```

If you override the command:
```sh
docker run cmd-example echo "Different Message"
```
**Output:**
```
Different Message
```

### Example 2: Using `ENTRYPOINT`
```dockerfile
FROM alpine
ENTRYPOINT ["echo"]
```
Running the container:
```sh
docker build -t entrypoint-example .
docker run entrypoint-example "Hello, World!"
```
**Output:**
```
Hello, World!
```

If you override the command:
```sh
docker run entrypoint-example "Different Message"
```
**Output:**
```
Different Message
```

### Example 3: Combining `ENTRYPOINT` and `CMD`
```dockerfile
FROM alpine
ENTRYPOINT ["echo"]
CMD ["Default message"]
```
Running the container:
```sh
docker build -t combined-example .
docker run combined-example
```
**Output:**
```
Default message
```

If overridden:
```sh
docker run combined-example "Overridden message"
```
**Output:**
```
Overridden message
```

## 3. When to Use What?

- Use `ENTRYPOINT` when you want the container to always run a specific command.
- Use `CMD` when you want to provide a default command but allow users to override it.
- Combine both when you want a fixed executable with default arguments that can be overridden.

## 4. Best Practices

- **Use `ENTRYPOINT` for mandatory commands**, such as starting a web server.
- **Use `CMD` for default arguments**, allowing users to customize behavior.
- **Use exec form (`[]` brackets) instead of shell form (`""`)** to avoid issues with signal handling.
- **Avoid overriding both `ENTRYPOINT` and `CMD`** unless necessary.
- **Provide clear documentation** in your `Dockerfile` so users know what can be overridden.

This helps ensure consistent and predictable behavior for your Docker containers.
