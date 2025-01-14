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
    </style>
</head>
<body>
    <header>
        <h1>Welcome to Our Bookshop</h1>
        <nav>
            <ul>
                <li><a onclick="navigateTo('home')">Home</a></li>
                <li><a onclick="navigateTo('books')">Books</a></li>
                <li><a onclick="navigateTo('cart')">Cart</a></li>
                <li><a onclick="navigateTo('contact')">Contact</a></li>
            </ul>
        </nav>
    </header>

    <section id="home">
        <h2>Home</h2>
        <p>Welcome to our online bookshop! Explore a wide range of books and find your next favorite read.</p>
    </section>

    <section id="books" class="hidden">
        <h2>Books</h2>
        <div class="book-container"></div>
    </section>

    <section id="cart" class="hidden">
        <h2>Cart</h2>
        <ul id="cart-items"></ul>
        <p>Delivery Fee: $3.00</p>
        <p>Total: $<span id="cart-total">0.00</span></p>

        <form id="checkout-form">
            <label for="customer-name">Name:</label>
            <input type="text" id="customer-name" placeholder="Enter your name" required>
            <label for="address">Address:</label>
            <input type="text" id="address" placeholder="Enter your address" required>
            <label for="city">City:</label>
            <input type="text" id="city" placeholder="Enter your city" required>
            <label for="phone">Phone Number:</label>
            <input type="text" id="phone" placeholder="Enter your phone number" required>
            <button type="submit">Submit Purchase</button>
        </form>
    </section>

    <section id="contact" class="hidden">
        <h2>Contact Us</h2>
        <p>If you have any questions, feel free to contact us at <a href="mailto:support@bookshop.com">support@bookshop.com</a>.</p>
    </section>

    <footer>
        <p>© 2025 Bookshop. All Rights Reserved.</p>
    </footer>

    <script src="https://cdn.emailjs.com/dist/email.min.js"></script>
    <script>
        emailjs.init("your_user_id"); // Replace with your EmailJS User ID

        // Navigation
        function navigateTo(sectionId) {
            document.querySelectorAll("section").forEach((section) => section.classList.add("hidden"));
            document.getElementById(sectionId).classList.remove("hidden");
        }

        // Load Books
        const books = Array.from({ length: 100 }, (_, i) => ({
            id: i + 1,
            title: `Book Title ${i + 1}`,
            author: `Author ${i + 1}`,
            price: (Math.random() * 20 + 5).toFixed(2),
        }));

        const bookContainer = document.querySelector(".book-container");
        books.forEach((book) => {
            const bookDiv = document.createElement("div");
            bookDiv.className = "book";
            bookDiv.innerHTML = `
                <h3>${book.title}</h3>
                <p>Author: ${book.author}</p>
                <p>Price: $${book.price}</p>
                <button onclick="addToCart(${book.id}, '${book.title}', ${book.price})">Add to Cart</button>
            `;
            bookContainer.appendChild(bookDiv);
        });

        // Cart Functionality
        const cart = [];
        const cartItemsList = document.getElementById("cart-items");
        const cartTotal = document.getElementById("cart-total");

        function addToCart(id, name, price) {
            const existing = cart.find((item) => item.id === id);
            if (existing) {
                existing.quantity++;
            } else {
                cart.push({ id, name, price, quantity: 1 });
            }
            updateCart();
        }

        function updateCart() {
            cartItemsList.innerHTML = "";
            let total = 0;

            cart.forEach((item) => {
                total += item.price * item.quantity;
                const li = document.createElement("li");
                li.textContent = `${item.name} - $${item.price} x ${item.quantity}`;
                cartItemsList.appendChild(li);
            });

            total += 3; // Delivery Fee
            cartTotal.textContent = total.toFixed(2);
        }

        // Handle Checkout Form
        document.getElementById("checkout-form").addEventListener("submit", (e) => {
            e.preventDefault();

            const name = document.getElementById("customer-name").value;
            const address = document.getElementById("address").value;
            const city = document.getElementById("city").value;
            const phone = document.getElementById("phone").value;

            if (cart.length === 0) {
                alert("Your cart is empty. Please add items to the cart before purchasing.");
                return;
            }

            const orderDetails = cart.map((item) => `${item.name} x${item.quantity}`).join("\n");
            const totalAmount = cart.reduce((sum, item) => sum + item.price * item.quantity, 0) + 3;

            const message = `
                New Order from ${name}:

                Order Details:
                ${orderDetails}

                Address: ${address}, ${city}
                Phone: ${phone}

                Total: $${totalAmount.toFixed(2)}
            `;

            // Send email via EmailJS
            emailjs.send("your_service_id", "your_template_id", {
                to_email: "your_email@example.com", // Replace with your email
                subject: "New Order",
                message: message,
            })
            .then(() => {
                alert("Your order has been placed successfully!");
                cart.length = 0;
                updateCart();
                e.target.reset();
            })
            .catch(() => alert("Failed to send order. Please try again."));
        });
    </script>
</body>
</html>
