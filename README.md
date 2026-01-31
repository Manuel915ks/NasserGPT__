<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NasserGPT</title>
<style>
body { font-family: Arial, sans-serif; margin:0; height:100vh; display:flex; flex-direction:column; background:#121212; color:#fff; }
header { background:#1f1f1f; color:#00bfff; padding:20px; text-align:center; font-size:28px; font-weight:bold; }
#messages { flex:1; padding:20px; overflow-y:auto; display:flex; flex-direction:column;}
.message { padding:12px 18px; margin:8px 0; border-radius:12px; max-width:80%; word-wrap:break-word; font-size:16px;}
.user { background:#00bfff; color:#fff; align-self:flex-end;}
.bot { background:#2c2c2c; color:#fff; align-self:flex-start;}
#inputArea { display:flex; padding:12px; background:#1f1f1f; border-top:1px solid #333;}
#input { flex:1; padding:12px; border-radius:8px; border:1px solid #333; font-size:16px; background:#2c2c2c; color:#fff;}
#input::placeholder { color:#aaa; }
button { padding:12px 24px; margin-left:10px; border:none; border-radius:8px; background:#00bfff; color:#fff; font-size:16px;}
</style>
</head>
<body>

<header>NasserGPT</header>

<div id="messages">
  <div class="message bot">Hallo! Ich bin NasserGPT. Frage mich etwas – ich gebe mein Bestes!</div>
</div>

<div id="inputArea">
  <input type="text" id="input" placeholder="Schreibe hier...">
  <button onclick="sendMessage()">Senden</button>
</div>

<script>
const messages = document.getElementById('messages');
const input = document.getElementById('input');

// Größere Wissensdatenbank
const knowledgeBase = [
  // Begrüßung & Identität
  {keywords: ["wie heißt du", "wer bist du"], answer: "Ich heiße NasserGPT, dein Offline‑Assistent mit viel Wissen!"},

  // Alltag & Smalltalk
  {keywords: ["hallo","hi","hey"], answer: "Hallo! Wie kann ich dir helfen?"},
  {keywords: ["guten morgen"], answer: "Guten Morgen! Ich wünsche dir einen tollen Tag!"},
  {keywords: ["tschüss","auf wiedersehen"], answer: "Tschüss! Bis bald!"},
  {keywords: ["danke"], answer: "Gern geschehen! Sag einfach, wenn du noch etwas wissen willst."},

  // Sprache
  {keywords: ["sprache","persisch"], answer: "Ja, ich kann sowohl Deutsch als auch Persisch verstehen und antworten."},
  {keywords: ["übersetzen", "übersetzung"], answer: "Sag mir einen Satz und ich übersetze ihn für dich."},

  // Mathe
  {keywords: ["was ist 2+2", "2+2"], answer: "2 + 2 = 4."},
  {keywords: ["addition"], answer: "Addition bedeutet Zahlen zusammenzählen, z.B. 5 + 3 = 8."},
  {keywords: ["multiplikation"], answer: "Multiplikation ist wiederholte Addition, z.B. 4 × 3 = 12."},
  {keywords: ["division"], answer: "Division teilt Zahlen auf, z.B. 12 ÷ 3 = 4."},
  {keywords: ["subtraktion"], answer: "Subtraktion bedeutet abziehen, z.B. 10 - 4 = 6."},

  // Physik
  {keywords: ["was ist energie"], answer: "Energie ist die Fähigkeit Arbeit zu verrichten, z.B. kinetische oder potenzielle Energie."},
  {keywords: ["kraft"], answer: "Kraft ist ein Einfluss, der Bewegungen ändert. Einheit ist Newton (N)."},
  {keywords: ["geschwindigkeit"], answer: "Geschwindigkeit beschreibt, wie schnell etwas ist (Meter pro Sekunde)."},
  {keywords: ["gravitation"], answer: "Die Gravitation ist die Anziehungskraft zwischen Massen, z.B. Erdanziehung."},

  // Biologie
  {keywords: ["zelle"], answer: "Die Zelle ist die kleinste lebende Einheit in Organismen."},
  {keywords: ["dna"], answer: "DNA enthält genetische Informationen, die Bauanleitung des Lebens."},

  // Geschichte
  {keywords: ["zweiter weltkrieg"], answer: "Der Zweite Weltkrieg war 1939–1945, ein globaler Konflikt mit vielen Nationen beteiligt."},
  {keywords: ["geschichte"], answer: "Geschichte beschreibt vergangene Ereignisse und Menschen."},

  // Geographie
  {keywords: ["erde"], answer: "Die Erde ist unser Planet mit Kontinenten, Ozeanen und einer Atmosphäre."},
  {keywords: ["kontinent"], answer: "Kontinente sind große Landmassen wie Europa, Afrika, Asien."},

  // Technologie
  {keywords: ["computer"], answer: "Ein Computer ist ein Gerät, das Daten verarbeitet und Programme ausführt."},
  {keywords: ["internet"], answer: "Das Internet verbindet Computer weltweit für Kommunikation und Daten."},
  {keywords: ["programmieren"], answer: "Programmieren heißt, einem Computer Anweisungen in einer Programmiersprache zu geben."},

  // Gesundheit
  {keywords: ["gesundheit"], answer: "Gesundheit ist wichtig: Achte auf Ernährung, Bewegung und Schlaf."},
  {keywords: ["ernährung"], answer: "Eine gesunde Ernährung umfasst Obst, Gemüse, Proteine und ausreichend Wasser."},
  {keywords: ["schlaf"], answer: "Ausreichender Schlaf ist wichtig für Körper und Geist."},

  // Fun / Trivia
  {keywords: ["witz"], answer: "Warum können Geister so schlecht lügen? Weil man durch sie hindurchsehen kann 😄"},
  {keywords: ["fakt"], answer: "Wusstest du? Bienen können tanzen, um anderen Bienen Informationen zu geben."},

  // Sprache Persian
  {keywords: ["سلام"], answer: "سلام! چطور می‌توانم کمکت کنم؟"},
  {keywords: ["اسم"], answer: "اسم من NasserGPT هست."},
  {keywords: ["چطوری"], answer: "من خوبم، مرسی! تو چطوری؟"},

  // … du kannst nachher noch viel mehr hinzufügen
];

// Antwort finden
function findAnswer(text) {
  const t = text.toLowerCase();
  for (const item of knowledgeBase) {
    if (item.keywords.every(kw => t.includes(kw))) {
      return item.answer;
    }
  }
  return "Tut mir leid, darauf habe ich noch keine Antwort. Versuch es anders zu fragen.";
}

function sendMessage() {
  const text = input.value.trim();
  if (!text) return;

  const userMsg = document.createElement('div');
  userMsg.className = 'message user';
  userMsg.textContent = text;
  messages.appendChild(userMsg);
  input.value = '';
  messages.scrollTop = messages.scrollHeight;

  const botThinking = document.createElement('div');
  botThinking.className = 'message bot';
  botThinking.textContent = 'NasserGPT denkt...';
  messages.appendChild(botThinking);
  messages.scrollTop = messages.scrollHeight;

  setTimeout(() => {
    const answer = findAnswer(text);
    const botMsg = document.createElement('div');
    botMsg.className = 'message bot';
    botMsg.textContent = answer;
    messages.removeChild(botThinking);
    messages.appendChild(botMsg);
    messages.scrollTop = messages.scrollHeight;
  }, 600);
}

input.addEventListener('keydown', e => {
  if (e.key === 'Enter') sendMessage();
});
</script>

</body>
</html>
