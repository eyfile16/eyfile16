<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tienda de Tecnología</title>
    pip freeze > requirements.txt
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            overflow-x: hidden;
        }

        header {
            background-color: #333;
            color: white;
            padding: 10px 0;
            text-align: center;
            position: relative;
        }

        header h1 {
            border: 2px solid white;
            display: inline-block;
            padding: 10px;
            border-radius: 8px;
            margin: 0;
        }

        nav {
            background-color: #444;
            padding: 10px 0;
            text-align: center;
        }

        nav a {
            color: white;
            margin: 0 15px;
            text-decoration: none;
            font-size: 20px;
        }

        #cart-container {
            position: absolute;
            right: 20px;
            top: 10px;
            cursor: pointer;
            display: inline-block;
        }

        #cart-icon {
            position: relative;
        }

        #cart-icon img {
            width: 30px;
            height: 30px;
            border-radius: 50%;
        }

        #cart-count {
            background-color: red;
            color: white;
            border-radius: 50%;
            padding: 1px 3px;
            position: absolute;
            top: 0;
            right: 0;
            font-size: 12px;
        }

        #mini-cart {
            display: none;
            position: absolute;
            top: 40px;
            right: 0;
            background-color: rgb(106, 105, 105);
            border: 1px solid #ccc;
            padding: 10px;
            width: 300px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            z-index: 1000;
        }

        #mini-cart ul {
            list-style-type: none;
            padding: 0;
            margin: 0;
        }

        #mini-cart li {
            margin-bottom: 10px;
            display: flex;
            align-items: center;
        }

        #mini-cart img {
            width: 40px;
            height: 30px;
            object-fit: cover;
            margin-right: 10px;
        }

        #mini-cart button {
            background-color: red;
            color: white;
            border: none;
            padding: 5px;
            cursor: pointer;
            font-size: 12px;
        }

        #cart-container:hover #mini-cart {
            display: block;
        }

        #mini-cart.hidden {
            display: none;
        }

        #main-content {
            padding: 20px;
            background: rgb(0,0,0);
            background: radial-gradient(circle, rgba(0,0,0,1) 0%, rgba(255,0,0,1) 100%);
            display: flex;
            justify-content: center;
        }

        #products {
            display: flex;
            flex-wrap: wrap;
            gap: 20px;
            justify-content: center;
            width: 100%;
            max-width: 1200px;
        }

        .product {
            border: 1px solid #ccc;
            padding: 10px;
            width: 250px;
            box-sizing: border-box;
            background: #fff;
            text-align: center;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            transition: transform 0.3s ease;
        }

        .product img {
            width: 230px;
            height: 167px;
            object-fit: cover;
            display: block;
            margin: 0 auto;
            border-bottom: 1px solid #ccc;
        }

        .product h3 {
            margin: 10px 0;
            font-size: 24px;
        }

        .product p {
            margin: 10px 0;
        }

        .product button {
            background-color: red;
            color: white;
            border: none;
            padding: 10px;
            cursor: pointer;
            border-radius: 4px;
        }

        .product:hover {
            transform: scale(1.05);
        }

        #cart-section {
            display: none;
            flex-direction: column;
            align-items: center;
            padding: 20px;
            background-color: #f0f0f0;
        }

        #cart-section h2 {
            margin: 0;
            padding: 10px 0;
            font-size: 24px;
        }

        #cart-section p {
            margin: 10px 0;
            font-size: 18px;
        }
   
        #cart-section ul {
            list-style-type: none;
            padding: 0;
            margin: 0;
            width: 100%;
            text-align: center;
        }

        #cart-section li {
            margin-bottom: 10px;
            display: flex;
            align-items: center;
        }

        #cart-section img {
            width: 40px;
            height: 30px;
            object-fit: cover;
            margin-right: 10px;
        }

        #cart-section button {
            background-color: red;
            color: white;
            border: none;
            padding: 10px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <header>
        <h1>Tienda de Tecnología</h1>
    </header>
    <nav>
        <a href="#" id="home-link">Inicio</a>
        <a href="#">Productos</a>
        <div id="cart-container">
            <div id="cart-icon">
                <img src="https://w7.pngwing.com/pngs/225/984/png-transparent-computer-icons-shopping-cart-encapsulated-postscript-shopping-cart-angle-black-shopping-thumbnail.png" alt="Carrito">
                <span id="cart-count">0</span>
            </div>
            <div id="mini-cart" class="hidden">
                <ul id="mini-cart-items"></ul>
                <p>Total: $<span id="mini-cart-total">0</span></p>
                <button id="view-cart">Ver Carrito Completo</button>
            </div>
        </div>
    </nav>
    <main id="main-content">
        <section id="products"></section>
    </main>
    <section id="cart-section">
        <h2>Carrito Completo</h2>
        <ul id="full-cart-items"></ul>
        <p>Total: $<span id="full-cart-total">0</span></p>
        <button id="back-to-main">Volver a la Tienda</button>
    </section>
    <script>
        const cartItems = [];
        let cartTotal = 0;

        function addToCart(productId) {
            console.log(`Añadiendo producto ${productId} al carrito.`);
            const product = products.find(p => p.id === productId);
            if (!product) return;

            const cartItem = cartItems.find(item => item.id === productId);
            if (cartItem) {
                cartItem.quantity += 1;
            } else {
                cartItems.push({ ...product, quantity: 1 });
            }
            cartTotal += product.price;
            updateCartDisplay();
        }

        function removeFromCart(productId) {
            console.log(`Eliminando producto ${productId} del carrito.`);
            const cartItem = cartItems.find(item => item.id === productId);
            if (cartItem) {
                cartItem.quantity -= 1;
                cartTotal -= cartItem.price;
                if (cartItem.quantity === 0) {
                    const cartItemIndex = cartItems.indexOf(cartItem);
                    cartItems.splice(cartItemIndex, 1);
                }
                updateCartDisplay();
            }
        }

        function updateCartDisplay() {
            const miniCartItems = document.getElementById('mini-cart-items');
            const fullCartItems = document.getElementById('full-cart-items');
            miniCartItems.innerHTML = '';
            fullCartItems.innerHTML = '';

            let totalMiniCart = 0;
            let totalFullCart = 0;

            cartItems.forEach(item => {
                // Mini carrito
                const li = document.createElement('li');
                li.innerHTML = `<img src="${item.image}" alt="${item.name}"> ${item.name} - $${item.price} x ${item.quantity} <button onclick="removeFromCart(${item.id})">Eliminar</button>`;
                miniCartItems.appendChild(li);

                // Carrito completo
                const liFull = document.createElement('li');
                liFull.innerHTML = `<img src="${item.image}" alt="${item.name}"> ${item.name} - $${item.price} x ${item.quantity}`;
                fullCartItems.appendChild(liFull);

                totalMiniCart += item.price * item.quantity;
                totalFullCart += item.price * item.quantity;
            });

            document.getElementById('mini-cart-total').textContent = totalMiniCart;
            document.getElementById('full-cart-total').textContent = totalFullCart;
            document.getElementById('cart-count').textContent = cartItems.length;
        }

        document.addEventListener('DOMContentLoaded', () => {
            window.products = [
                { id: 1, name: 'Auriculares Inalámbricos', price: 100, image: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcThtpXYASPiWgNym8x4EJH_Tyy6cwmd1hZdPA&s', description: 'Auriculares inalámbricos con cancelación de ruido y batería de larga duración.' },
                { id: 2, name: 'Smartwatch Deportivo', price: 150, image: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQUzZvYLaXIqdbHRWHwFTeAWQ41C5KEZ-MVLQ&s', description: 'Reloj inteligente con seguimiento de actividad física y notificaciones de smartphone.' },
                { id: 3, name: 'Tableta 10', price: 200, image: 'https://th.bing.com/th/id/OIP.oxpHFY-e8KijQWLzVCQpuAHaF1?rs=1&pid=ImgDetMain', description: 'Tableta con pantalla de 10 pulgadas, procesador rápido y almacenamiento expansible.' },
                { id: 4, name: 'Teclado Mecánico RGB', price: 250, image: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSImiQ3YbgguLp2L-5SsrtJUtd-MhC3ixIwKg&s', description: 'Teclado mecánico con retroiluminación RGB y switches de alta precisión.' },
                { id: 5, name: 'Cámara Web HD', price: 300, image: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcT-Dwh0Q2IIfEhGcRYRpEORRcAdv_Sig2GR7g&s', description: 'Cámara web con resolución HD y micrófono incorporado para videoconferencias.' },
                { id: 6, name: 'Disco Duro Externo 1TB', price: 120, image: 'https://http2.mlstatic.com/D_NQ_NP_666214-MLA54950120963_042023-O.webp', description: 'Disco duro externo con 1TB de capacidad y transferencia de datos rápida.' },
                { id: 7, name: 'Router WiFi de Alto Rendimiento', price: 180, image: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQ5OEumt6U5AanFaSIfXVGhqufGYvIuwJmYzQ&s', description: 'Router con alta velocidad de transferencia y amplia cobertura de señal.' },
                { id: 8, name: 'Altavoces Bluetooth', price: 140, image: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQXZmYDgI2064uLx8gf4jQnu04RaaKtT6bUaQ&s', description: 'Altavoces portátiles con conectividad Bluetooth y sonido de alta calidad.' },
                { id: 9, name: 'Lámpara LED de Escritorio', price: 80, image: 'https://http2.mlstatic.com/D_NQ_NP_770268-MLU69767759042_062023-O.webp', description: 'Lámpara LED regulable con diseño moderno para oficina o estudio.' },
                { id: 10, name: 'Batería Externa 20,000mAh', price: 90, image: 'https://http2.mlstatic.com/D_NQ_NP_932013-MLC50563432995_072022-O.webp', description: 'Batería externa con alta capacidad para cargar dispositivos múltiples.' },
                { id: 11, name: 'Auriculares Bluetooth Premium', price: 180, image: 'https://fundasinspiral.com/236187-thickbox_default/auriculares-inalambricos-bluetooth-premium.jpg', description: 'Auriculares Bluetooth de alta calidad con sonido envolvente y cancelación de ruido activa. Diseñados para ofrecer una experiencia auditiva superior con una duración de batería de hasta 20 horas.' },
                { id: 12, name: 'Monitor Curvo 27" 4K', price: 350, image: 'https://images.samsung.com/is/image/samsung/p6pim/pe/lc27r500fhlxpe/gallery/pe-cr50-32-409014-lc27r500fhlxpe-534165227?$650_519_PNG$', description: 'Monitor curvo de 27 pulgadas con resolución 4K y tecnología de refresco rápido. Ideal para gamers y profesionales que buscan la mejor calidad de imagen y una experiencia inmersiva.' }
            ];

            const productsContainer = document.getElementById('products');

            function renderProducts() {
                products.forEach(product => {
                    const productDiv = document.createElement('div');
                    productDiv.className = 'product';
                    productDiv.innerHTML = `
                        <img src="${product.image}" alt="${product.name}">
                        <h3>${product.name}</h3>
                        <p>${product.description}</p>
                        <p>$${product.price}</p>
                        <button onclick="addToCart(${product.id})">Añadir al Carrito</button>
                    `;
                    productsContainer.appendChild(productDiv);
                });
            }

            document.getElementById('view-cart').addEventListener('click', () => {
                document.getElementById('main-content').style.display = 'none';
                document.getElementById('cart-section').style.display = 'flex';
            });

            document.getElementById('back-to-main').addEventListener('click', () => {
                document.getElementById('main-content').style.display = 'flex';
                document.getElementById('cart-section').style.display = 'none';
            });

            renderProducts();
        });
    </script>
</body>
</html>

