# My-bookshop
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bookshop</title>
    <style>
        body { font-family: Arial, sans-serif; padding: 20px; }
        #shop { display: flex; flex-wrap: wrap; gap: 20px; }
        .product { border: 1px solid #ccc; padding: 10px; width: 200px; }
        .product img { width: 100%; }
        #cart { margin-top: 20px; }
        #cart ul { list-style: none; padding: 0; }
        #cart div { margin-bottom: 10px; }
    </style>
</head>
<body>

    <section id="shop">
        <h2>Shop</h2>
        <div class="product" data-id="1" data-name="Book Title 1" data-price="$5.00">
            <img src="book1.jpg" alt="Book 1 Cover" width="150">
            <h3>Book Title 1</h3>
            <p>Author: Jane Doe</p>
            <p>Price: $5.00</p>
            <button class="add-to-cart">Add to Cart</button>
        </div>
        <div class="product" data-id="2" data-name="Book Title 2" data-price="12.99">
            <img src="book2.jpg" alt="Book 2 Cover" width="150">
            <h3>Book Title 2</h3>
            <p>Author: John Smith</p>
            <p>Price: $12.99</p>
            <button class="add-to-cart">Add to Cart</button>
        </div>
        <div class="product" data-id="3" data-name="Book Title 3" data-price="18.99">
            <img src="book3.jpg" alt="Book 3 Cover" width="150">
            <h3>Book Title 3</h3>
            <p>Author: Emily Brown</p>
            <p>Price: $18.99</p>
            <button class="add-to-cart">Add to Cart</button>
        </div>

        <div id="cart">
            <h2>Shopping Cart</h2>
            <ul id="cart-items"></ul>
            <p>Delivery Fee: $3.00</p>
            <p>Total: $<span id="cart-total">0.00</span></p>
            
            <!-- Address and contact form -->
            <div>
                <label for="address">Address:</label>
                <input type="text" id="address" placeholder="Enter your address" required>
            </div>
            <div>
                <label for="city">City:</label>
                <input type="text" id="city" placeholder="Enter your city" required>
            </div>
            <div>
                <label for="phone">Phone Number:</label>
                <input type="text" id="phone" placeholder="Enter your phone number" required>
            </div>

            <button id="purchase-button">Submit Purchase</button>
        </div>
    </section>

    <script src="https://cdn.emailjs.com/dist/email.min.js"></script>
    <script>
        // Initialize EmailJS (replace with your actual User ID)
        emailjs.init("your_user_id");  // Replace with your EmailJS user ID

        document.addEventListener("DOMContentLoaded", () => {
            const cart = [];
            const cartItemsList = document.getElementById("cart-items");
            const cartTotal = document.getElementById("cart-total");

            // Add to Cart Functionality
            document.querySelectorAll(".add-to-cart").forEach((button) => {
                button.addEventListener("click", (e) => {
                    const productElement = e.target.closest(".product");
                    const id = productElement.dataset.id;
                    const name = productElement.dataset.name;
                    const price = parseFloat(productElement.dataset.price);

                    const existingProduct = cart.find((item) => item.id === id);
                    if (existingProduct) {
                        existingProduct.quantity++;
                    } else {
                        cart.push({ id, name, price, quantity: 1 });
                    }

                    updateCart();
                });
            });

            // Update Cart Display
            function updateCart() {
                cartItemsList.innerHTML = "";
                let total = 0;

                cart.forEach((item) => {
                    total += item.price * item.quantity;

                    const li = document.createElement("li");
                    li.textContent = `${item.name} - $${item.price} x ${item.quantity}`;
                    cartItemsList.appendChild(li);
                });

                // Add delivery fee
                total += 3;  // Delivery fee

                cartTotal.textContent = total.toFixed(2);
            }

            // Handle Purchase Button Click
            document.getElementById("purchase-button").addEventListener("click", () => {
                if (cart.length === 0) {
                    alert("Your cart is empty. Please add some items.");
                    return;
                }

                const address = document.getElementById("address").value;
                const city = document.getElementById("city").value;
                const phone = document.getElementById("phone").value;

                if (!address || !city || !phone) {
                    alert("Please fill out all the fields.");
                    return;
                }

                const totalAmount = cart.reduce((sum, item) => sum + item.price * item.quantity, 0) + 3; // Add delivery fee

                // Simulate sending the order (you could integrate with EmailJS or other services)
                sendPurchaseMessage(totalAmount, address, city, phone);
            });

            // Send Purchase Message to Your Email (using EmailJS)
            function sendPurchaseMessage(totalAmount, address, city, phone) {
                const orderDetails = cart.map(item => `${item.name} x${item.quantity}`).join("\n");
                const message = `
                    New purchase order:

                    ${orderDetails}

                    Delivery Address: ${address}
                    City: ${city}
                    Phone Number: ${phone}

                    Total Amount: $${totalAmount.toFixed(2)}
                `;

                // EmailJS example (replace with your own service details)
                emailjs.send("your_service_id", "your_template_id", {
                    to_email: "youremail@example.com", // Replace with your email
                    subject: "New Bookshop Order",
                    message: message,
                })
                .then((response) => {
                    alert("Order submitted successfully! You will be notified via email.");
                    cart.length = 0; // Clear the cart after submitting
                    updateCart(); // Refresh the cart display
                }, (error) => {
                    alert("There was an error submitting the order. Please try again.");
                });
            }
        });
    </script>

</body>
</html>
