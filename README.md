import React, { useState, useEffect } from 'react';
import { Sparkles, BookOpen, Users, TrendingUp } from 'lucide-react';

const SchoolAdminTarot = () => {
  const [selectedCard, setSelectedCard] = useState(null);
  const [isFlipping, setIsFlipping] = useState(false);
  const [todayReading, setTodayReading] = useState(null);

  const tarotCards = [
    {
      name: "ผู้นำแห่งการเปลี่ยนแปลง",
      image: "🎯",
      situation: "โรงเรียนกำลังเผชิญกับการเปลี่ยนแปลงที่สำคัญ อาจเป็นนโยบายใหม่หรือการปรับปรุงหน่วยงาน",
      guidance: "ใช้หลักการบริหารเชิงกลยุทธ์ (Strategic Management) วางแผนการเปลี่ยนแปลงอย่างเป็นระบบ สื่อสารวิสัยทัศน์ให้ชัดเจน และสร้างทีมขับเคลื่อนการเปลี่ยนแปลง ตามแนวคิดของ Kotter's 8-Step Change Model",
      theory: "ทฤษฎีการจัดการการเปลี่ยนแปลง (Change Management Theory)",
      action: "จัดประชุมชี้แจงวิสัยทัศน์กับคณะครูและบุคลากร สร้างความเข้าใจและการมีส่วนร่วม",
      resources: [
        { title: "ทฤษฎีการบริหารการศึกษาในศตวรรษที่ 21", url: "https://www.xn--12co8bkb4ccba6b3geffwj63b.com/theory-of-educational-administration-in-the-21st-century/" },
        { title: "Oxford Research: Transformational Leadership and Change", url: "https://oxfordre.com/education/display/10.1093/acrefore/9780190264093.001.0001/acrefore-9780190264093-e-631" }
      ]
    },
    {
      name: "ครูผู้สร้างสรรค์",
      image: "📚",
      situation: "วันนี้เป็นวันที่ต้องเน้นการพัฒนาศักยภาพบุคลากร ครูอาจต้องการการสนับสนุนในการพัฒนาวิชาชีพ",
      guidance: "ประยุกต์ใช้ทฤษฎีการพัฒนาทรัพยากรมนุษย์ (Human Resource Development) ส่งเสริม Professional Learning Community (PLC) จัดระบบ Coaching & Mentoring และสนับสนุนการเรียนรู้ตลอดชีวิตของบุคลากร",
      theory: "ทฤษฎีการพัฒนาองค์กรแห่งการเรียนรู้ (Learning Organization)",
      action: "จัดเวทีแลกเปลี่ยนเรียนรู้ระหว่างครู หรือสนับสนุนการอบรมพัฒนาทักษะใหม่ๆ",
      resources: [
        { title: "13 บทกับหลักการทฤษฎีการบริหารการศึกษา", url: "https://www.nupress.grad.nu.ac.th/การบริหารการศึกษา/" },
        { title: "Frontiers: Transformational Leadership Framework", url: "https://www.frontiersin.org/journals/psychology/articles/10.3389/fpsyg.2024.1331597/full" }
      ]
    },
    {
      name: "นักเรียนแห่งอนาคต",
      image: "🎓",
      situation: "โฟกัสของวันนี้อยู่ที่นักเรียน อาจมีประเด็นเกี่ยวกับผลสัมฤทธิ์ทางการเรียนหรือพัฒนาการของนักเรียน",
      guidance: "ใช้หลักการบริหารเชิงคุณภาพ (Total Quality Management in Education) มุ่งเน้น Student-Centered Learning ใช้ข้อมูลในการตัดสินใจ (Data-Driven Decision Making) และพัฒนาระบบประเมินผลที่สอดคล้องกับมาตรฐานการศึกษา",
      theory: "ทฤษฎีการบริหารคุณภาพทางการศึกษา (Educational Quality Management)",
      action: "วิเคราะห์ข้อมูลผลสัมฤทธิ์ทางการเรียน ออกแบบกิจกรรมพัฒนาศักยภาพนักเรียน",
      resources: [
        { title: "การบริหารงานวิชาการในศตวรรษที่ 21", url: "https://mbuisc.ac.th/mbuiscethesis/down/2566/6420440432019.pdf" },
        { title: "Park University: Transformational Leadership in Education", url: "https://www.park.edu/blog/transformational-leadership-in-education-how-to-inspire-change/" }
      ]
    },
    {
      name: "ผู้ปกครองผู้สนับสนุน",
      image: "👨‍👩‍👧",
      situation: "ความสัมพันธ์กับชุมชนและผู้ปกครองเป็นจุดสำคัญ อาจต้องสื่อสารหรือขอความร่วมมือในโครงการต่างๆ",
      guidance: "ประยุกต์ใช้ทฤษฎีการมีส่วนร่วม (Participatory Management) สร้างเครือข่ายความร่วมมือระหว่างโรงเรียน-บ้าน-ชุมชน ตามแนวคิด Epstein's Framework of Six Types of Involvement เพื่อส่งเสริมความสำเร็จของนักเรียน",
      theory: "ทฤษฎีการบริหารแบบมีส่วนร่วม (Participative Leadership)",
      action: "จัดกิจกรรมสร้างความสัมพันธ์กับผู้ปกครอง หรือประชุมคณะกรรมการสถานศึกษา",
      resources: [
        { title: "ResearchGate: Transformational Leadership in Education", url: "https://www.researchgate.net/publication/371002899_Transformational_Leadership_in_Education_A_Comprehensive_Approach_to_Educational_Success" },
        { title: "แนวคิดการบริหารการศึกษาเพื่อความเป็นเลิศ", url: "https://so04.tci-thaijo.org/index.php/JRBGS/article/view/254607" }
      ]
    },
    {
      name: "งบประมาณแห่งปัญญา",
      image: "💰",
      situation: "ประเด็นด้านการเงินและทรัพยากรต้องการความระมัดระวัง อาจต้องจัดสรรงบประมาณหรือหาแหล่งทุนเพิ่มเติม",
      guidance: "ใช้หลักการบริหารจัดการทรัพยากร (Resource Management) และหลักธรรมาภิบาล (Good Governance) ในการบริหารงบประมาณอย่างโปร่งใส มีประสิทธิภาพ ตรวจสอบได้ และคุ้มค่า ตามหลัก 3E's: Economy, Efficiency, Effectiveness",
      theory: "ทฤษฎีการบริหารการเงินการคลัง (Financial Management in Education)",
      action: "ทบทวนแผนการใช้จ่ายงบประมาณ หาแนวทางเพิ่มประสิทธิภาพการใช้ทรัพยากร",
      resources: [
        { title: "การศึกษาแนวทางการบริหารสถานศึกษา", url: "http://ir-ithesis.swu.ac.th/dspace/bitstream/123456789/2873/1/gs651160110.pdf" },
        { title: "MDPI: Impact of Transformational Leadership", url: "https://www.mdpi.com/2075-4698/13/6/133" }
      ]
    },
    {
      name: "สิ่งแวดล้อมแห่งการเรียนรู้",
      image: "🏫",
      situation: "โครงสร้างพื้นฐานและสภาพแวดล้อมทางกายภาพต้องการความสนใจ อาจต้องปรับปรุงอาคารสถานที่หรือสื่อการเรียนการสอน",
      guidance: "ประยุกต์ใช้ทฤษฎีการบริหารสภาพแวดล้อมการเรียนรู้ (Learning Environment Management) สร้าง Safe and Inclusive Learning Space ที่ส่งเสริมการเรียนรู้ในศตวรรษที่ 21 พัฒนาโครงสร้างพื้นฐานทั้งทางกายภาพและดิจิทัล",
      theory: "ทฤษฎีการออกแบบสภาพแวดล้อมเพื่อการเรียนรู้",
      action: "ตรวจสอบความปลอดภัยของอาคารสถานที่ วางแผนปรับปรุงพัฒนาสิ่งอำนวยความสะดวก",
      resources: [
        { title: "PMC: Deep Learning in Educational Management", url: "https://pmc.ncbi.nlm.nih.gov/articles/PMC9217561/" },
        { title: "ResearchGate: Impact on School Culture", url: "https://www.researchgate.net/publication/374537608_The_Impact_of_Transformational_Leadership_on_School_Culture" }
      ]
    },
    {
      name: "นวัตกรรมแห่งการศึกษา",
      image: "💡",
      situation: "โอกาสในการนำนวัตกรรมมาใช้ในการบริหารและการจัดการเรียนการสอน เทคโนโลยีเป็นตัวช่วยสำคัญ",
      guidance: "ใช้หลัก Instructional Leadership ส่งเสริมการใช้นวัตกรรมทางการศึกษา สนับสนุน Active Learning, STEM Education, และ Digital Literacy ตามกรอบ TPACK (Technological Pedagogical Content Knowledge) เพื่อบูรณาการเทคโนโลยีในการเรียนการสอน",
      theory: "ทฤษฎีภาวะผู้นำเชิงวิชาการ (Instructional Leadership)",
      action: "สำรวจนวัตกรรมใหม่ๆ ที่เหมาะกับบริบทของโรงเรียน จัดอบรมครูด้านเทคโนโลยี",
      resources: [
        { title: "Cognia: Transformational Leadership Perspective", url: "https://source.cognia.org/issue-article/transformational-leadership-matter-perspective/" },
        { title: "Frontiers: School Leadership Impact", url: "https://www.frontiersin.org/journals/education/articles/10.3389/feduc.2023.1171513/full" }
      ]
    },
    {
      name: "วัฒนธรรมองค์กร",
      image: "🌟",
      situation: "บรรยากาศและวัฒนธรรมการทำงานในองค์กรมีผลต่อประสิทธิภาพ อาจต้องสร้างแรงจูงใจหรือปรับปรุงบรรยากาศการทำงาน",
      guidance: "ประยุกต์ใช้ทฤษฎีวัฒนธรรมองค์กร (Organizational Culture) และ Transformational Leadership สร้างวิสัยทัศน์ร่วม ส่งเสริมการทำงานเป็นทีม พัฒนาค่านิยมองค์กรที่เน้นความเป็นเลิศทางวิชาการและการดูแลนักเรียนอย่างรอบด้าน",
      theory: "ทฤษฎีภาวะผู้นำการเปลี่ยนแปลง (Transformational Leadership)",
      action: "จัดกิจกรรมสร้างขวัญกำลังใจบุคลากร ส่งเสริมบรรยากาศการทำงานที่เป็นมิตร",
      resources: [
        { title: "ฐานข้อมูลบทความวิจัย สาขาการบริหารการศึกษา", url: "https://art.krirk.ac.th/paper-education-administration/" },
        { title: "Educational Outcomes Research Paper", url: "https://open-publishing.org/journals/index.php/jeicom/article/download/949/811/1877" }
      ]
    },
    {
      name: "มาตรฐานแห่งความเป็นเลิศ",
      image: "🏆",
      situation: "วันนี้เหมาะกับการทบทวนมาตรฐานและประเมินคุณภาพการศึกษา อาจมีการตรวจเยี่ยมหรือประเมินภายนอก",
      guidance: "ใช้หลัก Quality Assurance in Education และ Continuous Improvement Cycle (Plan-Do-Check-Act) ทบทวนมาตรฐานการศึกษา พัฒนาระบบประกันคุณภาพภายใน จัดทำ Self-Assessment Report (SAR) และวางแผนพัฒนาอย่างต่อเนื่อง",
      theory: "ทฤษฎีการประกันคุณภาพการศึกษา (Educational Quality Assurance)",
      action: "ทบทวนมาตรฐานการศึกษาของสถานศึกษา ตรวจสอบหลักฐานเชิงประจักษ์",
      resources: [
        { title: "สรุปหลักการและทฤษฎีการบริหารการศึกษา", url: "https://anyflip.com/tbwxl/bvyw/basic/" },
        { title: "ทฤษฎีการบริหารการศึกษาในศตวรรษที่ 21", url: "https://www.xn--12co8bkb4ccba6b3geffwj63b.com/theory-of-educational-administration-in-the-21st-century/" }
      ]
    },
    {
      name: "ความสมดุลแห่งชีวิต",
      image: "⚖️",
      situation: "ความสมดุลระหว่างงานและชีวิตเป็นสิ่งสำคัญ ต้องดูแลสุขภาวะของตนเองและทีมงาน",
      guidance: "ประยุกต์ใช้หลัก Work-Life Balance และ Wellness Management สร้าง Healthy Work Environment ส่งเสริมสุขภาพกายและใจของบุคลากร ป้องกัน Burnout และสร้างความยั่งยืนในการทำงาน ตามแนวคิด Servant Leadership ที่ใส่ใจความเป็นอยู่ของทุกคน",
      theory: "ทฤษฎีภาวะผู้นำเชิงรับใช้ (Servant Leadership)",
      action: "จัดกิจกรรมส่งเสริมสุขภาพบุคลากร ให้เวลาพักผ่อนที่เหมาะสม",
      resources: [
        { title: "Oxford Research: Transformational Leadership", url: "https://oxfordre.com/education/display/10.1093/acrefore/9780190264093.001.0001/acrefore-9780190264093-e-631" },
        { title: "การบริหารและการจัดการศึกษา", url: "https://www.nupress.grad.nu.ac.th/การบริหารการศึกษา/" }
      ]
    }
  ];

  useEffect(() => {
    const today = new Date().toDateString();
    const saved = localStorage.getItem('todayReading');
    
    if (saved) {
      const parsed = JSON.parse(saved);
      if (parsed.date === today) {
        setTodayReading(parsed.card);
      } else {
        localStorage.removeItem('todayReading');
      }
    }
  }, []);

  const drawCard = () => {
    if (todayReading) {
      alert('คุณได้ทำนายไปแล้ววันนี้ กลับมาใหม่พรุ่งนี้นะคะ 🙏');
      return;
    }

    setIsFlipping(true);
    
    setTimeout(() => {
      const randomCard = tarotCards[Math.floor(Math.random() * tarotCards.length)];
      setSelectedCard(randomCard);
      setIsFlipping(false);
      
      const today = new Date().toDateString();
      localStorage.setItem('todayReading', JSON.stringify({
        date: today,
        card: randomCard
      }));
      setTodayReading(randomCard);
    }, 1000);
  };

  const resetReading = () => {
    setSelectedCard(null);
    // ไม่ลบ localStorage เพื่อให้เล่นได้วันละครั้ง
  };

  const displayCard = selectedCard || todayReading;

  return (
          <div className="min-h-screen bg-gradient-to-br from-pink-100 via-rose-50 to-purple-100 p-6">
      <div className="max-w-4xl mx-auto">
        <div className="text-center mb-8">
          <div className="flex justify-center items-center gap-3 mb-4">
            <Sparkles className="text-pink-400" size={32} />
            <h1 className="text-4xl font-bold text-pink-600">EA Tarot</h1>
            <Sparkles className="text-pink-400" size={32} />
          </div>
          <p className="text-pink-700 text-lg">
            ค้นหาแนวทางการบริหารประจำวันด้วยภูมิปัญญาไพ่ยิปซีและทฤษฎีบริหารการศึกษา
          </p>
        </div>

        {!displayCard ? (
          <div className="text-center">
            <div className="mb-8 flex justify-center">
              <div 
                onClick={drawCard}
                className={`relative w-64 h-96 cursor-pointer transform transition-all duration-300 ${
                  isFlipping ? 'scale-95 rotate-12' : 'hover:scale-105 hover:-rotate-2'
                }`}
              >
                {/* กองไพ่ */}
                <div className="absolute inset-0 bg-gradient-to-br from-pink-300 to-rose-400 rounded-2xl shadow-2xl border-8 border-pink-200 transform rotate-3"></div>
                <div className="absolute inset-0 bg-gradient-to-br from-pink-300 to-rose-400 rounded-2xl shadow-2xl border-8 border-pink-200 transform rotate-1"></div>
                <div className="absolute inset-0 bg-gradient-to-br from-pink-300 to-rose-400 rounded-2xl shadow-2xl border-8 border-pink-200 flex flex-col items-center justify-center">
                  <div className="text-8xl mb-4">🔮</div>
                  <div className="text-white font-bold text-xl">EA Tarot</div>
                  <div className="text-pink-100 text-sm mt-2">คลิกเพื่อจั่วไพ่</div>
                </div>
              </div>
            </div>
            
            {todayReading && (
              <p className="text-pink-600 mt-4 font-medium">
                คุณได้ทำนายไปแล้ววันนี้ กลับมาใหม่พรุ่งนี้นะคะ 🙏
              </p>
            )}
          </div>
        ) : (
          <div className="space-y-6 animate-fade-in">
            <div className="bg-white/80 backdrop-blur-md rounded-2xl p-8 shadow-xl border-2 border-pink-200">
              <div className="flex items-center justify-center gap-4 mb-6">
                <div className="text-8xl">{displayCard.image}</div>
                <div>
                  <h2 className="text-3xl font-bold text-pink-600 mb-2">{displayCard.name}</h2>
                  <div className="flex items-center gap-2 text-pink-500">
                    <BookOpen size={20} />
                    <span className="text-sm italic">{displayCard.theory}</span>
                  </div>
                </div>
              </div>
            </div>

            <div className="bg-white/80 backdrop-blur-md rounded-2xl p-6 shadow-xl border-2 border-pink-200">
              <div className="flex items-center gap-2 mb-3">
                <TrendingUp className="text-rose-500" size={24} />
                <h3 className="text-xl font-bold text-rose-600">สถานการณ์โรงเรียนวันนี้</h3>
              </div>
              <p className="text-gray-700 leading-relaxed">{displayCard.situation}</p>
            </div>

            <div className="bg-white/80 backdrop-blur-md rounded-2xl p-6 shadow-xl border-2 border-pink-200">
              <div className="flex items-center gap-2 mb-3">
                <Users className="text-purple-500" size={24} />
                <h3 className="text-xl font-bold text-purple-600">แนวทางการบริหารตามทฤษฎี</h3>
              </div>
              <p className="text-gray-700 leading-relaxed mb-4">{displayCard.guidance}</p>
              
              <div className="bg-pink-50 rounded-lg p-4 mt-4 border border-pink-200">
                <h4 className="text-pink-600 font-bold mb-2">📋 การปฏิบัติที่แนะนำ</h4>
                <p className="text-gray-700">{displayCard.action}</p>
              </div>

              {displayCard.resources && (
                <div className="bg-purple-50 rounded-lg p-4 mt-4 border border-purple-200">
                  <h4 className="text-purple-600 font-bold mb-3">📚 บทความและงานวิจัยที่เกี่ยวข้อง</h4>
                  <ul className="space-y-2">
                    {displayCard.resources.map((resource, idx) => (
                      <li key={idx}>
                        <a 
                          href={resource.url} 
                          target="_blank" 
                          rel="noopener noreferrer"
                          className="text-purple-600 hover:text-purple-800 underline text-sm flex items-start gap-2"
                        >
                          <span className="text-purple-400 mt-0.5">🔗</span>
                          <span>{resource.title}</span>
                        </a>
                      </li>
                    ))}
                  </ul>
                </div>
              )}
            </div>

            {!todayReading && (
              <div className="text-center">
                <button
                  onClick={resetReading}
                  className="bg-gradient-to-r from-pink-400 to-rose-400 hover:from-pink-500 hover:to-rose-500 text-white font-semibold px-8 py-3 rounded-full shadow-lg transform hover:scale-105 transition-all"
                >
                  🔄 จั่วไพ่ใหม่
                </button>
              </div>
            )}

            <div className="bg-pink-100/80 backdrop-blur-sm rounded-xl p-4 border-2 border-pink-200">
              <p className="text-pink-700 text-sm text-center italic">
                💡 คำแนะนำจากไพ่เป็นเพียงแนวทางในการพิจารณา ควรประยุกต์ใช้ตามบริบทของโรงเรียนและใช้ดุลยพินิจของผู้บริหารเป็นสำคัญ
              </p>
            </div>
          </div>
        )}

        <div className="mt-12 text-center">
          <p className="text-pink-600 text-sm">
            🌙 สร้างสรรค์ด้วยความปรารถนาดีต่อผู้บริหารการศึกษาไทยทุกท่าน 🌙
          </p>
        </div>
      </div>

      <style jsx>{`
        @keyframes fade-in {
          from {
            opacity: 0;
            transform: translateY(20px);
          }
          to {
            opacity: 1;
            transform: translateY(0);
          }
        }
        .animate-fade-in {
          animation: fade-in 0.6s ease-out;
        }
      `}</style>
    </div>
  );
};

export default SchoolAdminTarot;
