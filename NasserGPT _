<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NasserGPT</title>
<style>
body { font-family: Arial, sans-serif; margin:0; height:100vh; display:flex; flex-direction:column; background:#121212; color:#fff; }
header { background:#1f1f1f; color:#00bfff; padding:20px; text-align:center; font-size:28px; font-weight:bold; box-shadow:0 2px 5px rgba(0,0,0,0.5);}
#messages { flex:1; padding:20px; overflow-y:auto; display:flex; flex-direction:column;}
.message { padding:12px 18px; margin:8px 0; border-radius:12px; max-width:80%; word-wrap:break-word; font-size:16px;}
.user { background:#00bfff; color:#fff; align-self:flex-end; box-shadow:0 2px 5px rgba(0,191,255,0.4);}
.bot { background:#2c2c2c; color:#fff; align-self:flex-start; box-shadow:0 2px 5px rgba(0,0,0,0.5);}
#inputArea { display:flex; padding:12px; background:#1f1f1f; border-top:1px solid #333;}
#input { flex:1; padding:12px; border-radius:8px; border:1px solid #333; font-size:16px; background:#2c2c2c; color:#fff;}
#input::placeholder { color:#aaa; }
button { padding:12px 24px; margin-left:10px; border:none; border-radius:8px; background:#00bfff; color:#fff; font-size:16px; cursor:pointer; transition:0.3s;}
button:hover { background:#009fdf;}
body::before { content:'به ناسر چی پی تی خوش آمدید.'; position:fixed; top:10px; right:10px; font-size:20px; color:#555;}
</style>
</head>
<body>

<header>NasserGPT</header>

<div id="messages">
  <div class="message bot">Hallo! Ich bin NasserGPT, dein smarter Assistent. Du kannst mir jede Frage stellen.</div>
</div>

<div id="inputArea">
  <input type="text" id="input" placeholder="Schreibe hier...">
  <button onclick="sendMessage()">Senden</button>
</div>

<script>
// Sehr große Wissensdatenbank für Offline-Antworten
const knowledgeBase = [
  // Name und Begrüßung
  {keywords: ["wie heißt du", "wer bist du"], answer: "Ich heiße NasserGPT und ich bin dein smarter virtueller Assistent."},
  {keywords: ["hallo", "hi", "hey"], answer: "Hallo! Schön, dass du da bist! Wie kann ich dir helfen?"},
  {keywords: ["guten morgen"], answer: "Guten Morgen! Ich hoffe, du hast einen tollen Tag vor dir."},
  {keywords: ["guten abend"], answer: "Guten Abend! Wie war dein Tag?"},
  {keywords: ["tschüss", "auf wiedersehen"], answer: "Tschüss! Bis bald."},

  // Sprache
  {keywords: ["persisch"], answer: "سلام! من می‌توانم به زبان فارسی هم پاسخ دهم."},
  {keywords: ["deutsch"], answer: "Ich kann auch auf Deutsch antworten."},

  // Schule
  {keywords: ["mathe", "aufgabe"], answer: "Ich kann dir bei Mathe helfen! Zum Beispiel: 2 + 2 = 4, oder bei komplizierteren Aufgaben kann ich Schritt für Schritt erklären."},
  {keywords: ["physik", "kraft"], answer: "Physik beschäftigt sich mit Naturgesetzen. Zum Beispiel: Die Schwerkraft zieht Objekte zur Erde."},
  {keywords: ["biologie", "tier"], answer: "Biologie ist die Lehre vom Leben. Tiere und Pflanzen haben unterschiedliche Eigenschaften."},
  {keywords: ["geschichte"], answer: "Geschichte beschreibt Ereignisse der Vergangenheit. Zum Beispiel: Alexander der Große lebte vor über 2000 Jahren."},
  {keywords: ["erde", "welt"], answer: "Die Erde ist der Planet, auf dem wir leben. Sie dreht sich um die Sonne und hat Kontinente und Ozeane."},

  // Allgemeinwissen
  {keywords: ["wetter"], answer: "Das Wetter beschreibt Sonne, Regen, Wind, Temperatur. Ich kann aktuelle Daten nicht live abrufen, aber allgemeine Infos geben."},
  {keywords: ["zeit"], answer: "Die Zeit misst, wann Ereignisse passieren. 1 Stunde hat 60 Minuten."},
  {keywords: ["stadt"], answer: "Eine Stadt ist ein Ort, wo viele Menschen leben, Häuser stehen und es viele Geschäfte gibt."},
  {keywords: ["land"], answer: "Ein Land ist ein Gebiet mit eigenen Grenzen und Regierung."},

  // Mathematik
  {keywords: ["addition"], answer: "Addition bedeutet Zahlen zusammenzählen, z.B. 3 + 5 = 8."},
  {keywords: ["subtraktion"], answer: "Subtraktion bedeutet Zahlen voneinander abziehen, z.B. 10 - 4 = 6."},
  {keywords: ["multiplikation"], answer: "Multiplikation bedeutet wiederholtes Addieren, z.B. 3 * 4 = 12."},
  {keywords: ["division"], answer: "Division bedeutet Aufteilen, z.B. 12 ÷ 3 = 4."},

  // Technologie
  {keywords: ["computer", "technik"], answer: "Computer sind Geräte, die Informationen verarbeiten. Sie helfen uns beim Rechnen, Schreiben und Spielen."},
  {keywords: ["internet"], answer: "Das Internet verbindet Menschen und Geräte auf der ganzen Welt. Webseiten und Apps nutzen es."},
  {keywords: ["programmieren", "code"], answer: "Programmieren bedeutet, einem Computer Anweisungen zu geben, z.B. mit Sprachen wie Python oder JavaScript."},

  // Gesundheit
  {keywords: ["gesundheit", "körper"], answer: "Gesundheit ist sehr wichtig. Achte auf Essen, Bewegung und Schlaf."},
  {keywords: ["sport"], answer: "Sport hält dich fit. Schon 30 Minuten Bewegung am Tag helfen."},
  {keywords: ["essen", "nahrung"], answer: "Eine ausgewogene Ernährung hat Gemüse, Obst, Proteine und Kohlenhydrate."},

  // Fun / Spaß
  {keywords: ["witz", "humor"], answer: "Warum können Geister so schlecht lügen? Weil man durch sie hindurchsehen kann 😄"},
  {keywords: ["lied", "musik"], answer: "Musik macht gute Laune! Spiel deine Lieblingslieder oder sing mit."},

  // Persische Antworten
  {keywords: ["سلام"], answer: "سلام! حال شما چطور است؟"},
  {keywords: ["خوب"], answer: "خوب است! خوشحالم که می‌بینمت."},
  {keywords: ["اسم"], answer: "اسم من ناسر جی پی تی است."},

  // Platz für viele weitere Antworten...
];

// Chat-Funktion
const messages = document.getElementById('messages');
const input = document.getElementById('input');

function findAnswer(text){
  text = text.toLowerCase();
  for(const item of knowledgeBase){
    if(item.keywords.every(kw => text.includes(kw))){
      return item.answer;
    }
  }
  return "Das weiß ich leider nicht. Versuch es anders zu formulieren.";
}

async function sendMessage() {
  const text = input.value.trim();
  if(!text) return;

  const userMsg = document.createElement('div');
  userMsg.className='message user';
  userMsg.textContent=text;
  messages.appendChild(userMsg);
  input.value='';
  messages.scrollTop = messages.scrollHeight;

  const typing = document.createElement('div');
  typing.className='message bot';
  typing.textContent='NasserGPT denkt...';
  messages.appendChild(typing);
  messages.scrollTop = messages.scrollHeight;

  // Pause, damit es realistischer wirkt
  setTimeout(() => {
    const botMsg = document.createElement('div');
    botMsg.className='message bot';
    botMsg.textContent = findAnswer(text);
    typing.remove();
    messages.appendChild(botMsg);
    messages.scrollTop = messages.scrollHeight;
  }, 500);
}

input.addEventListener('keydown', function(e){
  if(e.key==='Enter') sendMessage();
});
</script>

</body>
</html>
