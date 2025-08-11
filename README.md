<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Chicha Sena - Invoice</title>
  <style>
    /* Estilos existentes */
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background: #fff8ec;
      margin: 0;
      padding: 0;
      color: #333;
    }
    header {
      background: #ffc107;
      color: #000;
      padding: 2em 1em;
      text-align: center;
      box-shadow: 0 4px 8px rgba(0,0,0,0.1);
      position: relative;
    }
    h1 {
      margin: 0.2em 0;
      font-size: 2em;
    }
    .language-button {
      position: absolute;
      top: 1em;
      right: 1em;
      background: #ff9800;
      color: white;
      border: none;
      padding: 0.5em 1em;
      border-radius: 5px;
      cursor: pointer;
      transition: background 0.3s;
    }
    .language-button:hover {
      background: #e68900;
    }
    main {
      max-width: 500px;
      margin: 2em auto;
      padding: 1em;
      background: #fff;
      border-radius: 10px;
      box-shadow: 0 2px 10px rgba(0,0,0,0.1);
    }
    label {
      display: block;
      margin-top: 1em;
      font-weight: bold;
    }
    input[type="text"], input[type="tel"], select, input[type="number"] {
      width: 100%;
      padding: 0.5em;
      border: 1px solid #ccc;
      border-radius: 5px;
      margin-top: 0.3em;
      box-sizing: border-box;
    }
    button[type="submit"] {
      background: #ff9800;
      color: white;
      border: none;
      padding: 1em;
      width: 100%;
      border-radius: 5px;
      margin-top: 2em;
      font-size: 1em;
      cursor: pointer;
      transition: background 0.3s;
    }
    button[type="submit"]:hover {
      background: #e68900;
    }
    .whatsapp-button {
      background: #25D366;
      color: white;
      border: none;
      padding: 1em;
      width: 100%;
      border-radius: 5px;
      margin-top: 1em;
      font-size: 1em;
      cursor: pointer;
      transition: background 0.3s;
      display: none;
    }
    .whatsapp-button:hover {
      background: #128C7E;
    }
    #invoice {
      margin-top: 2em;
      padding: 1.5em;
      border: 2px solid #ff9800;
      border-radius: 10px;
      background: #fffbea;
      box-shadow: 0 2px 12px rgba(255, 152, 0, 0.3);
      display: none;
      white-space: pre-wrap;
    }
    #invoice h2 {
      color: #ff9800;
      margin-top: 0;
      text-align: center;
    }
    #invoice p {
      font-size: 1.1em;
      margin: 0.5em 0;
    }
    footer {
      text-align: center;
      padding: 1em;
      font-size: 0.9em;
      color: #777;
    }
  </style>
</head>
<body>
  <header>
    <h1 id="headerTitle">Chicha Sena🥤</h1>
    <img src="https://i.postimg.cc/WzK2pkvQ/unnamed-removebg-preview.png" alt="logo chicha" style="max-width: 300px; margin: 10px auto;" />
    <p id="headerSubtitle">Online orders from your phone — Fast, tasty, and no lines</p>
    <button type="button" id="languageButton" class="language-button">Cambiar a Español</button>
  </header>

  <main>
    <form id="orderForm">
      <label for="firstName" id="labelFirstName">First Name:</label>
      <input type="text" id="firstName" name="firstName" placeholder="Your name" required />

      <label for="lastName" id="labelLastName">Last Name:</label>
      <input type="text" id="lastName" name="lastName" placeholder="Your last name" required />

      <label for="phone" id="labelPhone">Phone Number:</label>
      <input type="tel" id="phone" name="phone" placeholder="E.g: +58 412 1234567" pattern="^\+?\d{7,15}$" required />



      <label for="size" id="labelSize">Size of Chicha:</label>
      <select id="size" name="size" required>
        <option value="Pequeña" data-price="1000">Small - $1.000</option>
        <option value="Mediana" data-price="2000">Medium - $2.000</option>
        <option value="Grande" data-price="3000">Large - $3.000</option>
      </select>

      <label for="toppings" id="labelToppings">Toppings:</label>
      <select id="toppings" name="toppings">
        <option value="Canela" data-price="0">Cinnamon</option>
        <option value="Leche condensada" data-price="500">Condensed milk - $500</option>
        <option value="Galleta" data-price="300">Cookie - $300</option>
        <option value="Ninguno" data-price="0">No topping</option>
      </select>

      <label for="quantity" id="labelQuantity">Quantity:</label>
      <input type="number" id="quantity" name="quantity" min="1" value="1" required />

      <label for="payment" id="labelPayment">Payment Method:</label>
      <select id="payment" name="payment">
        <option value="Efectivo">Cash</option>
        <option value="Nequi">Nequi</option>
      </select>

      <button type="submit">Generate Invoice</button>
    </form>

    <div id="invoice"></div>
    <button id="whatsappButton" class="whatsapp-button">📱 Send Invoice via WhatsApp</button>
  </main>

  <footer>
    © 2025 Chicha Sena | Developed by Los 7 Babys
  </footer>

  <script>
    const form = document.getElementById('orderForm');
    const invoice = document.getElementById('invoice');
    const whatsappButton = document.getElementById('whatsappButton');
    const languageButton = document.getElementById('languageButton');

    let isSpanish = false;
    let currentInvoiceData = null;

    languageButton.addEventListener('click', () => {
      isSpanish = !isSpanish;
      updateLanguage();
    });

    function updateLanguage() {
      if (isSpanish) {
        document.documentElement.lang = "es";
        document.getElementById('headerTitle').innerText = "Chicha Sena🥤";
        document.getElementById('headerSubtitle').innerText = "Pedidos online desde tu celular — Rápido, sabroso y sin filas";
        document.getElementById('labelFirstName').innerText = "Nombre:";
        document.getElementById('labelLastName').innerText = "Apellido:";
        document.getElementById('labelPhone').innerText = "Número de celular:";

        document.getElementById('labelSize').innerText = "Tamaño de la chicha:";
        document.getElementById('labelToppings').innerText = "Toppings:";
        document.getElementById('labelQuantity').innerText = "Cantidad:";
        document.getElementById('labelPayment').innerText = "Método de pago:";
        document.querySelector('button[type="submit"]').innerText = "Generar factura";
        whatsappButton.innerText = "📱 Enviar factura por WhatsApp";
        languageButton.innerText = "Switch to English";
      } else {
        document.documentElement.lang = "en";
        document.getElementById('headerTitle').innerText = "Chicha Sena🥤";
        document.getElementById('headerSubtitle').innerText = "Online orders from your phone — Fast, tasty, and no lines";
        document.getElementById('labelFirstName').innerText = "First Name:";
        document.getElementById('labelLastName').innerText = "Last Name:";
        document.getElementById('labelPhone').innerText = "Phone Number:";

        document.getElementById('labelSize').innerText = "Size of Chicha:";
        document.getElementById('labelToppings').innerText = "Toppings:";
        document.getElementById('labelQuantity').innerText = "Quantity:";
        document.getElementById('labelPayment').innerText = "Payment Method:";
        document.querySelector('button[type="submit"]').innerText = "Generate Invoice";
        whatsappButton.innerText = "📱 Send Invoice via WhatsApp";
        languageButton.innerText = "Cambiar a Español";
      }
    }

    form.addEventListener('submit', (e) => {
      e.preventDefault();

      // Customer data
      const firstName = form.firstName.value.trim();
      const lastName = form.lastName.value.trim();
      const phone = form.phone.value.trim();

      // Order data
      const sizeSelect = form.size;
      const toppingsSelect = form.toppings;
      const quantityInput = form.quantity;
      const paymentSelect = form.payment;

      const size = sizeSelect.value;
      const sizePrice = parseInt(sizeSelect.options[sizeSelect.selectedIndex].dataset.price);

      const topping = toppingsSelect.value;
      const toppingPrice = parseInt(toppingsSelect.options[toppingsSelect.selectedIndex].dataset.price);

      const quantity = parseInt(quantityInput.value);
      const payment = paymentSelect.value;

      // Current date and time
      const now = new Date();
      const dateStr = now.toLocaleDateString();
      const timeStr = now.toLocaleTimeString();

      // Total calculation
      let subtotal = (sizePrice + toppingPrice) * quantity;
      let iva = 0;

      if (quantity >= 10) {
        iva = subtotal * 0.19; // 19% VAT
      }

      const total = subtotal + iva;

      // Store invoice data for WhatsApp
      currentInvoiceData = {
        firstName,
        lastName,
        phone,
        size,
        sizePrice,
        topping,
        toppingPrice,
        quantity,
        payment,
        dateStr,
        timeStr,
        subtotal,
        iva,
        total
      };

      invoice.style.display = 'block';
      invoice.innerHTML = `
        <h2>Invoice</h2>
        <p><strong>Customer:</strong> ${firstName} ${lastName}</p>
        <p><strong>Phone:</strong> ${phone}</p>
        <p><strong>Date:</strong> ${dateStr}</p>
        <p><strong>Time:</strong> ${timeStr}</p>
        <hr />
        <p><strong>Quantity:</strong> ${quantity}</p>
        <p><strong>Size:</strong> ${size} - $${sizePrice.toLocaleString()}</p>
        <p><strong>Topping:</strong> ${topping} - $${toppingPrice.toLocaleString()}</p>
        <hr />
        <p><strong>Subtotal:</strong> $${subtotal.toLocaleString()}</p>
        <p><strong>VAT (19%):</strong> $${iva.toLocaleString()}</p>
        <p><strong>Total to pay:</strong> $${total.toLocaleString()}</p>
        <p style="font-size:0.9em; color:#555; margin-top:1em;">
          Thank you for your order. See you soon at the stand! 🎉
        </p>
      `;
      
      whatsappButton.style.display = 'block';
      invoice.scrollIntoView({behavior: 'smooth'});
    });

    whatsappButton.addEventListener('click', () => {
      if (!currentInvoiceData) return;

      const data = currentInvoiceData;
      
      // Create WhatsApp message
      const message = `🥤 *CHICHA SENA - INVOICE* 🥤

👤 *Customer:* ${data.firstName} ${data.lastName}
📞 *Phone:* ${data.phone}
📅 *Date:* ${data.dateStr}
🕐 *Time:* ${data.timeStr}

━━━━━━━━━━━━━━━━━━

📦 *ORDER DETAILS:*
• *Quantity:* ${data.quantity}
• *Size:* ${data.size} - $${data.sizePrice.toLocaleString()}
• *Topping:* ${data.topping} - $${data.toppingPrice.toLocaleString()}

━━━━━━━━━━━━━━━━━━

💰 *PAYMENT SUMMARY:*
• *Subtotal:* $${data.subtotal.toLocaleString()}
• *VAT (19%):* $${data.iva.toLocaleString()}
• *TOTAL TO PAY:* $${data.total.toLocaleString()}

💳 *Payment Method:* ${data.payment}

━━━━━━━━━━━━━━━━━━

Thank you for choosing Chicha Sena! 🎉
Your delicious chicha will be ready soon! 🥤✨`;

      // Clean and format phone number for WhatsApp (fixed number)
      const businessPhone = '573013591992';

      // Create WhatsApp URL
      const whatsappURL = `https://wa.me/${businessPhone}?text=${encodeURIComponent(message)}`;
      
      // Open WhatsApp
      window.open(whatsappURL, '_blank');
    });
  </script>
</body>
</html>
