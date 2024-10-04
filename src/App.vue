<template>
  <div class="flyer">
    <div class="palm-leaf left"></div>
    <div class="palm-leaf right"></div>
    <div class="content">
      <div class="header">
        <h3>Book Tickets</h3>
        <span class="fire-text">OMOKU RAVE PARTY</span>
        <h2>Beach Edition</h2>
      </div>
      <div class="main-content">
        <div class="image-container">
          <img
            src="./assets/event.jpeg"
            alt="Event image"
            class="event-image"
          />
        </div>
        <div class="event-details">
          <p class="date">1ST <span>NOV .</span> 2024</p>
          <!-- <p class="djs fire-text">OMOKU RAVE PARTY</p> -->
          <p class="location">
            H H HOTELS & APARTMENTS OMUKU ONELGA RIVERS STATE
          </p>
          <p class="info">POOL PARTY 2 PM - 6 PM</p>
          <div class="ticket-types">
            <h3>Ticket Types</h3>
            <ul>
              <li>Early Bird : ₦2500</li>
              <li>Regular : ₦5000</li>
              <li>VVIP REDROOM : ₦30,000</li>
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
        <form @submit.prevent="bookTickets" v-if="formStep">
          <div class="form-group">
            <label for="name">Name</label>
            <input
              type="text"
              v-model="form.name"
              id="name"
              placeholder="Enter your name"
              @blur="validateField('name')"
            />
            <span v-if="errors.name" class="error">{{ errors.name }}</span>
          </div>

          <div class="form-group">
            <label for="email">Email</label>
            <input
              type="email"
              v-model="form.email"
              id="email"
              placeholder="Enter your email"
              @blur="validateField('email')"
            />
            <span v-if="errors.email" class="error">{{ errors.email }}</span>
          </div>

          <!-- Ticket Type Section -->
          <div class="form-group">
            <label>Ticket Type</label>
            <select
              v-model="selectedTicketType"
              @change="validateField('ticketType')"
              class="form-select"
            >
              <option value="" disabled>Select Ticket Type</option>
              <option value="early bird">EARLY BIRD</option>
              <option value="Regular">REGULAR</option>
              <option value="VVIP">VVIP REDROOM</option>
            </select>
            <span v-if="errors.ticketType" class="error">{{
              errors.ticketType
            }}</span>
          </div>

          <button type="submit" class="submit-btn mt-3" @click="submitForm">
            Confirm Booking
          </button>
        </form>

        <!-- Account Number Section -->
        <div v-if="accountStep" class="account-number">
          <div class="payment-container">
            <div class="pay-header">
              <div class="logo">≡</div>
              <div class="email">{{ form.email }}</div>
            </div>
            <div class="amount text-center">Pay NGN {{ amount }}</div>
            <div class="transfer-info">
              <p class="email">
                Click on the button below to share payment evidence
              </p>

              <div class="bank-info">
                <h3>OPay</h3>
                <div class="account-number">
                  {{ accountNumber }}

                  <span class="copy-icon" @click="copyAccountNumber">📋</span>
                </div>
              </div>
              <div class="expires">Ordu Ejerulor Dickson</div>
            </div>
            <button class="confirm-button" @click="openWhatsAppLink">
              I've sent the money
            </button>
          </div>
        </div>

        <!-- <button class="close-btn" @click="closeModal">Close</button> -->
      </div>
    </div>
  </div>
</template>

<script setup>
/*eslint-disable*/
import { ref, computed } from "vue";

// Computed property to check if the form is valid
const isFormValid = computed(() => {
  return (
    form.value.name !== "" &&
    form.value.email !== "" &&
    selectedTicketType.value !== "" &&
    errors.value.name === "" &&
    errors.value.email === "" &&
    errors.value.ticketType === ""
  );
});

const form = ref({
  name: "",
  email: "",
});

const email = ref("someone@somail.com");
const amount = ref(null);
// const bankName = ref("Zenith Bank");
const accountNumber = ref("9166829361");

const copyAccountNumber = () => {
  navigator.clipboard.writeText(accountNumber.value);
  alert("Account number copied to clipboard!");
};

const confirmTransfer = () => {
  alert("Transfer confirmed!");
};

const selectedTicketType = ref("Regular");
const phoneNumber = "+2349166829361"; // Your WhatsApp number in international format
const preFilledMessage = `Payments for ${selectedTicketType.value} ticket`; // Pre-filled message

const formStep = ref(true);
const accountStep = ref(false);

// Ref for modal visibility state
const showBookingModal = ref(false);
const errors = ref({
  name: "",
  email: "",
  ticketType: "",
  paymentType: "",
});

// Close the modal
const closeModal = () => {
  showBookingModal.value = false;
};

// Validation function for each field
const validateField = (field) => {
  if (field === "name") {
    errors.value.name = form.value.name === "" ? "Name is required." : "";
  }

  if (field === "email") {
    const emailPattern = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,4}$/;
    if (form.value.email === "") {
      errors.value.email = "Email is required.";
    } else if (!emailPattern.test(form.value.email)) {
      errors.value.email = "Email is invalid.";
    } else {
      errors.value.email = "";
    }
  }

  if (field === "ticketType") {
    errors.value.ticketType =
      selectedTicketType.value === "" ? "Please select a ticket type." : "";
  }

  // if (field === "paymentType") {
  //   errors.value.paymentType =
  //     selectedPaymentType.value === "" ? "Please select a payment type." : "";
  // }
};

const submitForm = () => {
  // Validate all fields before submitting
  validateField("name");
  validateField("email");
  validateField("ticketType");
  // validateField("paymentType");

  if (isFormValid.value) {
    console.log(selectedTicketType.value);
    switch (selectedTicketType.value) {
      case "early bird":
        amount.value = 2500;
        break;
      case "Regular":
        amount.value = 5000;
        break;
      case "VVIP":
        amount.value = 30000;
        break;
    }
    formStep.value = false;
    accountStep.value = true;

    // You can add actual form submission logic here
  } else {
    console.log("Form is incomplete.");
    formStep.value = true;
    accountStep.value = false;
  }
};

// Function to open the WhatsApp link
const openWhatsAppLink = () => {
  const whatsappLink = `https://wa.me/${phoneNumber}?text=${encodeURIComponent(
    preFilledMessage
  )}`;
  window.open(whatsappLink, "_blank"); // Opens WhatsApp link in a new tab/window
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@400;700&display=swap");

.flyer {
  background-color: rgba(0, 0, 0, 0.6);
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

.header span {
  font-size: 80px;
  font-weight: bold;
  margin: 0;
  text-transform: uppercase;
  letter-spacing: 10px;
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

.account-number h3 {
  margin-bottom: 10px;
}

.account-number p {
  font-size: 18px;
  font-weight: bold;
}

.error {
  color: red;
  font-size: 12px;
  margin-top: 5px;
}

/* .payment-container {
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  max-width: 100%;
  margin: 20px auto;
  padding: 25px;
  border: 1px solid #e0e0e0;
  border-radius: 12px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  background-color: #ffffff;
} */

.pay-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 25px;
}

.logo {
  font-size: 28px;
  color: #4a90e2;
  font-weight: bold;
}

.email {
  font-size: 14px;
  color: #757575;
}

.amount {
  font-size: 22px;
  font-weight: 600;
  margin-bottom: 25px;
  color: #333;
}

.transfer-info {
  text-align: center;
  margin-bottom: 25px;
}

.transfer-info p {
  font-size: 16px;
  color: #555;
  margin-bottom: 15px;
}

.bank-info {
  background-color: #f7f9fc;
  padding: 15px;
  border-radius: 8px;
  box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.1);
}

.bank-info h3 {
  font-size: 18px;
  color: #333;
  margin-bottom: 10px;
}

.account-number {
  font-size: 22px;
  font-weight: bold;
  color: #4a90e2;
  display: flex;
  justify-content: center;
  align-items: center;
}

.copy-icon {
  cursor: pointer;
  margin-left: 10px;
  font-size: 18px;
  color: #4a90e2;
  transition: color 0.3s ease;
}

.copy-icon:hover {
  color: #3a7bc8;
}

.expires {
  font-size: 14px;
  color: #888;
  margin-top: 15px;
}

.confirm-button {
  width: 100%;
  padding: 12px;
  background-color: #00e600;
  color: #111;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 16px;
  font-weight: 600;
  transition: background-color 0.3s ease;
}

.confirm-button:hover {
  background-color: #00e600;
}

.fire-text {
  /* height: 300px; */
  width: 100%;
  background-image: url("https://dl.dropbox.com/s/r2s8s2r17wi0xm6/flame.png?dl=0");
  background-position: 0 -1000px;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  animation: fire 4s linear infinite;
}

/* h1 {
  color: #fff;
  font-size: 80px;
  text-align: center;
} */

@keyframes fire {
  0% {
    background-position: 0% -50%;
  }
  100% {
    background-position: 0% -25%;
  }
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