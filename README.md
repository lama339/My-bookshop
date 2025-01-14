<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bookshop</title>
    <style>
        /* General Styles */
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f9f9f9;
        }

        header {
            background-color: #4CAF50;
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
            gap: 15px;
        }

        nav ul li {
            display: inline;
        }

        nav ul li a {
            color: white;
            text-decoration: none;
            font-size: 18px;
        }

        section {
            padding: 20px;
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
            background-color: #4CAF50;
            color: white;
            text-align: center;
            padding: 10px 0;
            position: fixed;
            bottom: 0;
            width: 100%;
        }
    </style>
    <script src="https://cdn.emailjs.com/dist/email.min.js"></script>
</head>
<body>
    <!-- Header -->
    <header>
        <h1>Welcome to Our Bookshop</h1>
        <nav>
            <ul>
                <li><a href="#home">Home</a></li>
                <li><a href="#books">Books</a></li>
                <li><a href="#cart">Cart</a></li>
                <li><a href="#contact">Contact</a></li>
            </ul>
        </nav>
    </header>

    <!-- Home Section -->
    <section id="home">
        <h2>Home</h2>
        <p>Welcome to our online bookshop! Explore a wide range of books and find your next favorite read.</p>
    </section>

    <!-- Books Section -->
    <section id="books">
        <h2>Books</h2>
        <div class="book-container">
            <!-- Books will be dynamically added here by JavaScript -->
        </div>
    </section>

    <!-- Cart Section -->
    <section id="cart">
        <h2>Shopping Cart</h2>
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

    <!-- Contact Section -->
    <section id="contact">
        <h2>Contact Us</h2>
        <p>If you have any questions, feel free to contact us at support@bookshop.com.</p>
    </section>

    <!-- Footer -->
    <footer>
        <p>© 2025 Bookshop. All Rights Reserved.</p>
    </footer>

    <script>
        // Initialize EmailJS (replace with your actual User ID)
        emailjs.init("your_user_id"); // Replace with your EmailJS User ID

        document.addEventListener("DOMContentLoaded", () => {
            const books = [];
            for (let i = 1; i <= 100; i++) {
                books.push({
                    id: i,
                    title: `Book Title ${i}`,
                    author: `Author ${i}`,
                    price: (Math.random() * 20 + 5).toFixed(2), // Random price between $5 and $25
                });
            }

            const bookContainer = document.querySelector(".book-container");
            const cart = [];
            const cartItemsList = document.getElementById("cart-items");
            const cartTotal = document.getElementById("cart-total");

            // Render books
            books.forEach((book) => {
                const bookDiv = document.createElement("div");
                bookDiv.className = "book";
                bookDiv.innerHTML = `
                    <h3>${book.title}</h3>
                    <p>Author: ${book.author}</p>
                    <p>Price: $${book.price}</p>
                    <button data-id="${book.id}" data-name="${book.title}" data-price="${book.price}">Add to Cart</button>
                `;
                bookContainer.appendChild(bookDiv);
            });

            // Add to Cart
            document.querySelectorAll(".book button").forEach((button) => {
                button.addEventListener("click", (e) => {
                    const id = e.target.dataset.id;
                    const name = e.target.dataset.name;
                    const price = parseFloat(e.target.dataset.price);

                    const existingBook = cart.find((item) => item.id === id);
                    if (existingBook) {
                        existingBook.quantity++;
                    } else {
                        cart.push({ id, name, price, quantity: 1 });
                    }

                    updateCart();
                });
            });

            // Update Cart
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
            const checkoutForm = document.getElementById("checkout-form");
            checkoutForm.addEventListener("submit", (e) => {
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
                    checkoutForm.reset();
                })
                .catch(() => alert("Failed to send order. Please try again."));
            });
        });
    </script>
</body>
</html>
