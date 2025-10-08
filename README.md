# Transport-Booking
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Transport Booking</title>
<style>
  body {
    font-family: Arial, sans-serif;
    background: #f4f4f9;
    margin: 0;
    padding: 0;
  }
  header {
    background: #4CAF50;
    color: white;
    padding: 15px;
    text-align: center;
  }
  .container {
    width: 90%;
    max-width: 700px;
    margin: 20px auto;
    background: white;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0,0,0,0.1);
  }
  h2 { text-align: center; color: #333; }
  form { display: flex; flex-direction: column; }
  label { margin: 8px 0 4px; }
  input { padding: 10px; margin-bottom: 12px; border: 1px solid #ccc; border-radius: 6px; }
  button { padding: 10px; background: #4CAF50; color: white; border: none; border-radius: 6px; cursor: pointer; margin-top: 5px; }
  button:hover { background: #45a049; }
  .booking-list { margin-top: 20px; }
  .booking-item { background: #f9f9f9; border: 1px solid #ddd; margin: 8px 0; padding: 10px; border-radius: 6px; display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; }
  .delivered { text-decoration: line-through; color: gray; }
  .booking-buttons { display: flex; gap: 5px; flex-wrap: wrap; margin-top: 5px; }
</style>
</head>
<body>
<header>
  <h1>Transport Booking</h1>
</header>

<div class="container">
  <h2>Add Booking</h2>
  <form id="bookingForm">
    <label>Name:</label>
    <input type="text" id="name" required>
    
    <label>Phone:</label>
    <input type="text" id="phone" required>
    
    <label>Pickup Location:</label>
    <input type="text" id="pickup" required>
    
    <label>Dropoff Location:</label>
    <input type="text" id="dropoff" required>
    
    <label>Booking Date:</label>
    <input type="date" id="bookingDate" required>
    
    <button type="submit">Add Booking</button>
  </form>

  <div class="booking-list" id="bookingList">
    <h3>Bookings</h3>
  </div>
</div>

<script>
const bookingForm = document.getElementById("bookingForm");
const bookingList = document.getElementById("bookingList");

// Load bookings on page load
window.onload = () => {
  const bookings = JSON.parse(localStorage.getItem("bookings")) || [];
  bookings.forEach(b => displayBooking(b));
};

bookingForm.addEventListener("submit", (e) => {
  e.preventDefault();
  
  const booking = {
    id: Date.now(),
    name: document.getElementById("name").value,
    phone: document.getElementById("phone").value,
    pickup: document.getElementById("pickup").value,
    dropoff: document.getElementById("dropoff").value,
    bookingDate: document.getElementById("bookingDate").value,
    delivered: false
  };

  let bookings = JSON.parse(localStorage.getItem("bookings")) || [];
  bookings.push(booking);
  localStorage.setItem("bookings", JSON.stringify(bookings));

  displayBooking(booking);
  bookingForm.reset();
});

function displayBooking(booking) {
  const div = document.createElement("div");
  div.classList.add("booking-item");
  if (booking.delivered) div.classList.add("delivered");

  div.innerHTML = `
    <div>
      <strong>${booking.name}</strong> <br>
      Phone: ${booking.phone} <br>
      Pickup: ${booking.pickup} <br>
      Dropoff: ${booking.dropoff} <br>
      Date: ${booking.bookingDate}
    </div>
    <div class="booking-buttons">
      <button onclick="toggleDelivered(${booking.id})">${booking.delivered ? "Delivered" : "Mark as Delivered"}</button>
      <button onclick="deleteBooking(${booking.id})">Delete</button>
    </div>
  `;
  bookingList.appendChild(div);
}

function toggleDelivered(id) {
  let bookings = JSON.parse(localStorage.getItem("bookings")) || [];
  bookings = bookings.map(b => {
    if (b.id === id) b.delivered = !b.delivered;
    return b;
  });
  localStorage.setItem("bookings", JSON.stringify(bookings));
  refreshList();
}

function deleteBooking(id) {
  let bookings = JSON.parse(localStorage.getItem("bookings")) || [];
  bookings = bookings.filter(b => b.id !== id);
  localStorage.setItem("bookings", JSON.stringify(bookings));
  refreshList();
}

function refreshList() {
  bookingList.innerHTML = '<h3>Bookings</h3>';
  const bookings = JSON.parse(localStorage.getItem("bookings")) || [];
  bookings.forEach(b => displayBooking(b));
}
</script>
</body>
</html>
