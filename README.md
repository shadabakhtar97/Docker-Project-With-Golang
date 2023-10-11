# Docker-Project-With-Golang
3-Tier Project in Golang along with Docker

Creating a full three-tier application in Golang along with Docker deployment involves multiple components, such as the frontend, backend, and database. It's a complex task, but I can provide you with a basic example of a simple three-tier application to get you started. This example uses Golang for the backend and SQLite as the database. It includes a simple REST API for CRUD operations.

Here's a basic outline of the code:

1. **Backend (REST API):** Create a Golang REST API using the `net/http` package.

2. **Database (SQLite):** Use SQLite as a lightweight database.

3. **Frontend (Static HTML):** Create a simple HTML page for the frontend. In a more complex application, you would typically use a JavaScript framework for the frontend.

4. **Docker:** Use Docker to containerize the application.

Below is a basic example of the code. You can extend and improve this as needed for your project.

**main.go (Backend):**

```go
package main

import (
	"database/sql"
	"fmt"
	"log"
	"net/http"

	_ "github.com/mattn/go-sqlite3"
	"github.com/gorilla/mux"
)

var db *sql.DB

func main() {
	db, err := sql.Open("sqlite3", "./app.db")
	if err != nil {
		log.Fatal(err)
	}
	defer db.Close()

	router := mux.NewRouter()
	router.HandleFunc("/items", getItems).Methods("GET")
	router.HandleFunc("/items/{id}", getItem).Methods("GET")
	router.HandleFunc("/items", createItem).Methods("POST")
	router.HandleFunc("/items/{id}", updateItem).Methods("PUT")
	router.HandleFunc("/items/{id}", deleteItem).Methods("DELETE")

	log.Fatal(http.ListenAndServe(":8080", router))
}

func getItems(w http.ResponseWriter, r *http.Request) {
	// Fetch and return all items from the database
	// Implement SQL queries here
}

func getItem(w http.ResponseWriter, r *http.Request) {
	// Fetch and return a single item based on ID from the database
	// Implement SQL queries here
}

func createItem(w http.ResponseWriter, r *http.Request) {
	// Create a new item and add it to the database
	// Implement SQL insert queries here
}

func updateItem(w http.ResponseWriter, r *http.Request) {
	// Update an existing item in the database
	// Implement SQL update queries here
}

func deleteItem(w http.ResponseWriter, r *http.Request) {
	// Delete an item from the database
	// Implement SQL delete queries here
}
```

**Dockerfile:**

```Dockerfile
FROM golang:1.17 AS build

WORKDIR /app
COPY . .

RUN go mod download
RUN go build -o app

FROM alpine:latest

RUN apk --no-cache add ca-certificates

WORKDIR /root/

COPY --from=build /app/app .

EXPOSE 8080

CMD ["./app"]
```

**HTML Frontend:**

Create an `index.html` file for your frontend.

**Docker Compose (docker-compose.yml):**

```yaml
version: '3'
services:
  app:
    build: .
    ports:
      - "8080:8080"
    volumes:
      - .:/app
    depends_on:
      - db
  db:
    image: sqlite
    volumes:
      - ./app.db:/app.db
```

To run the application with Docker Compose:

1. Create the `docker-compose.yml` file and the HTML frontend file.
2. Build and run the application with the following commands:
   ```
   docker-compose build
   docker-compose up
   ```
3. Access the application in your browser at http://localhost:8080.

Please note that this is a basic example, and for a more complex production application, you would likely need to consider additional factors, such as security, authentication, and deployment strategies.
