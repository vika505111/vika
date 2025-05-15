<!DOCTYPE html>
<html lang="ar">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>متجر المشروبات</title>
  <style>
    body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  direction: rtl;
  text-align: center;
  padding-bottom: 100px;
  margin: 0;
}

.store-header {
  font-size: 18px;
  border: 2px solid green;
  border-radius: 12px;
  padding: 10px 15px;
  margin: 20px auto;
  width: fit-content;
  background: #f0fff0;
  box-shadow: 2px 2px 10px rgba(0,0,0,0.1);
}

    .product {
      border: 1px solid #ccc;
      margin: 10px;
      padding: 10px;
      width: 40%;
      max-width: 300px;
      box-sizing: border-box;
    }

    .product img {
      width: 100px;
      height: 100px;
      object-fit: cover;
      margin-bottom: 10px;
    }

    .products-container {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
    }

    .pagination {
      margin-top: 20px;
      display: flex;
      justify-content: center;
      gap: 10px;
    }

    .pagination button {
  width: 35px;
  height: 35px;
  border-radius: 50%;
  border: 1px solid #ccc;
  background-color: white;
  cursor: pointer;
  font-size: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: 0.2s;
}

.pagination button:hover {
  background-color: seagreen;
  color: white;
}

    input[type="number"] {
      width: 60px;
      margin-top: 5px;
      background-color: #fff8c4;
      border: 1px solid #ccc;
      text-align: center;
    }

    .form-container {
      margin-top: 30px;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 10px;
    }

    .form-container input,
    .form-container textarea {
      width: 90%;
      max-width: 300px;
      padding: 8px;
      font-size: 14px;
    }

    .form-container button {
  background-color: #25D366;
  color: white;
  border: none;
  border-radius: 8px;
  padding: 10px 20px;
  font-size: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  cursor: pointer;
  transition: 0.3s;
}

.form-container button:hover {
  background-color: #1ebe5b;
  transform: scale(1.05);
}

.form-container button img {
  width: 20px;
  height: 20px;
    }

    .total-price {
      margin-top: 20px;
      font-size: 18px;
      font-weight: bold;
      color: seagreen;
    }

    .cart-icon {
      position: fixed;
      bottom: 20px;
      left: 20px;
      background-color: aliceblue;
      color: white;
      border-radius: 50%;
      width: 60px;
      height: 60px;
      font-size: 20px;
      display: flex;
      align-items: center;
      justify-content: center;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
      z-index: 1000;
      cursor: pointer;
    }

    .cart-icon span {
      position: absolute;
      top: -5px;
      right: -5px;
      background-color: red;
      color: white;
      border-radius: 50%;
      font-size: 14px;
      padding: 2px 6px;
    }

    .cart-popup {
      display: none;
      position: fixed;
      bottom: 90px;
      left: 20px;
      width: 300px;
      max-height: 400px;
      overflow-y: auto;
      background-color: lightgray;
      border: 2px solid seagreen;
      border-radius: 10px;
      padding: 10px;
      box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
      z-index: 999;
      font-family: 'Segoe UI', sans-serif;
      text-align: right;
    }

    .cart-popup h4 {
      margin-top: 0;
      color: seagreen;
    }

    .cart-popup ul {
      list-style: none;
      padding: 0;
      margin: 0;
    }

    .cart-popup li {
      padding: 6px 0;
      border-bottom: 1px solid #ccc;
      font-size: 15px;
    }
  </style>
</head>
<body>
  <img src="https://i.postimg.cc/qRnzj4z0/smalldrink-20250513-013309-0000.png" alt="واجهة المتجر"
    style="width: 100%; max-height: 300px; object-fit: cover;">
  <h2 class="store-header">يمكنك طلب أي منتج لم تجده في المتجر في خانة الملاحظات لنقوم بتوفيره لك</h2>

  <div id="products" class="products-container"></div>
  <div class="pagination" id="pagination"></div>

  <div class="total-price" id="totalPrice">المبلغ الإجمالي: 0 دج</div>

  <div class="form-container">
    <textarea id="note" placeholder="ملاحظة (اختياري)"></textarea>
    <input type="text" id="address" placeholder="العنوان (إجباري)" required>
    <input type="tel" id="phone" placeholder="رقم الهاتف (إجباري)" required>
    <button onclick="sendOrder()">
  <img src="https://upload.wikimedia.org/wikipedia/commons/6/6b/WhatsApp.svg" alt="واتساب">
  طلب عبر واتساب
</button>
<p style="text-align: center; color: #555; margin-top: 10px;">يرجى تفعيل خدمة تحديد الموقع لتسريع عملية التوصيل بدقة.</p>
<p style="text-align: center; color: #555; margin-top: 5px;">
  للاستفسار: <a href="tel:0540667186" style="color: #007bff; text-decoration: none;">0540667186</a>
</p>
  </div>

  <div class="cart-icon" onclick="toggleCartPopup()">
    🛒
    <span id="cartCount">0</span>
  </div>

  <div class="cart-popup" id="cartPopup">
    <h4>محتوى السلة:</h4>
    <ul id="cartDetails"></ul>
  </div>

  <script>
    const products = [
  { id: 1, name: "ariaf 0.5 L", price: 185, pack: 12, img: "https://i.postimg.cc/vTFRv1Bk/images-17.jpg" },
  { id: 2, name: "azro pack 6", price: 440, pack: 6, img: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRGjgTUnWV8twl-jINHkTu6EJeprxEO0AICLZ-1ci95ak5Rtwhw0ZkEe0_z&s=10" },
  { id: 3, name: "bifa 20 cl", price: 520, pack: 30, img: "https://i.postimg.cc/GpXhzjwN/4225.jpg" },
  { id: 4, name: "bifa joy 12.5 cl", price: 430, pack: 48, img: "https://i.postimg.cc/K8FMHQwv/144230829095747170.jpg" },
  { id: 5, name: "candia 12.5 cl", price: 560, pack: 18, img: "https://i.postimg.cc/Prvyj6SW/Candia-Candy-Choco-125-ML.jpg" },
  { id: 6, name: "candia 20 cl", price: 820, pack: 18, img: "https://i.postimg.cc/kX215Mgt/images-18.jpg" },
  { id: 7, name: "coca bouteille 30 cl", price: 580, pack: 12, img: "https://i.postimg.cc/MGHn0P3c/Coca-Cola-30-cl.jpg" },
  { id: 8, name: "coca canette", price: 1430, pack: 24, img: "https://i.postimg.cc/nz7cT64L/coca-cola-original-canette-format-slim-33-cl-lot-24-72255-00-J.jpg" },
  { id: 9, name: "flash smaili", price: 500, pack: 156, img: "https://i.postimg.cc/zDc0T6dh/img-1-1746304040475.jpg" },
  { id: 10, name: "flash smurf", price: 550, pack: 180, img: "https://i.postimg.cc/k5LW2PMM/img-2-1746470252315.jpg" },
  { id: 11, name: "hamoud canette 24 cl", price: 280, pack: 6, img: "https://i.postimg.cc/dtSQrLhy/Hamoud-Boualem-0-33-L-Canettes.jpg" },
  { id: 12, name: "ifri 0.5 L", price: 200, pack: 12, img: "https://i.postimg.cc/WbDFJfKh/Ifri-Eau-Minerales-0-5-L.jpg" },
  { id: 13, name: "ifri vichi arome 33 cl", price: 360, pack: 12, img: "https://i.postimg.cc/JhyMp9Nk/images-1.jpg" },
  { id: 14, name: "ifri vichi nature 33 cl", price: 300, pack: 12, img: "https://i.postimg.cc/mk9PKP77/images-2.jpg" },
  { id: 15, name: "ifruit jus 30 cl", price: 480, pack: 12, img: "https://i.postimg.cc/W446C7zr/06130093056890-a1n1-s01.jpg" },
  { id: 16, name: "ifruit au lait 20 cl", price: 675, pack: 18, img: "https://i.postimg.cc/8kYd3Wgf/IMG-20250501-140930.jpg" },
  { id: 17, name: "ifruit au lait 30 cl", price: 530, pack: 12, img: "https://i.postimg.cc/ncHxysKZ/Photo00001006.jpg" },
  { id: 18, name: "ifruit au lait pack 33 cl", price: 700, pack: 12, img: "https://i.postimg.cc/Y0ZVqKSh/Photo00001037.jpg" },
  { id: 19, name: "izem bouteille 33 cl", price: 600, pack: 12, img: "https://i.postimg.cc/B6p3b4sR/Photo00001002.jpg" },
  { id: 20, name: "izem canette", price: 420, pack: 6, img: "https://i.postimg.cc/6pS6d9kZ/Photo00001040.jpg" },
  { id: 21, name: "izem canette 0.5 L", price: 750, pack: 6, img: "https://i.postimg.cc/dVL2jk4P/img-1-1746912952166.jpg" },
  { id: 22, name: "luxe d'orge", price: 1700, pack: 24, img: "https://i.postimg.cc/g2hQTmw8/images-19.jpg" },
  { id: 23, name: "manbaa 0.5 L", price: 185, pack: 12, img: "https://i.postimg.cc/cJshBSfs/images-20.jpg" },
  { id: 24, name: "manbaa 1.5 L", price: 175, pack: 6, img: "https://i.postimg.cc/HkTvBXgD/watzr-1.png" },
  { id: 25, name: "n'gaous 17.5 cl", price: 1050, pack: 24, img: "https://i.postimg.cc/cCQTc4kL/front-fr-4-full.jpg" },
  { id: 26, name: "n'gaous canette", price: 1230, pack: 24, img: "https://i.postimg.cc/3JTtxmW4/images-5.jpg" },
  { id: 27, name: "ninja 40 cl", price: 610, pack: 12, img: "https://i.postimg.cc/JhDNqzGC/images-21.jpg" },
  { id: 28, name: "ninja canette", price: 410, pack: 6, img: "https://i.postimg.cc/m2p15GxL/images-22.jpg" },
  { id: 29, name: "pitbull", price: 570, pack: 6, img: "https://i.postimg.cc/0QvzwkSf/httpsjumbocolombiaio-vtexassets-comarquivosids1944788410376042498-1-jpgv637814055218800000-2023-10-2.jpg" },
  { id: 30, name: "ramy au lait", price: 550, pack: 12, img: "https://i.postimg.cc/wjhHshQC/images-23.jpg" },
  { id: 31, name: "ramy jus bouteille", price: 530, pack: 12, img: "https://i.postimg.cc/3xC5w3K7/images-11.jpg" },
  { id: 32, name: "ramy canette jus", price: 300, pack: 6, img: "https://i.postimg.cc/Y9PTP9xN/images-7.jpg" },
  { id: 33, name: "rouiba 20 cl", price: 580, pack: 17, img: "https://i.postimg.cc/HkHWzZnY/images.jpg" },
  { id: 34, name: "Schweppes bouteille 30 cl", price: 620, pack: 12, img: "https://i.postimg.cc/ZR9JnX7c/images-16.jpg" },
  { id: 35, name: "Schweppes canette", price: 400, pack: 6, img: "https://i.postimg.cc/cJrpBwR1/055393-d1.jpg" },
  { id: 36, name: "Schweppes gold canette", price: 440, pack: 6, img: "https://i.postimg.cc/ZYr1HTdN/Remove-bg-ai-1710274534220.png" },
  { id: 37, name: "super bitter", price: 430, pack: 12, img: "https://i.postimg.cc/Bbns10sp/A1e7c5d3f4a1c4c2586d3003cff195bf3t.jpg" },
  { id: 38, name: "tazedj bouteille 33 cl", price: 495, pack: 12, img: "https://i.postimg.cc/6qMWHQzF/img-1-1746911663754.jpg" },
  { id: 39, name: "tazedj pulp", price: 350, pack: 8, img: "https://i.postimg.cc/YS0G7rFD/images-24.jpg" },
  { id: 40, name: "TNT", price: 630, pack: 12, img: "https://i.postimg.cc/0jYrnbgj/TNT-1.jpg" },
  { id: 41, name: "vichi tazedj nature", price: 210, pack: 8, img: "https://i.postimg.cc/tRndQjpF/IMG-20250510-110733.jpg" },
  { id: 42, name: "vichi tazedj arom citron", price: 260, pack: 8, img: "https://i.postimg.cc/2602yF3M/IMG-20250510-110714.jpg" },
  { id: 43, name: "izem 33 cl fruits rouges", price: 710, pack: 12, img: "https://i.postimg.cc/cHKB1KtB/IMG-20250510-104818.jpg" },
  { id: 44, name: "Orangina 33 cl", price: 480, pack: 12, img: "https://i.postimg.cc/Fz9cz5Hy/orangina-bouteille-plastique-de-50-cl.jpg" },
  { id: 45, name: "rouiba excellence 25 cl", price: 660, pack: 12, img: "https://i.postimg.cc/wTry8sLc/Rouiba-Excellences-100-Naturel-0-25-L.jpg" }
    ];

const productsPerPage = 16;
let currentPage = 1;
const quantities = {};
const phoneNumber = "213540667186";

function displayProducts(page) {
  const start = (page - 1) * productsPerPage;
  const end = start + productsPerPage;
  const visibleProducts = products.slice(start, end);

  document.getElementById("products").innerHTML = visibleProducts.map(p => `
    <div class="product">
      <img src="${p.img}" alt="${p.name}">
      <div><strong>${p.name}</strong></div>
      <div style="color: green;">السعر: ${p.price} دج</div>
      <div>الكمية في العلبة ${p.pack}</div>
      <div>
        عدد :
        <input type="number" min="0" placeholder="0" id="qty-${p.id}" 
          oninput="this.value = Math.max(0, this.value)" 
          value="${quantities[p.id] || ''}">
      </div>
    </div>
  `).join("");

  visibleProducts.forEach(p => {
    const input = document.getElementById(`qty-${p.id}`);
    input.addEventListener('input', () => {
      quantities[p.id] = input.value;
      updateTotalPrice();
    });
  });

  updateTotalPrice();
}

function updatePagination() {
  const totalPages = Math.ceil(products.length / productsPerPage);
  const container = document.getElementById("pagination");
  container.innerHTML = "";

  const prevBtn = document.createElement("button");
  prevBtn.textContent = "السابق";
  prevBtn.disabled = currentPage === 1;
  prevBtn.onclick = () => { currentPage--; update(); };

  const nextBtn = document.createElement("button");
  nextBtn.textContent = "التالي";
  nextBtn.disabled = currentPage === totalPages;
  nextBtn.onclick = () => { currentPage++; update(); };

  container.appendChild(prevBtn);

  for (let i = 1; i <= totalPages; i++) {
    if (i === 1 || i === totalPages || (i >= currentPage - 1 && i <= currentPage + 1)) {
      const pageBtn = document.createElement("button");
      pageBtn.textContent = i;
      pageBtn.onclick = () => { currentPage = i; update(); };
      if (i === currentPage) pageBtn.style.backgroundColor = "lightgreen";
      container.appendChild(pageBtn);
    } else if ((i === 2 && currentPage > 3) || (i === totalPages - 1 && currentPage < totalPages - 2)) {
      const dots = document.createElement("span");
      dots.textContent = "...";
      container.appendChild(dots);
    }
  }

  container.appendChild(nextBtn);
}

function update() {
  displayProducts(currentPage);
  updatePagination();
}

function updateTotalPrice() {
  let total = 0;
  for (const id in quantities) {
    const qty = parseInt(quantities[id]) || 0;
    const product = products.find(p => p.id == id);
    if (product) total += qty * product.price;
  }
  document.getElementById("totalPrice").textContent = `المبلغ الإجمالي: ${total} دج`;
  updateCartCount();
}

function updateCartCount() {
  let count = 0;
  for (const id in quantities) {
    count += parseInt(quantities[id]) || 0;
  }
  document.getElementById("cartCount").textContent = count;
}

function toggleCartPopup() {
  const popup = document.getElementById("cartPopup");
  const list = document.getElementById("cartDetails");
  list.innerHTML = "";

  let hasItems = false;
  for (const id in quantities) {
    const qty = parseInt(quantities[id]);
    if (qty > 0) {
      const product = products.find(p => p.id == id);
      const li = document.createElement("li");
      li.textContent = `${product.name} × ${qty} = ${qty * product.price} دج`;
      list.appendChild(li);
      hasItems = true;
    }
  }

  if (!hasItems) list.innerHTML = "<li>السلة فارغة.</li>";
  popup.style.display = popup.style.display === "block" ? "none" : "block";
}

function sendOrder() {
  const note = document.getElementById("note").value.trim();
  const address = document.getElementById("address").value.trim();
  const phone = document.getElementById("phone").value.trim();

  if (!address || !phone) {
    alert("يرجى إدخال العنوان ورقم الهاتف.");
    return;
  }

  let message = `طلب جديد:\n`;
  for (const id in quantities) {
    const qty = parseInt(quantities[id]);
    if (qty > 0) {
      const product = products.find(p => p.id == id);
      message += `- ${product.name} × ${qty} = ${qty * product.price} دج\n`;
    }
  }

  message += `\nالمجموع: ${document.getElementById("totalPrice").textContent.replace("المبلغ الإجمالي: ", "")}`;
  message += `\nالعنوان: ${address}\nرقم الهاتف: ${phone}`;
  if (note) message += `\nملاحظة: ${note}`;

  // عرض تأكيد لتفعيل GPS
  if (confirm("هل تريد إضافة موقعك الجغرافي للطلب؟")) {
    navigator.geolocation.getCurrentPosition(
      position => {
        const lat = position.coords.latitude;
        const lon = position.coords.longitude;
        const mapsLink = `https://www.google.com/maps?q=${lat},${lon}`;
        message += `\nالموقع: ${mapsLink}`;
        const encodedMsg = encodeURIComponent(message);
        window.open(`https://wa.me/${phoneNumber}?text=${encodedMsg}`, "_blank");
      },
      error => {
        alert("تعذر الحصول على الموقع. سيتم إرسال الطلب بدون الموقع.");
        const encodedMsg = encodeURIComponent(message);
        window.open(`https://wa.me/${phoneNumber}?text=${encodedMsg}`, "_blank");
      }
    );
  } else {
    const encodedMsg = encodeURIComponent(message);
    window.open(`https://wa.me/${phoneNumber}?text=${encodedMsg}`, "_blank");
  }
}

update();
