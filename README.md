# B2HOKKI

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Rumah Makan B2 Hokky</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            display: flex;
        }
        header {
            background-color: #ff6600;
            color: #fff;
            padding: 1rem;
            text-align: center;
            font-size: 1.5rem;
            font-weight: bold;
        }
        .sidebar {
            width: 200px;
            background-color: #333;
            color: white;
            height: 100vh;
            position: fixed;
            padding: 1rem;
        }
        .sidebar h2 {
            margin-bottom: 1.5rem;
        }
        .sidebar a {
            display: block;
            color: white;
            text-decoration: none;
            margin-bottom: 1rem;
            padding: 0.5rem;
            border-radius: 4px;
            transition: background-color 0.3s;
        }
        .sidebar a:hover {
            background-color: #555;
        }
        .container {
            margin-left: 220px;
            padding: 2rem;
            width: calc(100% - 220px);
        }
        section {
            margin-bottom: 2rem;
        }
        .menu-item {
            display: flex;
            align-items: center;
            margin-bottom: 1rem;
            cursor: pointer;
            border: 1px solid #ddd;
            padding: 1rem;
            border-radius: 5px;
            transition: box-shadow 0.3s;
        }
        .menu-item:hover {
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
        }
        .menu-item img {
            width: 100px;
            height: 100px;
            margin-right: 1rem;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 1rem;
        }
        table, th, td {
            border: 1px solid #ddd;
        }
        th, td {
            padding: 0.75rem;
            text-align: left;
        }
        th {
            background-color: #f4f4f4;
        }
        .btn {
            padding: 0.5rem 1rem;
            background-color: #ff6600;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        .btn:hover {
            background-color: #cc5200;
        }
        .hidden {
            display: none;
        }
        .visible {
            display: block;
        }
        .modal {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: white;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            padding: 2rem;
            border-radius: 8px;
            z-index: 1000;
        }
        .close-btn {
            float: right;
            background: none;
            border: none;
            font-size: 1.5rem;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <div class="sidebar">
        <h2>Menu</h2>
        <a href="#menu-makanan" onclick="showSection('menu-makanan')">Menu Makanan</a>
        <a href="#ulasan" onclick="showSection('ulasan')">Ulasan</a>
        <a href="#keluar">Keluar</a>
    </div>

    <div class="container">
        <header>
            Selamat Datang di Rumah Makan B2 Hokky
        </header>
        
        <!-- Section Menu Makanan -->
        <section id="menu-makanan" class="visible">
            <h2>Menu Makanan</h2>
            <div class="menu-item" onclick="showOrderForm('Babi Guling', 50000)">
                <img src="20.webp" alt="Babi Guling">
                <div>
                    <h3>Babi Guling</h3>
                    <p>Harga: Rp50,000</p>
                </div>
            </div>
            <div class="menu-item" onclick="showOrderForm('Sate Babi', 40000)">
                <img src="sate.jpeg" alt="Sate Babi">
                <div>
                    <h3>Sate Babi</h3>
                    <p>Harga: Rp40,000</p>
                </div>
            </div>
            <div class="menu-item" onclick="showOrderForm('Babi Rica', 45000)">
                <img src="sei jumbo.jpeg" alt="Babi Rica">
                <div>
                    <h3>Babi Rica</h3>
                    <p>Harga: Rp45,000</p>
                </div>
            </div>
        </section>

        <!-- Section Ulasan -->
        <section id="ulasan" class="hidden">
            <h2>Ulasan</h2>
            <textarea id="review" rows="5" class="form-control" placeholder="Tulis ulasan Anda di sini..."></textarea>
            <button class="btn mt-2" onclick="submitReview()">Kirim Ulasan</button>
            <div id="review-list" class="mt-4">
                <!-- Reviews will be displayed here -->
            </div>
        </section>

    </div>

    <!-- Popup Form -->
    <div id="order-modal" class="modal hidden">
        <div class="modal-content">
            <button class="close-btn" onclick="closeOrderForm()">&times;</button>
            <h2>Form Pemesanan</h2>
            <form id="order-form">
                <label for="menu-name">Nama Menu:</label><br>
                <input type="text" id="menu-name" name="menu-name" readonly><br><br>

                <label for="price">Harga Satuan:</label><br>
                <input type="text" id="price" name="price" readonly><br><br>

                <label for="table-number">Nomor Meja:</label><br>
                <input type="number" id="table-number" name="table-number" min="1" required><br><br>

                <label for="quantity">Jumlah:</label><br>
                <input type="number" id="quantity" name="quantity" min="1" value="1" required><br><br>

                <label for="total">Total Harga:</label><br>
                <input type="text" id="total" name="total" readonly><br><br>

                <button type="button" class="btn" onclick="submitOrder()">Pesan</button>
            </form>
        </div>
    </div>

    <script>
        const orders = JSON.parse(localStorage.getItem('orders')) || [];
        const reviews = JSON.parse(localStorage.getItem('reviews')) || [];

        function showSection(sectionId) {
            const sections = document.querySelectorAll('section');
            sections.forEach(section => section.classList.add('hidden'));
            document.getElementById(sectionId).classList.remove('hidden');
        }

        function showOrderForm(menuName, price) {
            document.getElementById('menu-name').value = menuName;
            document.getElementById('price').value = price;
            document.getElementById('total').value = price;

            document.getElementById('quantity').addEventListener('input', (e) => {
                const quantity = parseInt(e.target.value) || 1;
                document.getElementById('total').value = price * quantity;
            });

            document.getElementById('order-modal').classList.remove('hidden');
        }

        function closeOrderForm() {
            document.getElementById('order-modal').classList.add('hidden');
        }

        function submitOrder() {
            const menu = document.getElementById('menu-name').value;
            const quantity = parseInt(document.getElementById('quantity').value);
            const totalPrice = parseInt(document.getElementById('total').value);
            const tableNumber = parseInt(document.getElementById('table-number').value);

            const order = {
                tableNumber,
                name: "Pelanggan",
                menu,
                quantity,
                totalPrice
            };

            orders.push(order);
            localStorage.setItem('orders', JSON.stringify(orders));

            alert(`Pesanan berhasil!\nMenu: ${menu}\nJumlah: ${quantity}\nTotal: Rp${totalPrice.toLocaleString()}\nNomor Meja: ${tableNumber}`);
            closeOrderForm();
        }

        function submitReview() {
            const reviewText = document.getElementById('review').value.trim();
            if (!reviewText) {
                alert("Silakan isi ulasan Anda terlebih dahulu.");
                return;
            }

            reviews.push(reviewText);
            localStorage.setItem('reviews', JSON.stringify(reviews));
            document.getElementById('review').value = "";
            displayReviews();
        }

        function displayReviews() {
            const reviewList = document.getElementById('review-list');
            reviewList.innerHTML = reviews.map(review => `<p>${review}</p>`).join('');
        }

        document.addEventListener('DOMContentLoaded', displayReviews);
    </script>
</body>
</html>
