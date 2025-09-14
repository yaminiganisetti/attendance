<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Smart Attendance System — Fixed Register / Face Match</title>

  <script src="https://cdn.jsdelivr.net/npm/face-api.js@0.22.2/dist/face-api.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/qrcode@1.5.1/build/qrcode.min.js"></script>

  <style>
    body { font-family: "Georgia", serif; background: #f9f6f1; color: #2c2c2c; margin: 0; padding: 0; }
    .container { max-width: 960px; margin: 40px auto; background: #fffdf7; border-radius: 12px; padding: 40px; border: 2px solid #d8cbb3; box-shadow: 0 6px 20px rgba(0,0,0,0.15); }
    h2 { text-align: center; font-family: "Times New Roman", serif; font-size: 28px; margin-bottom: 20px; color: #3e2f2f; border-bottom: 2px solid #d8cbb3; padding-bottom: 10px; }
    input, button { width: 100%; padding: 14px; margin: 12px 0; border-radius: 6px; border: 1px solid #bfa98a; font-family: "Georgia", serif; }
    button { background: #5a3e2b; color: #fff; cursor: pointer; font-weight: bold; transition: 0.3s; }
    button:hover { background: #3e2f2f; }
    .grid-container { display: grid; grid-template-columns: 1fr 1fr; gap: 40px; margin-top: 30px; }
    .scanner-card { background: #fdfaf5; border-radius: 20px; padding: 40px; box-shadow: 0 6px 24px rgba(0,0,0,0.1); border: 2px solid #d8cbb3; text-align: center; }
    video, canvas { border-radius: 14px; width: 100%; max-height: 280px; margin-top: 15px; }
    table { width: 100%; border-collapse: collapse; margin-top: 20px; font-size: 15px; }
    th, td { border: 1px solid #bfa98a; padding: 10px; text-align: center; }
    th { background: #f2e8d5; color: #3e2f2f; }
    .success-message { color: #3e2f2f; font-weight: bold; display: none; text-align: center; background: #f2e8d5; padding: 10px; border-radius: 8px; border: 1px solid #d8cbb3; margin-top: 10px; }
    .hidden { display: none; }
    .small-note { font-size: 0.9rem; color: #5a4a3a; margin-top:6px; }
  </style>
</head>
<body>
  <div class="container">

    <div id="registerSection">
      <h2>Register Student</h2>
      <form id="registerForm">
        <input type="text" id="studentName" placeholder="Name" required>
        <input type="text" id="studentId" placeholder="Student ID" required>
        <input type="text" id="studentBranch" placeholder="Branch" required>
        <input type="text" id="studentSection" placeholder="Section" required>
        <input type="text" id="studentPhone" placeholder="Phone" required>
        <input type="file" id="studentPhoto" accept="image/*" required>
        <button type="submit">Register</button>
      </form>
      <p id="successMessage" class="success-message">Successfully Registered!</p>
      <p id="modelsStatus" class="small-note">Loading face models… (watch console for status)</p>
    </div>

    <div id="attendanceSection" class="hidden">
      <h2>Attendance Verification</h2>
      <div class="grid-container">
        <div class="scanner-card">
          <h3>QR Verification</h3>
          <p id="qrInfo">Your QR changes every 2 min</p>
          <canvas id="qrCanvas" style="margin-top:10px;"></canvas>
          <button id="qrBtn">Verify QR</button>
          <p id="qrResult"></p>
        </div>
        <div class="scanner-card">
          <h3>Face Recognition</h3>
          <button id="faceBtn">Start Face Scan</button>
          <div id="faceResult"></div>
          <video id="faceVideo" autoplay playsinline style="display:none;"></video>
          <canvas id="faceCanvas" style="display:none;"></canvas>
        </div>
      </div>
    </div>

    <div id="dashboardSection" class="hidden">
      <h2>Attendance Dashboard</h2>
      <div id="dailyAttendance"></div>
    </div>

  </div>

<script>
  const MODELS_URL = "https://justadudewhohacks.github.io/face-api.js/models";
  let modelsLoaded = false;
  let referenceDescriptor = null;
  let currentStudent = null;

  (async function loadModels(){
    try {
      await Promise.all([
        faceapi.nets.ssdMobilenetv1.loadFromUri(MODELS_URL),
        faceapi.nets.faceLandmark68Net.loadFromUri(MODELS_URL),
        faceapi.nets.faceRecognitionNet.loadFromUri(MODELS_URL)
      ]);
      modelsLoaded = true;
      document.getElementById('modelsStatus').textContent = "Face models loaded — you can register now.";
    } catch (err) {
      console.error("❌ Failed to load face-api models", err);
      document.getElementById('modelsStatus').textContent = "Failed to load face models.";
    }
  })();

  function saveStudentToStorage(studentObj, descriptorFloat32) {
    if (descriptorFloat32) studentObj.faceDescriptor = Array.from(descriptorFloat32);
    localStorage.setItem('currentStudent', JSON.stringify(studentObj));
  }

  (function restoreStudent(){
    const raw = localStorage.getItem('currentStudent');
    if (raw) {
      try {
        currentStudent = JSON.parse(raw);
        if (currentStudent && currentStudent.faceDescriptor) {
          referenceDescriptor = new Float32Array(currentStudent.faceDescriptor);
        }
      } catch(e) { console.warn("restoreStudent parse error:", e); }
    }
  })();

  const registerForm = document.getElementById('registerForm');
  const successMessage = document.getElementById('successMessage');

  registerForm.addEventListener('submit', async (ev) => {
    ev.preventDefault();
    if (!modelsLoaded) { alert("Face models still loading."); return; }

    const name = document.getElementById('studentName').value.trim();
    const id   = document.getElementById('studentId').value.trim();
    const branch = document.getElementById('studentBranch').value.trim();
    const section = document.getElementById('studentSection').value.trim();
    const phone = document.getElementById('studentPhone').value.trim();
    const photoInput = document.getElementById('studentPhoto');
    if (!photoInput.files || !photoInput.files[0]) { alert("Please upload a photo."); return; }

    const photoURL = URL.createObjectURL(photoInput.files[0]);
    try {
      const imgEl = await faceapi.fetchImage(photoURL);
      const detection = await faceapi.detectSingleFace(imgEl).withFaceLandmarks().withFaceDescriptor();
      if (!detection) { alert("No face detected."); return; }

      currentStudent = { name, id, branch, section, phone, photoURL, attendance: [], faceDescriptor: Array.from(detection.descriptor) };
      referenceDescriptor = new Float32Array(currentStudent.faceDescriptor);
      saveStudentToStorage(currentStudent, referenceDescriptor);

      successMessage.style.display = 'block';
      setTimeout(() => {
        document.getElementById('registerSection').classList.add('hidden');
        document.getElementById('attendanceSection').classList.remove('hidden');
      }, 900);
    } catch (err) {
      console.error("Registration error:", err);
    }
  });

  // --- QR Code logic ---
  let validQRCode = `QR-${Math.floor(Math.random() * 10000)}`;
  const qrCanvas = document.getElementById('qrCanvas');

  function updateQR() {
    validQRCode = `QR-${Math.floor(Math.random() * 10000)}`;
    document.getElementById('qrInfo').textContent = `Current QR: ${validQRCode}`;
    QRCode.toCanvas(qrCanvas, validQRCode, { width: 300, margin: 2 }, function (error) {
      if (error) console.error(error);
    });
  }
  updateQR();
  setInterval(updateQR, 120000);
document.getElementById('qrBtn').onclick = () => {
  const enteredQR = prompt("Enter the QR shown on screen:");
  if (enteredQR === validQRCode) {
    document.getElementById('qrResult').textContent = "✅ QR Verified! Attendance Recorded.";
    if (!currentStudent) currentStudent = JSON.parse(localStorage.getItem('currentStudent') || '{}');
    currentStudent.attendance.push({
      date: new Date().toISOString().split('T')[0],
      period: `P${currentStudent.attendance.length + 1}`,
      timestamp: new Date().toISOString()
    });
    saveStudentToStorage(currentStudent, referenceDescriptor);

    // ✅ Redirect to dashboard after 1 sec
    setTimeout(() => {
      document.getElementById('attendanceSection').classList.add('hidden');
      showDashboard();
    }, 1000);

  } else {
    document.getElementById('qrResult').textContent = "❌ Invalid QR!";
  }
};

 
  // --- Face Recognition ---
  document.getElementById('faceBtn').onclick = async () => {
    const faceResult = document.getElementById('faceResult');
    const video = document.getElementById('faceVideo');
    const canvas = document.getElementById('faceCanvas');

    if (!referenceDescriptor) {
      const raw = localStorage.getItem('currentStudent');
      if (raw) {
        const saved = JSON.parse(raw);
        if (saved && saved.faceDescriptor) {
          referenceDescriptor = new Float32Array(saved.faceDescriptor);
          currentStudent = saved;
        }
      }
    }
    if (!referenceDescriptor) { alert("No registered face found."); return; }

    faceResult.textContent = "Starting camera…";
    video.style.display = "block";
    try {
      const stream = await navigator.mediaDevices.getUserMedia({ video: true });
      video.srcObject = stream;
      await new Promise(resolve => video.onloadedmetadata = resolve);
      faceResult.textContent = "Click video to capture your face.";
    } catch (err) {
      console.error("Camera error:", err);
      faceResult.textContent = "Camera failed.";
      return;
    }

    video.onclick = async () => {
      canvas.width = video.videoWidth;
      canvas.height = video.videoHeight;
      const ctx = canvas.getContext('2d');
      ctx.drawImage(video, 0, 0, canvas.width, canvas.height);
      if (video.srcObject) video.srcObject.getTracks().forEach(t => t.stop());

      video.style.display = "none";
      canvas.style.display = "block";
      faceResult.textContent = "Processing…";

      try {
        const detection = await faceapi.detectSingleFace(canvas).withFaceLandmarks().withFaceDescriptor();
        if (!detection) { faceResult.textContent = "❌ No face detected."; return; }

        const distance = faceapi.euclideanDistance(referenceDescriptor, detection.descriptor);
        if (distance < 0.50) {
          faceResult.textContent = "✅ Face matched. Attendance recorded.";
          currentStudent.attendance.push({
            date: new Date().toISOString().split('T')[0],
            timestamp: new Date().toISOString()
          });
          saveStudentToStorage(currentStudent, referenceDescriptor);

          setTimeout(() => {
            document.getElementById('attendanceSection').classList.add('hidden');
            showDashboard();
          }, 900);
        } else {
          faceResult.textContent = "❌ Face mismatch. Attendance NOT recorded.";
        }
      } catch (err) {
        console.error("Face detection error:", err);
      }
    };
  };

  function showDashboard() {
    document.getElementById('dashboardSection').classList.remove('hidden');
    const dailyContainer = document.getElementById('dailyAttendance');
    dailyContainer.innerHTML = '';
    let student = JSON.parse(localStorage.getItem('currentStudent') || '{}');
    if (!student || !student.name) {
      dailyContainer.innerHTML = "<p>No student data found.</p>";
      return;
    }
    dailyContainer.innerHTML += `<p><strong>${student.name}</strong> — ID: ${student.id}</p>`;
    const attendanceByDate = {};
    (student.attendance || []).forEach(rec => {
      if (!attendanceByDate[rec.date]) attendanceByDate[rec.date] = [];
      attendanceByDate[rec.date].push(rec);
    });
    for (let date in attendanceByDate) {
      const records = attendanceByDate[date];
      let html = `<h3>${date}</h3><table><tr><th>Timestamp</th></tr>`;
      records.forEach(r => { html += `<tr><td>${r.timestamp}</td></tr>`; });
      html += `</table><p><b>Total Attendance Today:</b> ${records.length}</p>`;
      dailyContainer.innerHTML += html;
    }
  }
</script>
</body>
</html>
