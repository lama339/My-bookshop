<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bookshop</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #FFEBEB;
            color: #000;
        }
        header {
            background-color: #FF6F61;
            color: white;
            padding: 20px;
            text-align: center;
        }
        nav ul {
            list-style-type: none;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            gap: 20px;
        }
        nav ul li {
            display: inline;
        }
        nav ul li a {
            color: white;
            text-decoration: none;
            font-size: 18px;
            cursor: pointer;
        }
        section {
            padding: 20px;
        }
        .hidden {
            display: none;
        }
        h2 {
            color: #333;
        }
        .book-container {
            display: flex;
            flex-wrap: wrap;
            gap: 20px;
        }
        .book {
            border: 1px solid #ccc;
            padding: 10px;
            width: 200px;
            background-color: white;
        }
        footer {
            background-color: #FF6F61;
            color: white;
            text-align: center;
            padding: 10px 0;
            position: fixed;
            bottom: 0;
            width: 100%;
        }
        .admin-panel, .delivery-panel, .home-panel, .contact-panel {
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <header>
        <h1>Welcome to Our Bookshop</h1>
        <nav>
            <ul>
                <li><a onclick="navigateTo('home')">Home</a></li>
                <li><a onclick="navigateTo('books')">Books</a></li>
                <li><a onclick="navigateTo('delivery')">Delivery</a></li>
                <li><a onclick="navigateTo('contact')">Contact</a></li>
                <li><a onclick="navigateTo('admin')">Admin</a></li>
            </ul>
        </nav>
    </header>

    <section id="home">
        <h2>Home</h2>
        <p id="home-content">Welcome to our online bookshop! Explore a wide range of books and find your next favorite read.</p>
    </section>

    <section id="books" class="hidden">
        <h2>Books</h2>
        <div class="book-container" id="book-container"></div>
    </section>

    <section id="delivery" class="hidden">
        <h2>Delivery</h2>
        <p>Delivery Fee: $3.00</p>
        <p>Standard delivery time: 3-5 business days.</p>
    </section>

    <section id="contact" class="hidden">
        <h2>Contact Us</h2>
        <p id="contact-content">If you have any questions, feel free to contact us at <a href="mailto:support@bookshop.com">support@bookshop.com</a>.</p>
    </section>

    <section id="admin" class="hidden">
        <h2>Admin Panel</h2>
        <form id="admin-form">
            <label for="username">Username:</label>
            <input type="text" id="username" required>
            <label for="password">Password:</label>
            <input type="password" id="password" required>
            <button type="submit">Login</button>
        </form>

        <div class="admin-panel hidden">
            <h3>Edit Book Information</h3>
            <input type="text" id="edit-book-title" placeholder="Book Title">
            <input type="text" id="edit-book-price" placeholder="Book Price">
            <input type="file" id="edit-book-image" placeholder="Book Image">
            <button onclick="saveBookDetails()">Save</button>

            <h3>Edit Home Page Content</h3>
            <textarea id="home-textarea"></textarea>
            <button onclick="saveHomeContent()">Save Home Content</button>

            <h3>Edit Contact Information</h3>
            <textarea id="contact-textarea"></textarea>
            <button onclick="saveContactContent()">Save Contact Content</button>
        </div>
    </section>

    <footer>
        <p>© 2025 Bookshop. All Rights Reserved.</p>
    </footer>

    <script>
        // Navigation
        function navigateTo(sectionId) {
            document.querySelectorAll("section").forEach((section) => section.classList.add("hidden"));
            document.getElementById(sectionId).classList.remove("hidden");
        }

        // Load Books
        let books = JSON.parse(localStorage.getItem('books')) || [];
        const bookContainer = document.getElementById("book-container");
        function renderBooks() {
            bookContainer.innerHTML = '';
            books.forEach((book, index) => {
                const bookDiv = document.createElement("div");
                bookDiv.className = "book";
                bookDiv.innerHTML = `
                    <h3>${book.title}</h3>
                    <p>Price: $${book.price}</p>
                    <img src="${book.image}" alt="${book.title}" style="width: 100%; height: auto;">
                    <button onclick="editBook(${index})">Edit</button>
                `;
                bookContainer.appendChild(bookDiv);
            });
        }
        renderBooks();

        // Admin Panel
        document.getElementById("admin-form").addEventListener("submit", (e) => {
            e.preventDefault();
            const username = document.getElementById("username").value;
            const password = document.getElementById("password").value;

            if (username === "admin" && password === "password123") {
                document.querySelector(".admin-panel").classList.remove("hidden");
                navigateTo('admin');
            } else {
                alert("Invalid credentials");
            }
        });

        function saveBookDetails() {
            const title = document.getElementById("edit-book-title").value;
            const price = document.getElementById("edit-book-price").value;
            const image = document.getElementById("edit-book-image").files[0];

            if (image) {
                const reader = new FileReader();
                reader.onload = function(e) {
                    const newBook = { title, price, image: e.target.result };
                    books.push(newBook);
                    localStorage.setItem('books', JSON.stringify(books));
                    renderBooks();
                };
                reader.readAsDataURL(image);
            }
        }

        function editBook(index) {
            const book = books[index];
            document.getElementById("edit-book-title").value = book.title;
            document.getElementById("edit-book-price").value = book.price;
        }

        // Save Home Content
        function saveHomeContent() {
            const content = document.getElementById("home-textarea").value;
            document.getElementById("home-content").innerText = content;
            localStorage.setItem('homeContent', content);
        }

        // Save Contact Content
        function saveContactContent() {
            const content = document.getElementById("contact-textarea").value;
            document.getElementById("contact-content").innerText = content;
            localStorage.setItem('contactContent', content);
        }

        // Initialize saved content
        document.getElementById("home-content").innerText = localStorage.getItem('homeContent') || "Welcome to our online bookshop!";
        document.getElementById("contact-content").innerText = localStorage.getItem('contactContent') || "If you have any questions, feel free to contact us.";
    </script>
</body>
</html>
