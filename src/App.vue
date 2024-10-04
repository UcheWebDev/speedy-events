<template>
  <div class="flyer">
    <div class="palm-leaf left"></div>
    <div class="palm-leaf right"></div>
    <div class="content">
      <div class="header">
        <h3>Book Tickets</h3>
        <h1>Canada</h1>
        <h2>Socials & Events</h2>
      </div>
      <div class="main-content">
        <div class="image-container">
          <img src="./assets/event.jpeg" alt="Event image" class="event-image" />
        </div>
        <div class="event-details">
          <p class="date">SUNDAY <span>OCT 1ST</span></p>
          <p class="djs">DJ SNOOB x DJ KATTY</p>
          <p class="location">123 MAINSTREET TANGGULUN CITY - LOBSIDE PUB</p>
          <p class="info">DOORS OPEN AT 10:00 PM - FREE DRINK</p>
          <div class="ticket-types">
            <h3>Ticket Types</h3>
            <ul>
              <li>VIP: ₦5,000</li>
              <li>Regular: ₦2,000</li>
              <li>Early Bird: ₦1,500 (before Sep 15)</li>
            </ul>
          </div>
          <!-- Book Now Button -->
          <div class="button-container">
            <button class="book-now-btn" @click="showBookingModal = true">
              Book Now
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Booking Modal -->
    <div v-if="showBookingModal" class="modal-overlay" @click="closeModal">
      <div class="modal-content" @click.stop>
        <h2>Book Your Tickets</h2>
        <form @submit.prevent="bookTickets">
          <div class="form-group">
            <label for="full-name">Full Name</label>
            <input
              type="text"
              id="full-name"
              class="form-control"
              v-model="fullName"
              placeholder="Enter fullname"
              required
            />
          </div>
          <div class="form-group">
            <label for="ticket-type">Ticket Type</label>
            <select
              id="ticket-type"
              v-model="selectedTicketType"
              class="form-select"
            >
              <option value="VIP">VIP - ₦5,000</option>
              <option value="Regular">Regular - ₦2,000</option>
              <option value="Early Bird">
                Early Bird - ₦1,500 (before Sep 15)
              </option>
            </select>
          </div>

          <div class="form-group">
            <label for="email">Email</label>
            <input
              type="email"
              id="email"
              class="form-control"
              v-model="email"
              required
              placeholder="Enter email"
            />
          </div>

          <!-- Payment Type Section -->
          <div class="form-group">
            <label>Payment Type</label>
            <div class="payment-grid">
              <div>
                <input
                  type="radio"
                  id="credit-card"
                  value="Credit Card"
                  v-model="selectedPaymentType"
                />
                <label for="credit-card">Crypto</label>
              </div>

              <div>
                <input
                  type="radio"
                  id="bank-transfer"
                  value="Bank Transfer"
                  v-model="selectedPaymentType"
                />
                <label for="bank-transfer">Bank Transfer</label>
              </div>
              <div>
                <input
                  type="radio"
                  id="cash"
                  value="Cash"
                  v-model="selectedPaymentType"
                />
                <label for="cash">Cash</label>
              </div>
            </div>
          </div>

          <button type="submit" class="submit-btn">Confirm Booking</button>
        </form>
        <button class="close-btn" @click="closeModal">Close</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const selectedTicketType = ref("VIP");
const fullName = ref("");
const email = ref("");

// Ref for modal visibility state
const showBookingModal = ref(false);

// Close the modal
const closeModal = () => {
  showBookingModal.value = false;
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@400;700&display=swap");

.flyer {
  background: linear-gradient(135deg, #004d00, #006400);
  color: white;
  padding: 40px;
  font-family: "Poppins", sans-serif;
  position: relative;
  overflow: hidden;
  border-radius: 15px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
  min-height: 600px;
}

.palm-leaf {
  position: absolute;
  background: url("./assets/leaf.webp") no-repeat;
  background-size: contain;
  opacity: 0.1;
  z-index: 0;
}

.palm-leaf.left {
  top: -50px;
  left: -50px;
  width: 300px;
  height: 300px;
  transform: rotate(-45deg);
}

.palm-leaf.right {
  bottom: -50px;
  right: -50px;
  width: 250px;
  height: 250px;
  transform: rotate(135deg);
}

.content {
  position: relative;
  z-index: 1;
}

.header {
  text-align: center;
  margin-bottom: 35px;
}

.header h3 {
  font-size: 18px;
  text-transform: uppercase;
  letter-spacing: 3px;
  margin-bottom: 10px;
  color: #00ff00;
  animation: fadeInDown 1s ease-out;
}

.header h1 {
  font-size: 80px;
  font-weight: bold;
  margin: 0;
  text-transform: uppercase;
  letter-spacing: 10px;
  background: linear-gradient(to right, #00ff00, #ffffff);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  text-shadow: 3px 3px 6px rgba(0, 0, 0, 0.3);
  animation: scaleIn 1.5s ease-out;
}

.header h2 {
  font-size: 36px;
  margin-top: -5px;
  font-style: italic;
  color: #f0f0f0;
  animation: fadeInUp 1s ease-out;
}

.main-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.image-container {
  width: 45%;
  padding: 10px;
  background-color: rgba(255, 255, 255, 0.1);
  border-radius: 10px;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
  transform: rotate(-3deg);
  transition: transform 0.3s ease;
}

.image-container:hover {
  transform: rotate(0deg) scale(1.05);
}

.event-image {
  width: 100%;
  border-radius: 5px;
}

.event-details {
  width: 50%;
  background-color: rgba(0, 0, 0, 0.6);
  padding: 30px;
  border-radius: 10px;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
  /* margin-top: -200px; */
  margin-left: 80px;
}

.date {
  font-size: 28px;
  font-weight: bold;
  margin-bottom: 20px;
}

.date span {
  color: #00ff00;
}

.djs {
  font-size: 32px;
  font-weight: bold;
  margin: 20px 0;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
}

.location,
.info {
  font-size: 18px;
  line-height: 1.6;
  margin-bottom: 10px;
}

.ticket-types {
  margin-top: 30px;
  padding: 20px;
  background-color: rgba(0, 255, 0, 0.1);
  border-radius: 10px;
  text-align: left;
  color: #fff;
}

.ticket-types h3 {
  font-size: 24px;
  margin-bottom: 15px;
  color: #00ff00;
  text-transform: uppercase;
}

.ticket-types ul {
  list-style: none;
  padding: 0;
}

.ticket-types li {
  font-size: 18px;
  margin-bottom: 10px;
}

.button-container {
  margin-top: 30px;
  text-align: center;
}

.book-now-btn {
  background-color: #00ff00;
  color: #004d00;
  padding: 15px 30px;
  font-size: 20px;
  font-weight: bold;
  text-transform: uppercase;
  border: none;
  border-radius: 50px;
  box-shadow: 0 5px 15px rgba(0, 255, 0, 0.3);
  cursor: pointer;
  transition: all 0.3s ease;
}

.book-now-btn:hover {
  background-color: #00e600;
  box-shadow: 0 7px 20px rgba(0, 255, 0, 0.5);
  transform: translateY(-3px);
}

.book-now-btn:active {
  transform: translateY(0px);
  box-shadow: 0 5px 15px rgba(0, 255, 0, 0.3);
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.7);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal-content {
  background: white;
  padding: 30px;
  border-radius: 10px;
  width: 400px;
  max-width: 90%;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
  position: relative;
}

.modal-content h2 {
  margin-bottom: 20px;
  font-size: 24px;
  text-align: center;
}

.form-group {
  margin-bottom: 20px;
}

.form-group label {
  display: block;
  margin-bottom: 8px;
  font-weight: bold;
  color: #111;
}

.form-group input,
.form-group select {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.payment-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 15px;
}

.payment-grid div {
  display: flex;
  align-items: center;
}

.payment-grid input {
  margin-right: 10px;
}

.submit-btn {
  width: 100%;
  background-color: #00ff00;
  color: #004d00;
  padding: 12px;
  font-size: 18px;
  font-weight: bold;
  text-transform: uppercase;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.submit-btn:hover {
  background-color: #00e600;
}

.close-btn {
  position: absolute;
  top: 10px;
  right: 10px;
  background-color: transparent;
  border: none;
  font-size: 18px;
  cursor: pointer;
}

@keyframes fadeInDown {
  from {
    opacity: 0;
    transform: translateY(-20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes scaleIn {
  from {
    opacity: 0;
    transform: scale(0.8);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}

@media (max-width: 768px) {
  .flyer {
    padding: 20px;
  }

  .header h1 {
    font-size: 60px;
  }

  .main-content {
    flex-direction: column;
  }

  .image-container,
  .event-details {
    width: 100%;
    margin-bottom: 20px;
  }

  .event-details {
    /* width: 50%;
  background-color: rgba(0, 0, 0, 0.6);
  padding: 30px;
  border-radius: 10px;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3); */
    margin-top: 0px;
    margin-left: 0px;
  }
}
</style>