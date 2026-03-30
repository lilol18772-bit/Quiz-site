# Quiz-site

<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<title>اختبار السيرة النبوية</title>
<style>
body {
    font-family: Tahoma;
    background: #f4f6f8;
}
.container {
    max-width: 800px;
    margin: auto;
    padding: 20px;
}
.card {
    background: white;
    padding: 20px;
    border-radius: 12px;
    margin-top: 20px;
    box-shadow: 0 4px 10px rgba(0,0,0,0.1);
}
button {
    padding: 10px 20px;
    border: none;
    background: #4CAF50;
    color: white;
    border-radius: 8px;
    cursor: pointer;
}
.question {
    margin-bottom: 20px;
}
.result {
    font-weight: bold;
    margin-top: 20px;
}
</style>
</head>

<body>

<div class="container">
<h1>اختبار السيرة النبوية</h1>

<div class="card" id="levelSelect">
<h3>اختاري المستوى</h3>
<button onclick="startQuiz('easy')">سهل</button>
<button onclick="startQuiz('medium')">متوسط</button>
<button onclick="startQuiz('hard')">صعب</button>
</div>

<div class="card" id="quiz" style="display:none;">
<form id="quizForm"></form>
<button onclick="submitQuiz()">إرسال</button>
<div class="result" id="result"></div>
</div>

</div>

<script>

const questions = {
easy: [
["ما أول غزوة خاضها النبي؟", ["بدر","أحد","تبوك","حنين"], "بدر"],
["في أي غزوة حفر المسلمون خندقًا؟", ["أحد","الخندق","بدر","خيبر"], "الخندق"],
["في أي غزوة خالف الرماة أمر النبي؟", ["بدر","أحد","حنين","تبوك"], "أحد"],
["ما آخر غزوة خرج فيها النبي؟", ["بدر","تبوك","خيبر","حنين"], "تبوك"],
["من هو الصحابي الذي كان النبي يستحي منه؟", ["علي","عثمان","عمر","أبو بكر"], "عثمان"],
["من لقب بسيف الله المسلول؟", ["علي","خالد","عمر","حمزة"], "خالد"],
["أول امرأة آمنت؟", ["عائشة","خديجة","فاطمة","حفصة"], "خديجة"],
["براءة حادثة الإفك؟", ["حفصة","عائشة","أسماء","زينب"], "عائشة"],
["أول شهيدة؟", ["سمية","خديجة","فاطمة","صفية"], "سمية"],
["اسم الغار؟", ["حراء","ثور","أحد","النور"], "ثور"],
["أعمار أهل الجنة؟", ["25","30","33","40"], "33"],
["استشهد في أحد؟", ["حمزة","علي","عمر","زيد"], "حمزة"],
["أول زوجات النبي؟", ["عائشة","حفصة","خديجة","صفية"], "خديجة"]
],

medium: [
["فتح مكة كان في؟", ["بدر","فتح مكة","أحد","حنين"], "فتح مكة"],
["غزوة العسرة؟", ["تبوك","بدر","خيبر","أحد"], "تبوك"],
["يوم الفرقان؟", ["بدر","أحد","حنين","تبوك"], "بدر"],
["سنة بدر؟", ["1","2","3","4"], "2"],
["عدد الغزوات؟", ["20","27","30","15"], "27"],
["غسلته الملائكة؟", ["حنظلة","علي","عمر","بلال"], "حنظلة"],
["أشار بحفر الخندق؟", ["سلمان","عمر","علي","أبو بكر"], "سلمان"],
["أول من جهر؟", ["ابن مسعود","بلال","عمر","علي"], "ابن مسعود"],
["ذات النطاقين؟", ["أسماء","فاطمة","عائشة","حفصة"], "أسماء"],
["أم المساكين؟", ["زينب","عائشة","حفصة","صفية"], "زينب"],
["سورة البراءة؟", ["النور","البقرة","النساء","التوبة"], "النور"],
["بعد الغزوات؟", ["ينام","يشكر الله","يترك","يغضب"], "يشكر الله"],
["أعمار أهل الجنة؟", ["33","40","20","25"], "33"]
],

hard: [
["سورة الحشر نزلت في؟", ["بني النضير","بدر","أحد","تبوك"], "بني النضير"],
["قتل أبا جهل؟", ["معاذ","عمر","علي","خالد"], "معاذ"],
["اهتز له العرش؟", ["سعد","حمزة","علي","بلال"], "سعد"],
["خطيبة النساء؟", ["أسماء بنت يزيد","عائشة","حفصة","فاطمة"], "أسماء بنت يزيد"],
["سبب إسلام عمر؟", ["فاطمة","عائشة","أسماء","زينب"], "فاطمة"],
["شُق صدره؟", ["محمد","موسى","عيسى","إبراهيم"], "محمد"],
["سبب حب الصحابة؟", ["أخلاقه","ماله","قوته","نسبه"], "أخلاقه"],
["سنة مهجورة؟", ["السواك","النوم كثير","الكسل","الصمت"], "السواك"]
]
};

let current = [];

function startQuiz(level){
document.getElementById("levelSelect").style.display="none";
document.getElementById("quiz").style.display="block";

current = questions[level];
let form = document.getElementById("quizForm");
form.innerHTML = "";

current.forEach((q,i)=>{
form.innerHTML += `<div class="question"><p>${i+1}- ${q[0]}</p>`;
q[1].forEach(opt=>{
form.innerHTML += `
<label>
<input type="radio" name="q${i}" value="${opt}">
${opt}
</label><br>`;
});
form.innerHTML += `</div>`;
});
}

function submitQuiz(){
let score = 0;

current.forEach((q,i)=>{
let selected = document.querySelector(`input[name="q${i}"]:checked`);
if(selected && selected.value === q[2]){
score++;
}
});

document.getElementById("result").innerText =
"درجتك: " + score + " / " + current.length;
}
</script>

</body>
body {
    font-family: Tahoma;
    background: linear-gradient(135deg, #667eea, #764ba2);
    color: #333;
}

.card {
    background: white;
    padding: 20px;
    border-radius: 15px;
    margin-top: 20px;
    box-shadow: 0 10px 25px rgba(0,0,0,0.2);
    animation: fadeIn 0.5s ease;
}

button {
    background: linear-gradient(45deg, #ff6a00, #ee0979);
    font-weight: bold;
    transition: 0.3s;
}

button:hover {
    transform: scale(1.05);
}

@keyframes fadeIn {
    from {opacity:0; transform:translateY(20px);}
    to {opacity:1; transform:translateY(0);}
}
</html>
