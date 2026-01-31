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
  <div class="message bot">Hallo! Ich bin NasserGPT. Du kannst mir jede Frage stellen.</div>
</div>

<div id="inputArea">
  <input type="text" id="input" placeholder="Schreibe hier...">
  <button onclick="sendMessage()">Senden</button>
</div>

<script>
// Vorprogrammierte Datenbank
const knowledgeBase = [
  {keywords: ["wie", "geht", "es"], answer: "Mir geht es gut, danke der Nachfrage!"},
  {keywords: ["wer", "bist", "du"], answer: "Ich bin NasserGPT, dein smarter virtueller Assistent."},
  {keywords: ["persisch", "sprache"], answer: "Ich kann sowohl Deutsch als auch Persisch verstehen und antworten."},
  {keywords: ["schule", "aufgabe"], answer: "Ich kann dir bei deinen Schulaufgaben helfen, erkläre alles verständlich."},
  {keywords: ["wetter"], answer: "Das aktuelle Wetter kann ich nicht live abrufen, aber ich kann allgemeine Infos geben."},
  {keywords: ["python"], answer: "Python ist eine Programmiersprache, die leicht zu lernen ist und für viele Aufgaben genutzt wird."},
  {keywords: ["physik"], answer: "Physik ist die Lehre von Materie, Energie und den Naturgesetzen."},
  {keywords: ["geschwindigkeit"], answer: "Geschwindigkeit beschreibt, wie schnell sich etwas bewegt."},
  {keywords: ["mathe"], answer: "In Mathe kann ich Aufgaben erklären, Beispiele geben und Formeln erläutern."},
  {keywords: ["gesundheit"], answer: "Gesundheit ist sehr wichtig, achte auf Ernährung, Bewegung und Schlaf."},
  {keywords: ["nasser"], answer: "Das ist mein Name – NasserGPT, dein smarter Assistent."},
  {keywords: ["hallo"], answer: "Hallo! Schön, dass du da bist!"},
  {keywords: ["persisch"], answer: "سلام! من می‌توانم به زبان فارسی هم پاسخ دهم."},
  {keywords: ["deutsch"], answer: "Ich kann auch auf Deutsch antworten."},
  // Hier kannst du **beliebig viele Fragen + Antworten** einfügen
];

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

  // Kleine Pause, damit es realistischer wirkt
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
