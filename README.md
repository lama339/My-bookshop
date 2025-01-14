<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bookshop</title>
    <link rel="stylesheet" href="styles.css">
    <script src="https://cdn.emailjs.com/dist/email.min.js"></script>
    <script src="script.js" defer></script>
</head>
<body>
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

    <section id="home">
        <h2>Home</h2>
        <p>Welcome to our online bookshop! Explore a wide range of books and find your next favorite read.</p>
    </section>

    <section id="books">
        <h2>Books</h2>
        <div class="book-container">
            <!-- Books will be dynamically added here by JavaScript -->
        </div>
    </section>

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

    <section id="contact">
        <h2>Contact Us</h2>
        <p>If you have any questions, feel free to contact us at support@bookshop.com.</p>
    </section>

    <footer>
        <p>© 2025 Bookshop. All Rights Reserved.</p>
    </footer>
</body>
</html>
