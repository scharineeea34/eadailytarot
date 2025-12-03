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
    
    <!-- Footer -->
    <div class="text-center mt-12 pb-6">
      <p class="text-pink-600 text-sm font-medium">
        🌙 สร้างสรรค์ด้วยความปรารถนาดีต่อผู้บริหารการศึกษาไทยทุกท่าน 🌙
      </p>
    </div>
  </div>

  <script>
    // Wait for page to load completely
    window.addEventListener('DOMContentLoaded', function() {
      if (typeof lucide !== 'undefined') {
        lucide.createIcons();
      }
    });

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
        situation: "โรงเรียนกำลังเผชิญกับการเปลี่ยนแปลงที่สำคัญ อาจเป็นนโยบายใหม่หรือการปรับปรุงหน่วยงาน",
        guidance: "ใช้หลักการบริหารเชิงกลยุทธ์ (Strategic Management) วางแผนการเปลี่ยนแปลงอย่างเป็นระบบ สื่อสารวิสัยทัศน์ให้ชัดเจน และสร้างทีมขับเคลื่อนการเปลี่ยนแปลง ตามแนวคิดของ Kotter's 8-Step Change Model",
        theory: "ทฤษฎีการจัดการการเปลี่ยนแปลง (Change Management Theory)",
        action: "จัดประชุมชี้แจงวิสัยทัศน์กับคณะครูและบุคลากร สร้างความเข้าใจและการมีส่วนร่วม",
        resources: [
        { 
          title: "Leadership in the Implementation of Change: Functions, Sources, and Requisite Variety",
          journal: "Journal of Change Management (Taylor & Francis)",
          url: "https://www.tandfonline.com/doi/full/10.1080/14697017.2021.1861697"
        },
        ]
      },
      {
      name: "งบประมาณแห่งปัญญา",
      image: "💰",
      category: "budget",
      situation: "ประเด็นด้านการเงินและทรัพยากรต้องการความระมัดระวัง อาจต้องจัดสรรงบประมาณหรือหาแหล่งทุนเพิ่มเติม",
      guidance: "ใช้หลักการบริหารจัดการทรัพยากร (Resource Management) และหลักธรรมาภิบาล (Good Governance) ในการบริหารงบประมาณอย่างโปร่งใส มีประสิทธิภาพ ตรวจสอบได้ และคุ้มค่า ตามหลัก 3E's: Economy, Efficiency, Effectiveness",
      theory: "ทฤษฎีการบริหารการเงินการคลัง (Financial Management in Education)",
      action: "ทบทวนแผนการใช้จ่ายงบประมาณ หาแนวทางเพิ่มประสิทธิภาพการใช้ทรัพยากร",
      resources: [
        { 
          title: "Leadership and Policy in Schools: Journal Focus on Resource Management",
          journal: "Leadership and Policy in Schools (Taylor & Francis)",
          url: "https://www.tandfonline.com/journals/nlps20"
        }
      ]
      },
      {
      name: "ครูผู้สร้างสรรค์",
      image: "📚",
      category: "personnel",
      situation: "วันนี้เป็นวันที่ต้องเน้นการพัฒนาศักยภาพบุคลากร ครูอาจต้องการการสนับสนุนในการพัฒนาวิชาชีพ",
      guidance: "ประยุกต์ใช้ทฤษฎีการพัฒนาทรัพยากรมนุษย์ (Human Resource Development) ส่งเสริม Professional Learning Community (PLC) จัดระบบ Coaching & Mentoring และสนับสนุนการเรียนรู้ตลอดชีวิตของบุคลากร",
      theory: "ทฤษฎีการพัฒนาองค์กรแห่งการเรียนรู้ (Learning Organization)",
      action: "จัดเวทีแลกเปลี่ยนเรียนรู้ระหว่างครู หรือสนับสนุนการอบรมพัฒนาทักษะใหม่ๆ",
      resources: [
        { 
          title: "Leading Organizational Learning in Dynamic Environments",
          journal: "Journal of Graduate Medical Education (PMC)",
          url: "https://pmc.ncbi.nlm.nih.gov/articles/PMC10324730/"
        }
      ]
    },
    {
      name: "นักเรียนแห่งอนาคต",
      image: "🎓",
      category: "students",
      situation: "โฟกัสของวันนี้อยู่ที่นักเรียน อาจมีประเด็นเกี่ยวกับผลสัมฤทธิ์ทางการเรียนหรือพัฒนาการของนักเรียน",
      guidance: "ใช้หลักการบริหารเชิงคุณภาพ (Total Quality Management in Education) มุ่งเน้น Student-Centered Learning ใช้ข้อมูลในการตัดสินใจ (Data-Driven Decision Making) และพัฒนาระบบประเมินผลที่สอดคล้องกับมาตรฐานการศึกษา",
      theory: "ทฤษฎีการบริหารคุณภาพทางการศึกษา (Educational Quality Management)",
      action: "วิเคราะห์ข้อมูลผลสัมฤทธิ์ทางการเรียน ออกแบบกิจกรรมพัฒนาศักยภาพนักเรียน",
      resources: [
        { 
          title: "Data Driven Teaching and Real Time Decision Making in Education Management",
          journal: "ICFNDS '24: Proceedings of the 8th International Conference on Future Networks & Distributed Systems",
          url: "https://dl.acm.org/doi/10.1145/3726122.3726232"
        }
      ]
    },
    {
      name: "ผู้ปกครองผู้สนับสนุน",
      image: "👨‍👩‍👧",
      category: "community",
      situation: "ความสัมพันธ์กับชุมชนและผู้ปกครองเป็นจุดสำคัญ อาจต้องสื่อสารหรือขอความร่วมมือในโครงการต่างๆ",
      guidance: "ประยุกต์ใช้ทฤษฎีการมีส่วนร่วม (Participatory Management) สร้างเครือข่ายความร่วมมือระหว่างโรงเรียน-บ้าน-ชุมชน ตามแนวคิด Epstein's Framework of Six Types of Involvement เพื่อส่งเสริมความสำเร็จของนักเรียน",
      theory: "ทฤษฎีการบริหารแบบมีส่วนร่วม (Participative Leadership)",
      action: "จัดกิจกรรมสร้างความสัมพันธ์กับผู้ปกครอง หรือประชุมคณะกรรมการสถานศึกษา",
      resources: [
        { 
          title: "Participative Leadership: A Literature Review and Prospects for Future Research",
          journal: "Frontiers in Psychology (PMC)",
          url: "https://pmc.ncbi.nlm.nih.gov/articles/PMC9204162/"
        }
      ]
    },
        {
      name: "สิ่งแวดล้อมแห่งการเรียนรู้",
      image: "🏫",
      category: "students",
      situation: "โครงสร้างพื้นฐานและสภาพแวดล้อมทางกายภาพต้องการความสนใจ อาจต้องปรับปรุงอาคารสถานที่หรือสื่อการเรียนการสอน",
      guidance: "ประยุกต์ใช้ทฤษฎีการบริหารสภาพแวดล้อมการเรียนรู้ (Learning Environment Management) สร้าง Safe and Inclusive Learning Space ที่ส่งเสริมการเรียนรู้ในศตวรรษที่ 21 พัฒนาโครงสร้างพื้นฐานทั้งทางกายภาพและดิจิทัล",
      theory: "ทฤษฎีการออกแบบสภาพแวดล้อมเพื่อการเรียนรู้",
      action: "ตรวจสอบความปลอดภัยของอาคารสถานที่ วางแผนปรับปรุงพัฒนาสิ่งอำนวยความสะดวก",
      resources: [
        { 
          title: "A systematic literature review on adaptive content recommenders in personalized learning environments from 2015 to 2020",
          journal: " Journal of Computers in Education",
          url: "https://link.springer.com/article/10.1007/s40692-021-00199-4"
        }
      ]
    },
    {
      name: "นวัตกรรมแห่งการศึกษา",
      image: "💡",
      category: "students",
      situation: "โอกาสในการนำนวัตกรรมมาใช้ในการบริหารและการจัดการเรียนการสอน เทคโนโลยีเป็นตัวช่วยสำคัญ",
      guidance: "ใช้หลัก Instructional Leadership ส่งเสริมการใช้นวัตกรรมทางการศึกษา สนับสนุน Active Learning, STEM Education, และ Digital Literacy ตามกรอบ TPACK (Technological Pedagogical Content Knowledge) เพื่อบูรณาการเทคโนโลยีในการเรียนการสอน",
      theory: "ทฤษฎีภาวะผู้นำเชิงวิชาการ (Instructional Leadership)",
      action: "สำรวจนวัตกรรมใหม่ๆ ที่เหมาะกับบริบทของโรงเรียน จัดอบรมครูด้านเทคโนโลยี",
      resources: [
        { 
          title: "Instructional leadership in a centralized and competitive educational system: a qualitative meta-synthesis of research from Turkey",
          journal: "Journal of Educational Administration",
          url: "https://www.emerald.com/jea/article-abstract/59/6/702/201647/Instructional-leadership-in-a-centralized-and?redirectedFrom=fulltext"
        }
      ]
    },
    {
      name: "วัฒนธรรมองค์กร",
      image: "🌟",
      category: "personnel",
      situation: "บรรยากาศและวัฒนธรรมการทำงานในองค์กรมีผลต่อประสิทธิภาพ อาจต้องสร้างแรงจูงใจหรือปรับปรุงบรรยากาศการทำงาน",
      guidance: "ประยุกต์ใช้ทฤษฎีวัฒนธรรมองค์กร (Organizational Culture) และ Transformational Leadership สร้างวิสัยทัศน์ร่วม ส่งเสริมการทำงานเป็นทีม พัฒนาค่านิยมองค์กรที่เน้นความเป็นเลิศทางวิชาการและการดูแลนักเรียนอย่างรอบด้าน",
      theory: "ทฤษฎีภาวะผู้นำการเปลี่ยนแปลง (Transformational Leadership)",
      action: "จัดกิจกรรมสร้างขวัญกำลังใจบุคลากร ส่งเสริมบรรยากาศการทำงานที่เป็นมิตร",
      resources: [
        { 
          title: "Transformational leadership effectiveness: Evidence-based primer",
          journal: "European Journal of Work and Organizational Psychology (Taylor & Francis)",
          url: "https://www.tandfonline.com/doi/full/10.1080/13678868.2022.2135938"
        }
      ]
    },
    {
      name: "มาตรฐานแห่งความเป็นเลิศ",
      image: "🏆",
      category: "students",
      situation: "วันนี้เหมาะกับการทบทวนมาตรฐานและประเมินคุณภาพการศึกษา อาจมีการตรวจเยี่ยมหรือประเมินภายนอก",
      guidance: "ใช้หลัก Quality Assurance in Education และ Continuous Improvement Cycle (Plan-Do-Check-Act) ทบทวนมาตรฐานการศึกษา พัฒนาระบบประกันคุณภาพภายใน จัดทำ Self-Assessment Report (SAR) และวางแผนพัฒนาอย่างต่อเนื่อง",
      theory: "ทฤษฎีการประกันคุณภาพการศึกษา (Educational Quality Assurance)",
      action: "ทบทวนมาตรฐานการศึกษาของสถานศึกษา ตรวจสอบหลักฐานเชิงประจักษ์",
      resources: [
        { 
          title: "Best practices for quality assurance in higher education: implications for educational administration",
          journal: "International Journal of Leadership in Education ",
          url: "https://www.tandfonline.com/doi/full/10.1080/13603124.2019.1710569"
        }
      ]
    },
    {
      name: "ความสมดุลแห่งชีวิต",
      image: "⚖️",
      category: "personnel",
      situation: "ความสมดุลระหว่างงานและชีวิตเป็นสิ่งสำคัญ ต้องดูแลสุขภาวะของตนเองและทีมงาน",
      guidance: "ประยุกต์ใช้หลัก Work-Life Balance และ Wellness Management สร้าง Healthy Work Environment ส่งเสริมสุขภาพกายและใจของบุคลากร ป้องกัน Burnout และสร้างความยั่งยืนในการทำงาน ตามแนวคิด Servant Leadership ที่ใส่ใจความเป็นอยู่ของทุกคน",
      theory: "ทฤษฎีภาวะผู้นำเชิงรับใช้ (Servant Leadership)",
      action: "จัดกิจกรรมส่งเสริมสุขภาพบุคลากร ให้เวลาพักผ่อนที่เหมาะสม",
      resources: [
        { 
          title: "Servant leadership to support wellbeing in higher education teaching",
          journal: "Journal of Further and Higher Education",
          url: "https://www.tandfonline.com/doi/full/10.1080/0309877X.2021.2023733"
        }
      ]
    },
    {
      name: "ทรัพยากรที่คุ้มค่า",
      image: "💎",
      category: "budget",
      situation: "การจัดการทรัพยากรอย่างมีประสิทธิภาพเป็นกุญแจสำคัญ มีโอกาสในการหาแหล่งทุนหรือพันธมิตรใหม่",
      guidance: "ประยุกต์ใช้หลักการบริหารเชิงกลยุทธ์ทางการเงิน (Strategic Financial Management) วิเคราะห์ต้นทุนและผลประโยชน์ (Cost-Benefit Analysis) สร้างเครือข่ายพันธมิตรทางธุรกิจและชุมชน และพัฒนาแผนระดมทุนที่ยั่งยืน",
      theory: "ทฤษฎีการจัดการทรัพยากรเชิงกลยุทธ์ (Strategic Resource Management)",
      action: "จัดทำแผนการใช้ทรัพยากรให้เกิดประโยชน์สูงสุด สำรวจแหล่งทุนทางเลือก",
      resources: [
        { 
          title: "Strategic Leadership, Strategic Resources Allocation, Strategic Incentive and Performance of Public Secondary Schools in Bungoma, County.Kenya",
          journal: "Journal of Business and Management Sciences",
          url: "https://www.researchgate.net/publication/373172603_Strategic_Leadership_Strategic_Resources_Allocation_Strategic_Incentive_and_Performance_of_Public_Secondary_Schools_in_Bungoma_CountyKenya?enrichId=rgreq-d53be51b9cdf4c178fe1c15be15ced63-XXX&enrichSource=Y292ZXJQYWdlOzM3MzE3MjYwMztBUzoxMTQzMTI4MTE4MjExNTYzN0AxNjkyMjkwMDg3NTY1&el=1_x_3"
        }
      ]
    },
    {
      name: "เครือข่ายชุมชน",
      image: "🤝",
      category: "community",
      situation: "การสร้างความร่วมมือกับชุมชนจะเป็นปัจจัยสำคัญในการขับเคลื่อนโรงเรียน มีโอกาสในการสร้างพันธมิตร",
      guidance: "ใช้หลักการบริหารแบบมีส่วนร่วม (Collaborative Leadership) สร้างเครือข่ายความร่วมมือกับองค์กรภาครัฐ เอกชน และชุมชนท้องถิ่น พัฒนาโครงการที่เกิดประโยชน์ร่วมกัน (Win-Win Projects) และสร้างความไว้วางใจผ่านการสื่อสารที่โปร่งใส",
      theory: "ทฤษฎีภาวะผู้นำแบบร่วมมือ (Collaborative Leadership Theory)",
      action: "ประชุมผู้นำชุมชนเพื่อหาโอกาสความร่วมมือ จัดกิจกรรมเปิดบ้านโรงเรียน",
      resources: [
        { 
          title: "Participative leadership and change-oriented organizational citizenship",
          journal: "Eurasian Journal of Educational Research (ERIC)",
          url: "https://files.eric.ed.gov/fulltext/EJ1097902.pdf"
        }
      ]
    }
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
      let cards = tarotCards;
      if (topicFilter) cards = tarotCards.filter(c => c.category === topicFilter);

      const randomCard = cards[Math.floor(Math.random() * cards.length)];
      selectedCard = randomCard;

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
