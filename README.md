# QueueLess-Digital-Rationing-for-a-smaller-community

## 1. Introduction :

QueueLess is a digital rationing and queue management system designed to address the challenges faced by smaller communities during ration distribution. Traditional ration shops often suffer from long waiting times, overcrowding, lack of communication, and inefficient manual queue handling. QueueLess introduces a technology-driven approach that enables citizens to book time slots digitally, thereby reducing congestion and ensuring a smooth, transparent, and organized ration distribution process. The system aligns with the objectives of Digital India by promoting accessibility, efficiency, and fairness in public distribution services.

## 2. Problem Statement :

1. In smaller communities, ration shops rely heavily on manual queue management
2. Leading to overcrowding
3. Time wastage
4. Miscommunication
5. Inconvenience for beneficiaries

## 3. Purpose of the Project :

The purpose of QueueLess is to provide a digital platform that allows ration beneficiaries to book slots in advance, minimizing queues and improving service efficiency in small community ration shops.

## 4. Proposed Solution

QueueLess is a web-based digital rationing system that enables users to:

1. Register using basic credentials
2. Log in securely
3. Select preferred date and time slots
4. Receive confirmation for ration collection
The ration shop administrator can manage bookings, monitor daily schedules, and control crowd flow effectively.

## 6. System Components :
### User Module
1. User registration and login
2. Slot selection and booking
3. Booking confirmation display

### Admin Module
1. View booked slots
2. Monitor daily ration distribution
3. Prevent overbooking

### Database Module

1. Stores user details
2. Maintains slot availability
3. Records booking history

## Program :
```
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Ration Slot Booking</title>

<style>
/* ---------- GLOBAL ---------- */
body {
  font-family: Arial, sans-serif;
  background: linear-gradient(to bottom, #f6f1ea, #d8c3a5);
  margin: 0;
  padding: 0;
  min-height: 100vh;
}

h1, h2, h3 {
  text-align: center;
  color: #8b5a2b;
}

.container {
  max-width: 420px;
  margin: auto;
  padding: 20px;
  position: relative;
  z-index: 1;
}

/* ---------- OVERLAY & MODAL ---------- */
.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  z-index: 1000;
  display: none;
}

.modal {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: white;
  padding: 30px;
  border-radius: 15px;
  z-index: 1001;
  max-width: 400px;
  width: 90%;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
}

/* ---------- BUTTONS ---------- */
button {
  width: 100%;
  padding: 15px;
  margin-top: 10px;
  font-size: 18px;
  border: none;
  border-radius: 8px;
  background: #8b5a2b;
  color: white;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

button:focus {
  outline: 3px solid #6f4518;
  outline-offset: 2px;
}

button:hover:not(:disabled) {
  background: #6f4518;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
  transform: translateY(-1px);
}

button:disabled {
  background: #ccc;
  cursor: not-allowed;
  transform: none;
}

.secondary {
  background: #d8c3a5;
  color: #000;
}

.secondary:hover {
  background: #c0ad8a;
}

.secondary:focus {
  outline: 3px solid #6f4518;
  outline-offset: 2px;
}

/* ---------- INPUTS ---------- */
label {
  display: block;
  margin-top: 10px;
  font-weight: bold;
  color: #8b5a2b;
}

input {
  width: 100%;
  padding: 14px;
  font-size: 18px;
  margin-top: 5px;
  border-radius: 8px;
  border: 1px solid #ccc;
  box-sizing: border-box;
  transition: border-color 0.3s ease;
  background: white;
}

input:focus {
  border-color: #8b5a2b;
  outline: 3px solid #6f4518;
  outline-offset: 2px;
}

input[type="date"] {
  padding: 12px;
  font-size: 16px;
}

/* ---------- TOGGLE ---------- */
.toggle {
  display: flex;
  gap: 10px;
}

.toggle button {
  flex: 1;
  margin-top: 0;
}

.active {
  background: #6f4518;
}

.toggle button[aria-selected="true"] {
  background: #6f4518;
}

/* ---------- HIDDEN ---------- */
.hidden {
  display: none;
}

/* ---------- CARD ---------- */
.card {
  background: white;
  padding: 20px;
  border-radius: 10px;
  margin-top: 15px;
  font-size: 16px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  transition: box-shadow 0.3s ease;
}

.card:hover {
  box-shadow: 0 6px 16px rgba(0, 0, 0, 0.15);
}

/* ---------- SLOTS ---------- */
.slot {
  width: 100%;
  padding: 12px;
  border: 1px solid #8b5a2b;
  border-radius: 8px;
  margin: 6px 0;
  cursor: pointer;
  background: white;
  transition: all 0.3s ease;
  font-weight: bold;
  font-size: 16px;
  color: #333;
}

.slot:focus {
  outline: 3px solid #6f4518;
  outline-offset: 2px;
}

.slot:hover:not(.full) {
  background: #f0e0c6;
  transform: translateY(-1px);
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
}

.selected {
  background: #8b5a2b !important;
  color: white;
  border-color: #6f4518;
}

.full {
  background: #ddd;
  cursor: not-allowed;
  color: #999;
}

.full:hover,
.full:focus {
  background: #ddd;
  transform: none;
  box-shadow: none;
}

/* ---------- CAPTCHA ---------- */
.captcha {
  font-size: 22px;
  letter-spacing: 3px;
  background: #eee;
  text-align: center;
  padding: 12px;
  margin-top: 10px;
  border-radius: 8px;
  font-weight: bold;
  color: #8b5a2b;
}

/* ---------- REDUCED MOTION ---------- */
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
</style>
</head>

<body>

<div id="overlay" class="overlay"></div>

<!-- ========== LOGIN PAGE (MODAL) ========== -->
<!-- ========== LOGIN PAGE (MODAL) ========== -->
<div id="loginPage" class="hidden modal">
  <h1 data-translate="title">🛒 Ration Slot Booking</h1>

  <div class="toggle" role="tablist">
    <button id="userBtn" class="active" role="tab" aria-selected="true" onclick="setRole('user')" data-translate="userLogin">User Login</button>
    <button id="adminBtn" role="tab" aria-selected="false" onclick="setRole('admin')" data-translate="adminLogin">Admin Login</button>
  </div>

  <label id="loginLabel" data-translate="rationCardNumber">Ration Card Number</label>
  <input id="loginInput" aria-labelledby="loginLabel" placeholder="Enter Ration Card Number">

  <label for="nameInput">Name</label>  <!-- NEW: Name label -->
  <input id="nameInput" placeholder="Enter Name" type="text">  <!-- NEW: Name input -->

  <div class="captcha" id="captchaText" role="img" aria-label="Captcha image"></div>
  <label for="captchaInput" data-translate="enterCaptcha">Enter Captcha</label>
  <input id="captchaInput" aria-labelledby="captchaText">

  <button onclick="sendOTP()" data-translate="loginOtp">Login with OTP</button>
</div>

<div class="container">

<!-- ========== OTP PAGE ========== -->
<div id="otpPage" class="hidden">
  <h2 data-translate="enterOtp">Enter OTP</h2>
  <p style="text-align:center;" data-translate="demoOtp">(Demo OTP shown below)</p>
  <h3 id="otpDisplay" role="status" aria-live="polite"></h3>
  <label for="otpInput" data-translate="enterOtp">Enter OTP</label>
  <input id="otpInput">
  <button onclick="verifyOTP()" data-translate="verify">Verify</button>
</div>

<!-- ========== LANGUAGE PAGE ========== -->
<div id="languagePage" class="hidden">
  <h2 data-translate="selectLanguage">Select Language</h2>
  <button class="secondary" onclick="setLanguage('en')" data-translate="english">English</button>
  <button class="secondary" onclick="setLanguage('hi')">हिंदी</button>
  <button class="secondary" onclick="setLanguage('ta')">தமிழ்</button>
  <button onclick="showDashboard()" data-translate="continue">Continue</button>
</div>

<!-- ========== USER DASHBOARD ========== -->
<div id="dashboardPage" class="hidden">
  <h2 data-translate="yourRationCard">Your Ration Card</h2>

  <div class="card" id="userCard">
    <b data-translate="name">Name:</b> Yashoda<br>
    <b data-translate="rationCard">Ration Card:</b> 055000014578<br>
    <b data-translate="fps">FPS:</b> Sanjay Prasad
  </div>

  <h3 data-translate="selectDate">Select Date</h3>
  <label for="datePicker" data-translate="chooseDate">Choose a date for booking</label>
  <input type="date" id="datePicker" min="2026-01-07" max="2026-03-07" onchange="setDate(this.value)">
  <button id="dateBtn" onclick="openSlots()" disabled data-translate="selectTimeSlots">Select Time Slots</button>

  <button onclick="showPage('loginPage')" class="secondary" style="margin-top: 20px;" data-translate="logout">Logout</button>
</div>

<!-- ========== SLOT PAGE ========== -->
<div id="slotPage" class="hidden">
  <h2 data-translate="selectTimeSlot">Select Time Slot</h2>
  <div id="slots" role="radiogroup" aria-label="Available time slots"></div>
  <button id="confirmBtn" class="hidden" onclick="bookSlot()" data-translate="bookSelectedSlot">Book Selected Slot</button>
</div>

<!-- ========== CONFIRM PAGE ========== -->
<div id="confirmPage" class="hidden">
  <h2 data-translate="slotBooked">✅ Slot Booked</h2>
  <p id="confirmDetails" style="text-align:center;" role="status" aria-live="polite">
    <span data-translate="thankYou">Thank you!</span><br><br>
    <span data-translate="slotBookedSuccessfully">Your ration slot has been booked successfully.</span>
  </p>
  <button onclick="showDashboard()" class="secondary" style="margin-top: 20px;" data-translate="bookAnotherSlot">Book Another Slot</button>
</div>

<!-- ========== ADMIN DASHBOARD ========== -->
<div id="adminPage" class="hidden">
  <h2 data-translate="adminDashboard">Admin Dashboard</h2>

  <div class="card">
    <b data-translate="date">Date:</b> 10 Jan 2026<br>
    <b data-translate="totalSlots">Total Slots:</b> 20<br>
    <b data-translate="totalBookings">Total Bookings:</b> 42
  </div>

  <button onclick="showPage('loginPage')" class="secondary" style="margin-top: 20px;" data-translate="logout">Logout</button>
</div>

</div>

<script>
/* ---------- TRANSLATIONS ---------- */
const translations = {
  en: {
    title: "🛒 Ration Slot Booking",
    userLogin: "User Login",
    adminLogin: "Admin Login",
    rationCardNumber: "Ration Card Number",
    enterRationCardNumber: "Enter Ration Card Number",
    mobileNumber: "Mobile Number",
    enterMobileNumber: "Enter Mobile Number",
    enterCaptcha: "Enter Captcha",
    loginOtp: "Login with OTP",
    enterOtp: "Enter OTP",
    demoOtp: "(Demo OTP shown below)",
    verify: "Verify",
    selectLanguage: "Select Language",
    english: "English",
    continue: "Continue",
    yourRationCard: "Your Ration Card",
    name: "Name:",
    rationCard: "Ration Card:",
    fps: "FPS:",
    selectDate: "Select Date",
    chooseDate: "Choose a date for booking",
    selectTimeSlots: "Select Time Slots",
    selectTimeSlot: "Select Time Slot",
    available: "(Available)",
    booked: "(Booked)",
    bookSelectedSlot: "Book Selected Slot",
    slotBooked: "✅ Slot Booked",
    thankYou: "Thank you!",
    slotBookedSuccessfully: "Your ration slot for {time} on {date} has been booked successfully.",
    bookAnotherSlot: "Book Another Slot",
    adminDashboard: "Admin Dashboard",
    date: "Date:",
    totalSlots: "Total Slots:",
    totalBookings: "Total Bookings:",
    logout: "Logout"
  },
  hi: {
    title: "🛒 राशन स्लॉट बुकिंग",
    userLogin: "उपयोगकर्ता लॉगिन",
    adminLogin: "प्रशासक लॉगिन",
    rationCardNumber: "राशन कार्ड नंबर",
    enterRationCardNumber: "राशन कार्ड नंबर दर्ज करें",
    mobileNumber: "मोबाइल नंबर",
    enterMobileNumber: "मोबाइल नंबर दर्ज करें",
    enterCaptcha: "कैप्चा दर्ज करें",
    loginOtp: "OTP के साथ लॉगिन करें",
    enterOtp: "OTP दर्ज करें",
    demoOtp: "(डेमो OTP नीचे दिखाया गया)",
    verify: "सत्यापित करें",
    selectLanguage: "भाषा चुनें",
    english: "अंग्रेजी",
    continue: "जारी रखें",
    yourRationCard: "आपका राशन कार्ड",
    name: "नाम:",
    rationCard: "राशन कार्ड:",
    fps: "FPS:",
    selectDate: "तिथि चुनें",
    chooseDate: "बुकिंग के लिए तिथि चुनें",
    selectTimeSlots: "समय स्लॉट चुनें",
    selectTimeSlot: "समय स्लॉट चुनें",
    available: "(उपलब्ध)",
    booked: "(बुक किया गया)",
    bookSelectedSlot: "चयनित स्लॉट बुक करें",
    slotBooked: "✅ स्लॉट बुक किया गया",
    thankYou: "धन्यवाद!",
    slotBookedSuccessfully: "{time} पर {date} को आपका राशन स्लॉट सफलतापूर्वक बुक किया गया है।",
    bookAnotherSlot: "एक और स्लॉट बुक करें",
    adminDashboard: "प्रशासक डैशबोर्ड",
    date: "तिथि:",
    totalSlots: "कुल स्लॉट:",
    totalBookings: "कुल बुकिंग:",
    logout: "लॉगआउट"
  },
  ta: {
    title: "🛒 ரேஷன் ஸ்லாட் புக் செய்தல்",
    userLogin: "பயனர் உள்நுழைவு",
    adminLogin: "நிர்வாகி உள்நுழைவு",
    rationCardNumber: "ரேஷன் கார்டு எண்",
    enterRationCardNumber: "ரேஷன் கார்டு எண் உள்ளிடவும்",
    mobileNumber: "மொபைல் எண்",
    enterMobileNumber: "மொபைல் எண் உள்ளிடவும்",
    enterCaptcha: "கேப்சா உள்ளிடவும்",
    loginOtp: "OTP உடன் உள்நுழையவும்",
    enterOtp: "OTP உள்ளிடவும்",
    demoOtp: "(டெமோ OTP கீழே காட்டப்பட்டுள்ளது)",
    verify: "சரிபார்க்கவும்",
    selectLanguage: "மொழி தேர்ந்தெடுக்கவும்",
    english: "ஆங்கிலம்",
    continue: "தொடரவும்",
    yourRationCard: "உங்கள் ரேஷன் கார்டு",
    name: "பெயர்:",
    rationCard: "ரேஷன் கார்டு:",
    fps: "FPS:",
    selectDate: "தேதி தேர்ந்தெடுக்கவும்",
    chooseDate: "புக் செய்வதற்கான தேதியை தேர்ந்தெடுக்கவும்",
    selectTimeSlots: "நேர ஸ்லாட்களை தேர்ந்தெடுக்கவும்",
    selectTimeSlot: "நேர ஸ்லாட் தேர்ந்தெடுக்கவும்",
    available: "(கிடைக்கும்)",
    booked: "(புக் செய்யப்பட்டது)",
    bookSelectedSlot: "தேர்ந்தெடுக்கப்பட்ட ஸ்லாட்டை புக் செய்யவும்",
    slotBooked: "✅ ஸ்லாட் புக் செய்யப்பட்டது",
    thankYou: "நன்றி!",
    slotBookedSuccessfully: "{date} இல் {time} க்கான உங்கள் ரேஷன் ஸ்லாட் வெற்றிகரமாக புக் செய்யப்பட்டது.",
    bookAnotherSlot: "மற்றொரு ஸ்லாட்டை புக் செய்யவும்",
    adminDashboard: "நிர்வாகி டாஷ்போர்டு",
    date: "தேதி:",
    totalSlots: "மொத்த ஸ்லாட்கள்:",
    totalBookings: "மொத்த புக் செய்தல்:",
    logout: "வெளியேறு"
  }
};

/* ---------- GLOBAL STATE ---------- */
let role = "user";
let generatedOTP = "";
let captcha = "";
let selectedTime = "";
let selectedDate = "";
let selectedLanguage = "en";
let userId = null; // From DB after login
let userData = { name: 'Yashoda', rationCard: '055000014578', fps: 'Sanjay Prasad' }; // Demo; fetch from DB in real

/* ---------- INIT ---------- */
generateCaptcha();
showPage("loginPage");
updateTranslations();

/* ---------- TRANSLATION FUNCTIONS ---------- */
function updateTranslations() {
  document.documentElement.lang = selectedLanguage;
  document.querySelectorAll('[data-translate]').forEach(el => {
    const key = el.getAttribute('data-translate');
    const translation = translations[selectedLanguage][key] || el.textContent;
    if (el.tagName === 'INPUT') {
      el.placeholder = translation;
    } else {
      el.textContent = translation;
    }
  });
  // Update placeholders
  const loginInput = document.getElementById('loginInput');
  loginInput.placeholder = role === 'user' ? translations[selectedLanguage].enterRationCardNumber : translations[selectedLanguage].enterMobileNumber;
}

function setLanguage(lang) {
  selectedLanguage = lang;
  updateTranslations();
  // Highlight selected button
  document.querySelectorAll('#languagePage .secondary').forEach(btn => btn.classList.remove('active'));
  event.target.classList.add('active');
}

/* ---------- FUNCTIONS ---------- */
function setRole(r) {
  role = r;
  const userBtn = document.getElementById("userBtn");
  const adminBtn = document.getElementById("adminBtn");
  userBtn.classList.remove("active");
  adminBtn.classList.remove("active");
  userBtn.setAttribute("aria-selected", "false");
  adminBtn.setAttribute("aria-selected", "false");

  const loginLabel = document.getElementById("loginLabel");
  const loginInput = document.getElementById("loginInput");

  if (r === "user") {
    userBtn.classList.add("active");
    userBtn.setAttribute("aria-selected", "true");
    loginLabel.textContent = translations[selectedLanguage].rationCardNumber;
    loginInput.placeholder = translations[selectedLanguage].enterRationCardNumber;
  } else {
    adminBtn.classList.add("active");
    adminBtn.setAttribute("aria-selected", "true");
    loginLabel.textContent = translations[selectedLanguage].mobileNumber;
    loginInput.placeholder = translations[selectedLanguage].enterMobileNumber;
  }
}

function generateCaptcha() {
  captcha = Math.random().toString(36).substring(2, 7).toUpperCase();
  const captchaEl = document.getElementById("captchaText");
  captchaEl.innerText = captcha;
  captchaEl.setAttribute("aria-label", `Captcha: ${captcha}`);
}

function sendOTP() {
  if (document.getElementById("captchaInput").value !== captcha) {
    alert("Captcha incorrect");
    generateCaptcha();
    document.getElementById("captchaInput").focus();
    return;
  }

  generatedOTP = Math.floor(100000 + Math.random() * 900000).toString();
  document.getElementById("otpDisplay").innerText = generatedOTP;

  showPage("otpPage");
  document.getElementById("otpInput").focus();
}

async function verifyOTP() {
  if (document.getElementById("otpInput").value !== generatedOTP) {
    alert("Invalid OTP");
    document.getElementById("otpInput").focus();
    return;
  }

  if (role === "admin") {
    showPage("adminPage");
    return;
  }

  const rationCard = document.getElementById("loginInput").value;
  const name = document.getElementById("nameInput").value.trim() || 'Yashoda';  // NEW: Get from input, fallback to default

  if (!rationCard) {
    alert('Please enter a ration card number');
    return;
  }

  // NEW: Fill userData with entered values
  userData = { name, rationCard, fps: 'Sanjay Prasad' };  // FPS hardcoded for demo

  const formData = new FormData();
  formData.append('ration_card', rationCard);
  formData.append('name', name);  // NEW: Send entered name
  formData.append('mobile', '');
  formData.append('fps', userData.fps);

  try {
    const response = await fetch('login.php', {
      method: 'POST',
      body: formData
    });

    const text = await response.text();
    let result = JSON.parse(text);

    if (result.success) {
      userId = result.user_id;
      sessionStorage.setItem('userId', userId);
      // NEW: Use entered name in card
      document.getElementById('userCard').innerHTML = `
        <b data-translate="name">Name:</b> ${name}<br>
        <b data-translate="rationCard">Ration Card:</b> ${rationCard}<br>
        <b data-translate="fps">FPS:</b> ${userData.fps}
      `;
      showPage("languagePage");
    } else {
      alert(result.message || 'Login failed');
    }
  } catch (error) {
    alert('Login error: ' + error.message);
  }
}

function showDashboard() {
  if (!userId) userId = sessionStorage.getItem('userId');
  showPage("dashboardPage");
  document.getElementById("datePicker").value = "";
  document.getElementById("dateBtn").disabled = true;
  selectedDate = "";
  document.getElementById("datePicker").focus();
}

function setDate(val) {
  selectedDate = val;
  document.getElementById("dateBtn").disabled = !val;
}

function openSlots() {
  if (!selectedDate) {
    alert("Please select a date first.");
    document.getElementById("datePicker").focus();
    return;
  }
  showPage("slotPage");
  generateSlots();
}

function generateSlots() {
  const slotsDiv = document.getElementById("slots");
  slotsDiv.innerHTML = "";
  document.getElementById("confirmBtn").classList.add("hidden");

  // Fetch bookings from backend (add get_bookings.php for real; simulate here for demo)
  const dateBookings = {}; // Replace with fetch('get_bookings.php?date=' + selectedDate)

  let startHour = 8;
  let endHour = 16;

  for (let h = startHour; h < endHour; h++) {
    if (h === 13) continue; // lunch break

    for (let m = 0; m < 60; m += 15) {
      let time = `${h}:${m.toString().padStart(2, "0")}`;
      let slot = document.createElement("button");
      slot.className = "slot";
      slot.type = "button";
      const isBooked = !!dateBookings[time];
      const statusText = isBooked ? translations[selectedLanguage].booked : translations[selectedLanguage].available;
      slot.innerText = time + " " + statusText;
      slot.setAttribute("role", "radio");
      slot.setAttribute("aria-checked", "false");
      if (isBooked) {
        slot.classList.add("full");
        slot.disabled = true;
      } else {
        slot.addEventListener("click", () => {
          document.querySelectorAll('.slot:not(.full)').forEach(s => {
            s.classList.remove('selected');
            s.setAttribute("aria-checked", "false");
          });
          slot.classList.add('selected');
          slot.setAttribute("aria-checked", "true");
          selectedTime = time;
          document.getElementById("confirmBtn").classList.remove('hidden');
        });
      }

      slotsDiv.appendChild(slot);
    }
  }
}

async function bookSlot() {
  if (!selectedTime || !userId) {
    alert('No slot selected or user not logged in (userId: ' + userId + ', time: ' + selectedTime + ')');
    return;
  }

  const formData = new FormData();
  formData.append('user_id', userId);
  formData.append('date', selectedDate);
  formData.append('time_slot', selectedTime);

  console.log('Booking data:', { userId, selectedDate, selectedTime });  // Log to console

  try {
    console.log('Fetching book_slot.php...');
    const response = await fetch('book_slot.php', {
      method: 'POST',
      body: formData
    });

    console.log('Response status:', response.status);
    const text = await response.text();
    console.log('Raw response text:', text);

    let result;
    try {
      result = JSON.parse(text);
      console.log('Parsed result:', result);
    } catch (parseErr) {
      console.error('JSON parse failed:', parseErr);
      alert('Booking response is not JSON. Raw: ' + (text.substring(0, 200) || 'Empty'));
      return;
    }

    // Fallback if result or message is undefined
    const message = result.message || (result.success ? 'Slot booked successfully' : 'Booking failed (unknown reason)');
    if (result.success) {
      generateSlots();  // Refresh slots
      showPage("confirmPage");
      alert(message);  // Show success message
    } else {
      alert(message);
    }
  } catch (error) {
    console.error('Full booking error:', error);
    alert('Booking error: ' + (error.message || 'Unknown error'));
  }
}

function showPage(pageId) {
  // Hide all pages
  document.getElementById("loginPage").style.display = "none";
  [
    "otpPage",
    "languagePage",
    "dashboardPage",
    "slotPage",
    "confirmPage",
    "adminPage"
  ].forEach(id => document.getElementById(id).classList.add("hidden"));

  document.getElementById("overlay").style.display = "none";

  if (pageId === "loginPage") {
    document.getElementById("overlay").style.display = "block";
    document.getElementById("loginPage").style.display = "block";
    document.getElementById("loginInput").focus();
  } else {
    document.getElementById(pageId).classList.remove("hidden");
    if (pageId === "confirmPage") {
      const dateObj = new Date(selectedDate);
      const formattedDate = dateObj.toLocaleDateString(selectedLanguage === 'hi' ? 'hi-IN' : selectedLanguage === 'ta' ? 'ta-IN' : 'en-GB', { day: 'numeric', month: 'short', year: 'numeric' });
      const successMsg = translations[selectedLanguage].slotBookedSuccessfully
        .replace('{time}', `<b>${selectedTime}</b>`)
        .replace('{date}', formattedDate);
      document.getElementById("confirmDetails").innerHTML = `<span data-translate="thankYou">${translations[selectedLanguage].thankYou}</span><br><br>${successMsg}`;
    }
    // Focus first interactive element
    const firstInput = document.querySelector(`#${pageId} input, #${pageId} button:not([disabled])`);
    if (firstInput) firstInput.focus();
  }
}
</script>

</body>
</html>

```

## Output :

<img width="1600" height="900" alt="image" src="https://github.com/user-attachments/assets/a3c497d1-c055-42d3-a94e-ba0acddbf19c" />

<img width="1600" height="900" alt="image" src="https://github.com/user-attachments/assets/c22a702b-20cc-4fbb-885a-6453c8b85bcf" />

<img width="1600" height="900" alt="image" src="https://github.com/user-attachments/assets/be02bf69-69e8-435d-b4a7-a89b7ac422d9" />

<img width="1600" height="900" alt="image" src="https://github.com/user-attachments/assets/0e98b08d-05b2-434a-8ba1-3a2418dd96da" />

<img width="1600" height="900" alt="image" src="https://github.com/user-attachments/assets/d2fac2ad-d910-4aa8-85f6-d96d2428310e" />

<img width="1600" height="900" alt="image" src="https://github.com/user-attachments/assets/f3de45d1-306c-4a03-a728-c746e19a4d60" />

## 7. Advantages of QueueLess :

1. Reduces overcrowding at ration shops
2. Saves time for beneficiaries and shopkeepers
3. Ensures fair and transparent ration distribution
4. Improves communication between users and ration shops
5. Easy to use, even for smaller communities

## Result :

QueueLess provides an innovative digital solution to traditional ration shop challenges by replacing manual queue systems with a smart slot-based booking mechanism. The system enhances efficiency, reduces crowding, and ensures a smoother ration distribution experience, making it ideal for smaller communities.
