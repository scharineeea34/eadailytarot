<html lang="th">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>EA Tarot</title>

  <!-- Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>

  <!-- Lucide Icons -->
  <script src="https://unpkg.com/lucide@latest"></script>

  <style>
    .flip {
      transition: 0.6s;
      transform-style: preserve-3d;
    }
    .flip.rotate {
      transform: rotateY(180deg);
    }
  </style>
</head>

<body class="min-h-screen bg-gradient-to-br from-pink-100 via-rose-50 to-purple-100 p-6">
  <div class="max-w-4xl mx-auto">
    <!-- Header -->
    <div class="text-center mb-8">
      <div class="flex justify-center items-center gap-3 mb-4">
        <i data-lucide="sparkles" class="text-pink-400 w-8 h-8"></i>
        <h1 class="text-4xl font-bold text-pink-600">EA Tarot</h1>
        <i data-lucide="sparkles" class="text-pink-400 w-8 h-8"></i>
      </div>
      <p class="text-pink-700 text-lg">
        ค้นหาแนวทางการบริหารด้วยไพ่ยิปซี + ทฤษฎีบริหารการศึกษา
      </p>
    </div>

    <!-- Mode Buttons -->
    <div class="flex justify-center gap-4 mb-8">
      <button id="dailyBtn"
        class="flex items-center gap-2 px-6 py-3 rounded-full font-semibold bg-gradient-to-r from-pink-400 to-rose-400 text-white shadow-lg scale-105">
        <i data-lucide="calendar" class="w-5 h-5"></i> ไพ่ประจำวัน
      </button>

      <button id="topicBtn"
        class="flex items-center gap-2 px-6 py-3 rounded-full font-semibold bg-white text-pink-600 border-2 border-pink-200 hover:bg-pink-50">
        <i data-lucide="filter" class="w-5 h-5"></i> ไพ่ตามหัวข้อ
      </button>
    </div>

    <!-- Topic Section -->
    <div id="topicSection" class="hidden mb-8">
      <h3 class="text-center text-pink-700 font-bold text-xl mb-4">
        เลือกหัวข้อที่ต้องการคำแนะนำ
      </h3>

      <div id="topicButtons" class="grid grid-cols-2 md:grid-cols-4 gap-4"></div>
    </div>

    <!-- Draw Card Section (Daily) -->
    <div id="drawSection" class="text-center">
      <div class="mb-8 flex justify-center">
        <div id="drawCardBtn"
          class="relative w-64 h-96 cursor-pointer transform transition-all hover:scale-105 hover:-rotate-2">
          <div class="absolute inset-0 bg-gradient-to-br from-pink-300 to-rose-400 rounded-2xl shadow-2xl border-8 border-pink-200 rotate-3"></div>
          <div class="absolute inset-0 bg-gradient-to-br from-pink-300 to-rose-400 rounded-2xl shadow-2xl border-8 border-pink-200 rotate-1"></div>
          <div
            class="absolute inset-0 bg-gradient-to-br from-pink-300 to-rose-400 rounded-2xl shadow-2xl border-8 border-pink-200 flex flex-col items-center justify-center">
            <div class="text-8xl mb-4">🔮</div>
            <div class="text-white font-bold text-xl">EA Tarot</div>
            <div class="text-pink-100 text-sm mt-2">คลิกเพื่อเปิดไพ่</div>
          </div>
        </div>
      </div>
    </div>

    <!-- Result Card -->
    <div id="resultCard" class="hidden bg-white rounded-2xl p-6 shadow-xl border border-pink-200"></div>
  </div>

  <script>
    lucide.createIcons();

    /* --------------------------
         Data (เหมือน React)
    ---------------------------*/
    const topics = [
      { id: 'personnel', name: 'บุคลากร', icon: '👥' },
      { id: 'budget', name: 'งบประมาณ', icon: '💰' },
      { id: 'students', name: 'นักเรียน', icon: '🎓' },
      { id: 'community', name: 'ชุมชน', icon: '🏘️' }
    ];

    const tarotCards = [
      {
        name: "ผู้นำแห่งการเปลี่ยนแปลง",
        image: "🎯",
        category: "personnel",
        situation: "โรงเรียนกำลังเผชิญกับการเปลี่ยนแปลงที่สำคัญ...",
        guidance: "ใช้หลักการบริหารเชิงกลยุทธ์...",
        theory: "ทฤษฎีการจัดการการเปลี่ยนแปลง",
        action: "จัดประชุมชี้แจง...",
        resources: [
          { title: "Leadership in the Implementation of Change", journal: "Journal of Change Management", url: "https://www.tandfonline.com" }
        ]
      },
      {
        name: "งบประมาณแห่งปัญญา",
        image: "💰",
        category: "budget",
        situation: "ประเด็นด้านการเงินและทรัพยากรต้องการความระมัดระวัง...",
        guidance: "ใช้หลักการบริหารจัดการทรัพยากร...",
        theory: "การบริหารการเงินการคลัง",
        action: "ทบทวนแผนการใช้จ่าย...",
        resources: []
      }
      // … ให้คุณเพิ่มไพ่ที่เหลือได้เหมือน React
    ];

    /* --------------------------
          State Variables
    ---------------------------*/
    let mode = "daily";
    let selectedTopic = null;
    let selectedCard = null;

    /* --------------------------
          DOM Elements
    ---------------------------*/
    const topicSection = document.getElementById("topicSection");
    const topicButtons = document.getElementById("topicButtons");
    const drawSection = document.getElementById("drawSection");
    const resultCard = document.getElementById("resultCard");

    /* --------------------------
          Render Topic Buttons
    ---------------------------*/
    topics.forEach(t => {
      const btn = document.createElement("button");
      btn.className =
        "bg-white border-2 border-pink-200 rounded-2xl p-6 flex flex-col items-center gap-3 hover:scale-105 hover:shadow-lg transition";
      btn.innerHTML = `<div class="text-5xl">${t.icon}</div><div class="font-bold">${t.name}</div>`;
      btn.onclick = () => handleTopicSelect(t.id);
      topicButtons.appendChild(btn);
    });

    /* --------------------------
             Draw Card
    ---------------------------*/
    function drawCard(topicFilter = null) {
      // Daily mode locking
      if (mode === "daily") {
        const todayKey = localStorage.getItem("todayReading");
        if (todayKey) {
          alert("คุณได้ทำนายไปแล้ววันนี้ กลับมาใหม่พรุ่งนี้นะคะ 🙏");
          return;
        }
      }

      let cards = tarotCards;
      if (topicFilter) cards = tarotCards.filter(c => c.category === topicFilter);

      const randomCard = cards[Math.floor(Math.random() * cards.length)];
      selectedCard = randomCard;

      // Save daily
      if (mode === "daily") {
        localStorage.setItem("todayReading", JSON.stringify({
          date: new Date().toDateString(),
          card: randomCard
        }));
      }

      renderResult(randomCard);
    }

    /* --------------------------
          Render Result Card
    ---------------------------*/
    function renderResult(card) {
      resultCard.classList.remove("hidden");
      drawSection.classList.add("hidden");
      topicSection.classList.add("hidden");

      resultCard.innerHTML = `
        <div class="text-center mb-6">
          <div class="text-7xl mb-4">${card.image}</div>
          <h2 class="text-3xl font-bold text-pink-600">${card.name}</h2>
        </div>

        <p class="mb-4"><strong>สถานการณ์:</strong> ${card.situation}</p>
        <p class="mb-4"><strong>คำแนะนำ:</strong> ${card.guidance}</p>
        <p class="mb-4"><strong>ทฤษฎีที่เกี่ยวข้อง:</strong> ${card.theory}</p>
        <p class="mb-4"><strong>การนำไปใช้:</strong> ${card.action}</p>

        ${card.resources.length > 0 ?
          `<div><strong>แหล่งข้อมูลเพิ่มเติม:</strong>
            <ul class="list-disc pl-6 mt-2">
              ${card.resources.map(r => `<li><a class="text-blue-600 underline" href="${r.url}" target="_blank">${r.title}</a> (${r.journal})</li>`).join("")}
            </ul>
          </div>`
        : ""}

        <div class="text-center mt-8">
          <button onclick="location.reload()" class="px-6 py-3 bg-pink-500 text-white rounded-full hover:bg-pink-600">
            เริ่มใหม่
          </button>
        </div>
      `;
    }

    /* --------------------------
          Event Listeners
    ---------------------------*/
    document.getElementById("drawCardBtn").onclick = () => drawCard();

    document.getElementById("dailyBtn").onclick = () => {
      mode = "daily";
      selectedCard = null;
      selectedTopic = null;

      drawSection.classList.remove("hidden");
      topicSection.classList.add("hidden");
      resultCard.classList.add("hidden");
    };

    document.getElementById("topicBtn").onclick = () => {
      mode = "topic";

      drawSection.classList.add("hidden");
      topicSection.classList.remove("hidden");
      resultCard.classList.add("hidden");
    };

    function handleTopicSelect(topicId) {
      selectedTopic = topicId;
      drawCard(topicId);
    }
  </script>
</body>
</html>
