<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<title>زاد المؤمن</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
:root{
--green:#1e8e3e;
--bg:#f3f6f4;
--card:#ffffff;
--text:#222;
}
body.dark{
--bg:#121212;
--card:#1e1e1e;
--text:#eee;
}
body{
margin:0;
background:var(--bg);
color:var(--text);
font-family:Tajawal,Segoe UI;
}
header{
background:var(--green);
color:#fff;
padding:14px;
text-align:center;
font-size:22px;
}
.tabs{
display:flex;
overflow-x:auto;
}
.tab{
padding:8px 14px;
margin:6px;
border-radius:20px;
background:#ccc;
cursor:pointer;
white-space:nowrap;
}
.tab.active{
background:var(--green);
color:#fff;
}
.card{
background:var(--card);
margin:16px;
padding:20px;
border-radius:14px;
font-size:18px;
text-align:center;
line-height:1.8;
}
.repeat{
margin-top:10px;
font-size:14px;
color:#666;
}
.nav{
display:flex;
justify-content:space-between;
align-items:center;
margin:10px 16px;
}
.nav button{
background:var(--green);
color:#fff;
border:none;
border-radius:8px;
padding:8px 14px;
}
.counter{
font-size:15px;
}
</style>
</head>
<body>

<header>زاد المؤمن</header>

<div class="tabs">
<div class="tab active" onclick="changeGroup('morning',this)">أذكار الصباح</div>
<div class="tab" onclick="changeGroup('evening',this)">أذكار المساء</div>
</div>

<div class="card" id="zekr"></div>
<div class="repeat" id="repeat"></div>

<div class="nav">
<button onclick="prev()">السابق</button>
<div class="counter" id="count"></div>
<button onclick="next()">التالي</button>
</div>

<script>
const azkar={
morning:[
{ text:`أَصْبَحْنا وَأَصْبَحَ المُلْكُ لله وَالحَمدُ لله ، لا إله إلا اللهُ وَحدَهُ لا شَرِيكَ لهُ، له الملك وله الحمد، وهُوَ على كل شَيءٍ قدير ، رَبِّ أَسْأَلُكَ خَيرَ ما في هذا اليوم وَخَيرَ ما بَعْدَه ، وَأَعوذُ بِكَ مِنْ شَرِّ هذا اليوم وَشَرِّ ما بَعْدَه، رَبِّ أَعوذُ بِكَ مِنَ الْكَسَلِ وَسوءِ الْكِبَر ، رَبِّ أَعوذُ بِكَ مِنْ عَذَابٍ في النَّارِ وَعَذَابٍ في القبر.` },
{ text:`اللَّهُمَّ بِكَ أَصْبَحْنا وَبِكَ أَمْسَينا ، وَبِكَ نَحْيا وَبِكَ نَمُوتُ وَإِلَيْكَ النُّشُور.` },
{ text:`اللَّهُمَّ أَنْتَ رَبِّي لَا إِلَهَ إِلَّا أَنْتَ خَلَقْتَني وَأَنا عَبْدُكَ ، وَأَنا عَلَى عَهْدِكَ وَوَعْدِكَ ما اسْتَطَعْت ، أَعوذُ بِكَ مِنْ شَرِّ ما صَنَعْت ، أَبوءُ لَكَ بِنِعْمَتِكَ عَلَيَّ وَأَبوءُ بِذَنْبِي فَاغْفِرْ لِي فَإِنَّهُ لا يَغْفِرُ الذُّنُوبَ إِلَّا أَنْتَ.` },
{ text:`اللَّهُمَّ إِنِّي أَصْبَحْتُ أَشْهِدُك ... وَأَنَّ مُحَمَّداً عَبْدُكَ وَرَسولُك .`, repeat:4 },
{ text:`اللَّهُمَّ مَا أَصْبَحَ بي مِنْ نِعْمَةٍ ... فَلَكَ الْحَمْدُ وَلَكَ الشُّكْر.` },
{ text:`اللهم عافني في بدني ... لَا إِلَهَ إِلَّا أَنْتَ`, repeat:3 },
{ text:`حَسْبِيَ اللهُ لا إلهَ إِلَّا هُوَ ...`, repeat:7 },
{ text:`أعوذُ بِكَلِماتِ اللهِ التَّامَّاتِ مِنْ شَرِّ ما خَلَق .`, repeat:3 },
{ text:`اللَّهُمَّ إِنِّي أَسْأَلُكَ العَفْوَ وَالعافية ...` },
{ text:`اللَّهُمَّ عالِمَ الغَيْبِ وَالشَّهَادَةِ ...` },
{ text:`بسم ﷲ الذي لا يَضُرُّ ...`, repeat:3 },
{ text:`رضيتُ باللهِ رَبَّاً ...`, repeat:3 },
{ text:`سُبْحانَ اللهِ وَبِحَمْدِهِ عَدَدَ خَلْقِه ...`, repeat:3 },
{ text:`سُبْحانَ اللهِ وَبِحَمْدِهِ .`, repeat:100 },
{ text:`يا حَيُّ يَا قَيُّومُ ...` },
{ text:`لا إله إلا الله وحْدَهُ ...`, repeat:100 },
{ text:`أَصْبَحْنا وَأَصْبَحْ المُلكُ للهِ ...` }
],
evening:[
{ text:`أَمْسَيْنا وَأَمْسى الملك لله وَالحَمد لله ...` },
{ text:`اللَّهُمَّ بِكَ أَمْسَينا ...` },
{ text:`اللَّهُمَّ أَنْتَ رَبِّي لَا إِلَهَ إِلَّا أَنْتَ ...` },
{ text:`اللَّهُمَّ إِنِّي أَمسيتُ أَشْهِدُك ...`, repeat:4 },
{ text:`اللَّهُمَّ مَا أَمْسَى بِي مِنْ نِعْمَةٍ ...` },
{ text:`اللَّهُمَّ عَافِنِي فِي بَدَنِي ...`, repeat:3 },
{ text:`حَسْبِيَ اللَّهُ لَا إِلَٰهَ إِلَّا هُوَ ...`, repeat:7 },
{ text:`أَعُوذُ بِكَلِمَاتِ اللَّهِ التَّامَّاتِ ...`, repeat:3 },
{ text:`اللَّهُمَّ إِنِّي أَسْأَلُكَ الْعَفْوَ وَالْعَافِيَةَ ...` },
{ text:`اللَّهُمَّ عالِمَ الغَيْبِ وَالشَّهَادَةِ ...` },
{ text:`بِسْمِ اللَّهِ الَّذِي لَا يَضُرُّ ...`, repeat:3 }
]
};

let group="morning", i=0;

function show(){
const item=azkar[group][i];
document.getElementById("zekr").innerText=item.text;
document.getElementById("repeat").innerText=item.repeat?`يُقال ${item.repeat} مرات`:"";
document.getElementById("count").innerText=`${azkar[group].length} / ${i+1}`;
}
function next(){ i=(i+1)%azkar[group].length; show(); }
function prev(){ i=(i-1+azkar[group].length)%azkar[group].length; show(); }
function changeGroup(g,el){
group=g; i=0;
document.querySelectorAll(".tab").forEach(t=>t.classList.remove("active"));
el.classList.add("active");
show();
}
show();
</script>

</body>
</html>
