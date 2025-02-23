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
            background-color: #FFEBEB; /* Light pink background */
            color: #000; /* Black text */
        }
        header {
            background-color: #FF6F61; /* Darker pink for header */
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
        .admin-page input {
            margin-bottom: 10px;
            padding: 5px;
        }
        .admin-page button {
            padding: 10px;
            background-color: #FF6F61;
            color: white;
            border: none;
            cursor: pointer;
        }
        .admin-page form {
            margin-bottom: 20px;
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
                <li><a onclick="navigateTo('contact')">Contact</a></li>
                <li><a onclick="navigateTo('admin')">Admin</a></li>
            </ul>
        </nav>
    </header>

    <section id="home">
        <h2>Home</h2>
        <p id="home-text-content">Welcome to our online bookshop! Explore a wide range of books and find your next favorite read.</p>
    </section>

    <section id="books" class="hidden">
        <h2>Books</h2>
        <div class="book-container" id="book-container"></div>
    </section>

    <section id="contact" class="hidden">
        <h2>Contact Us</h2>
        <p id="contact-text-content">If you have any questions, feel free to contact us at <a href="mailto:support@bookshop.com">support@bookshop.com</a>.</p>
    </section>

    <section id="admin" class="hidden admin-page">
        <h2>Admin Login</h2>
        <form id="admin-login-form">
            <input type="text" id="admin-username" placeholder="Username" required>
            <input type="password" id="admin-password" placeholder="Password" required>
            <button type="submit">Login</button>
        </form>

        <div id="admin-dashboard" class="hidden">
            <h2>Admin Dashboard</h2>
            <h3>Edit Books</h3>
            <form id="edit-book-form">
                <input type="number" id="book-id" placeholder="Book ID" required>
                <input type="text" id="book-title" placeholder="Book Title" required>
                <input type="text" id="book-author" placeholder="Author" required>
                <input type="number" id="book-price" placeholder="Price" required>
                <input type="file" id="book-image" accept="image/*">
                <button type="submit">Update Book</button>
            </form>

            <h3>Edit Home Page</h3>
            <form id="edit-home-form">
                <textarea id="home-text" placeholder="Edit Home Page Text" rows="4" cols="50"></textarea>
                <button type="submit">Save Home Page</button>
            </form>

            <h3>Edit Contact Page</h3>
            <form id="edit-contact-form">
                <textarea id="contact-text" placeholder="Edit Contact Page Text" rows="4" cols="50"></textarea>
                <button type="submit">Save Contact Page</button>
            </form>

            <h3>Edit Delivery Page</h3>
            <form id="edit-delivery-form">
                <textarea id="delivery-text" placeholder="Edit Delivery Page Text" rows="4" cols="50"></textarea>
                <button type="submit">Save Delivery Page</button>
            </form>
        </div>
    </section>

    <footer>
        <p>© 2025 Bookshop. All Rights Reserved.</p>
    </footer>

    <script src="https://cdn.emailjs.com/dist/email.min.js"></script>
    <script>
        emailjs.init("your_user_id"); // Replace with your EmailJS User ID

        // Initial books data
        let books = JSON.parse(localStorage.getItem('books')) || [
            { id: 1, title: "Book 1", author: "Author 1", price: 10.00 },
            { id: 2, title: "Book 2", author: "Author 2", price: 12.50 },
        ];

        // Update localStorage with current book data
        function saveBooksToLocalStorage() {
            localStorage.setItem('books', JSON.stringify(books));
        }

        // Display books
        function loadBooks() {
            const bookContainer = document.getElementById("book-container");
            bookContainer.innerHTML = '';
            books.forEach((book) => {
                const bookDiv = document.createElement("div");
                bookDiv.className = "book";
                bookDiv.innerHTML = `
                    <h3>${book.title}</h3>
                    <p>Author: ${book.author}</p>
                    <p>Price: $${book.price}</p>
                `;
                bookContainer.appendChild(bookDiv);
            });
        }

        // Navigation
        function navigateTo(sectionId) {
            document.querySelectorAll("section").forEach((section) => section.classList.add("hidden"));
            document.getElementById(sectionId).classList.remove("hidden");
        }

        // Admin Login
        document.getElementById("admin-login-form").addEventListener("submit", (e) => {
            e.preventDefault();
            const username = document.getElementById("admin-username").value;
            const password = document.getElementById("admin-password").value;

            // Simple hardcoded login check (replace with actual authentication in production)
            if (username === "admin" && password === "admin123") {
                document.getElementById("admin-dashboard").classList.remove("hidden");
                document.getElementById("admin-login-form").classList.add("hidden");
            } else {
                alert("Invalid username or password");
            }
        });

        // Editing Books
        document.getElementById("edit-book-form").addEventListener("submit", (e) => {
            e.preventDefault();
            const bookId = parseInt(document.getElementById("book-id").value);
            const bookTitle = document.getElementById("book-title").value;
            const bookAuthor = document.getElementById("book-author").value;
            const bookPrice = parseFloat(document.getElementById("book-price").value);
            const bookImage = document.getElementById("book-image").files[0];

            const existingBook = books.find(book => book.id === bookId);
            if (existingBook) {
                existingBook.title = bookTitle;
                existingBook.author = bookAuthor;
                existingBook.price = bookPrice;
                // Update image if needed
                if (bookImage) {
                    // Handling image uploading should be done on the server-side
                }
                saveBooksToLocalStorage();
                alert(`Book ${bookTitle} updated successfully!`);
                loadBooks(); // Re-render updated book list
            } else {
                alert("Book not found.");
            }
        });

        // Editing Pages
        document.getElementById("edit-home-form").addEventListener("submit", (e) => {
            e.preventDefault();
            const homeText = document.getElementById("home-text").value;
            document.getElementById("home-text-content").textContent = homeText;
            localStorage.setItem("home-text", homeText);
            alert("Home page updated!");
        });

        document.getElementById("edit-contact-form").addEventListener("submit", (e) => {
            e.preventDefault();
            const contactText = document.getElementById("contact-text").value;
            document.getElementById("contact-text-content").textContent = contactText;
            localStorage.setItem("contact-text", contactText);
            alert("Contact page updated!");
        });

        document.getElementById("edit-delivery-form").addEventListener("submit", (e) => {
            e.preventDefault();
            const deliveryText = document.getElementById("delivery-text").value;
            localStorage.setItem("delivery-text", deliveryText);
            alert("Delivery page updated!");
        });

        // Load data on page load
        window.onload = function() {
            const savedHomeText = localStorage.getItem("home-text");
            if (savedHomeText) document.getElementById("home-text-content").textContent = savedHomeText;

            const savedContactText = localStorage.getItem("contact-text");
            if (savedContactText) document.getElementById("contact-text-content").textContent = savedContactText;

            const savedDeliveryText = localStorage.getItem("delivery-text");
            if (savedDeliveryText) document.getElementById("delivery-text").value = savedDeliveryText;

            loadBooks();
        };
    </script>
</body>
</html>
