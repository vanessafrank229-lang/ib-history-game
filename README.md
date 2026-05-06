<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>IB History DP1 Flashcard Game</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f4f4f4;
      padding: 30px;
      text-align: center;
    }
    .game-box {
      background: white;
      max-width: 800px;
      margin: auto;
      padding: 30px;
      border-radius: 15px;
      box-shadow: 0 0 15px rgba(0,0,0,0.15);
    }
    h1 {
      margin-bottom: 10px;
    }
    .timer {
      font-size: 24px;
      font-weight: bold;
      color: #c0392b;
      margin: 15px;
    }
    .question {
      font-size: 24px;
      margin: 25px 0;
      font-weight: bold;
    }
    .answer {
      display: none;
      font-size: 20px;
      background: #eef6ff;
      padding: 20px;
      border-radius: 10px;
      margin: 20px 0;
    }
    button {
      padding: 12px 20px;
      margin: 8px;
      border: none;
      border-radius: 8px;
      background: #2c7be5;
      color: white;
      font-size: 16px;
      cursor: pointer;
    }
    button:hover {
      background: #1a5fb4;
    }
    .correct {
      background: #27ae60;
    }
    .wrong {
      background: #e74c3c;
    }
    .skip {
      background: #f39c12;
    }
    .restart {
      background: #8e44ad;
    }
    .score {
      margin-top: 15px;
      font-size: 18px;
    }
    .progress {
      margin-top: 10px;
      font-size: 16px;
      color: #555;
    }
  </style>
</head>
<body>
<div class="game-box">
  <h1>IB History DP1 Flashcard Game</h1>
  <p>Random order every time. Try to answer before revealing.</p>
  <div class="timer" id="timer">Time: 05:00</div>
  <div class="progress" id="progress"></div>
  <div class="question" id="question"></div>
  <div class="answer" id="answer"></div>
  <button onclick="showAnswer()">Show Answer</button>
  <button class="correct" onclick="markCorrect()">I got it right</button>
  <button class="wrong" onclick="markWrong()">I got it wrong</button>
  <button class="skip" onclick="skipCard()">Skip</button>
  <button class="restart" onclick="restartGame()">Restart</button>
  <div class="score" id="score"></div>
</div>
<script>
const flashcards = [
  {
    q: "Why did Japan expand in the 1930s?",
    a: "Japan lacked natural resources, faced economic problems after the Great Depression, had a growing population, and militarists believed Japan deserved dominance in Asia."
  },
  {
    q: "What was the Mukden Incident in 1931?",
    a: "Japanese soldiers blew up part of a railway in Manchuria and blamed China. Japan used this as an excuse to invade Manchuria."
  },
  {
    q: "Why was Manchuria important to Japan?",
    a: "Manchuria had coal, iron, industry, land, and resources Japan needed for expansion."
  },
  {
    q: "What was Manchukuo?",
    a: "A puppet state created by Japan in Manchuria in 1932 with Puyi as ruler."
  },
  {
    q: "Why was the League of Nations weak in Manchuria?",
    a: "The League reacted slowly, condemned Japan, but took no strong action. Japan simply left the League in 1933."
  },
  {
    q: "What was the Second Sino-Japanese War?",
    a: "A full-scale war between Japan and China beginning in 1937 after the Marco Polo Bridge Incident."
  },
  {
    q: "What happened during the Nanjing Massacre?",
    a: "Japanese troops killed and raped civilians after capturing Nanjing in 1937. Around 200,000 to 300,000 people died."
  },
  {
    q: "Why did Japan attack Pearl Harbor?",
    a: "Japan wanted to weaken the US Pacific Fleet after American sanctions and oil embargoes."
  },
  {
    q: "Who was Akira Iriye?",
    a: "A historian who argued Japan expanded due to economic pressure and fear of Western domination."
  },
  {
    q: "Who was E.H. Norman?",
    a: "A historian who believed Japanese militarism and nationalism caused aggression."
  },
  {
    q: "Who was Mussolini?",
    a: "Mussolini was the leader of Fascist Italy. He came to power in 1922."
  },
  {
    q: "Why did Mussolini expand Italy?",
    a: "He wanted prestige, a new Roman Empire, and a way to distract Italians from domestic problems."
  },
  {
    q: "What was the Corfu Incident in 1923?",
    a: "Italy occupied Corfu after Italian officials were killed in Greece."
  },
  {
    q: "Why did Italy invade Abyssinia?",
    a: "Italy wanted to build an empire and revenge its defeat at Adowa in 1896."
  },
  {
    q: "Why did the League fail in Abyssinia?",
    a: "Sanctions were weak, and Britain and France avoided strong action against Mussolini."
  },
  {
    q: "What was the Hoare-Laval Pact?",
    a: "A secret British-French plan to give much of Ethiopia to Italy."
  },
  {
    q: "What was the Rome-Berlin Axis?",
    a: "An alliance between Mussolini and Hitler formed in 1936."
  },
  {
    q: "Who was Denis Mack Smith?",
    a: "A historian who argued Mussolini was driven by prestige and opportunism."
  },
  {
    q: "Who was Richard Bosworth?",
    a: "A historian who emphasized propaganda and domestic politics in Mussolini’s expansion."
  },
  {
    q: "What were Hitler’s main goals?",
    a: "Destroy the Treaty of Versailles, unite Germans, expand territory, and destroy communism."
  },
  {
    q: "What was German rearmament?",
    a: "Hitler rebuilt Germany’s military in violation of the Treaty of Versailles."
  },
  {
    q: "Why was the Rhineland important?",
    a: "Hitler remilitarized it in 1936. Britain and France did nothing, which encouraged him further."
  },
  {
    q: "What was Anschluss?",
    a: "Germany annexed Austria in 1938."
  },
  {
    q: "What was the Munich Agreement?",
    a: "Britain and France allowed Hitler to take the Sudetenland from Czechoslovakia in 1938."
  },
  {
    q: "What was appeasement?",
    a: "Appeasement was Britain and France giving in to Hitler’s demands to avoid war."
  },
  {
    q: "Why did appeasement fail?",
    a: "It encouraged Hitler and allowed Germany to grow stronger."
  },
  {
    q: "What was the Nazi-Soviet Pact?",
    a: "A 1939 non-aggression pact between Hitler and Stalin, including secret plans to divide Poland."
  },
  {
    q: "What happened on 1 September 1939?",
    a: "Germany invaded Poland, beginning World War II."
  },
  {
    q: "Who was A.J.P. Taylor?",
    a: "A historian who argued Hitler was opportunistic rather than following a fixed master plan."
  },
  {
    q: "Who was Ian Kershaw?",
    a: "A historian who argued Hitler’s ideology drove expansion."
  },
  {
    q: "Who was Richard Evans?",
    a: "A historian who emphasized Nazism and racial ideology as key causes of aggression."
  },
  {
    q: "What were the long-term causes of WWII?",
    a: "Versailles resentment, dictators, nationalism, militarism, economic instability, and League weakness."
  },
  {
    q: "What were the short-term causes of WWII?",
    a: "Abyssinia, Rhineland, Anschluss, Munich, Nazi-Soviet Pact, and invasion of Poland."
  },
  {
    q: "Who fought in the Spanish Civil War?",
    a: "Republicans fought against Nationalists led by Franco."
  },
  {
    q: "Who supported the Republicans?",
    a: "The USSR and International Brigades supported the Republicans."
  },
  {
    q: "Who supported Franco?",
    a: "Germany and Italy supported Franco’s Nationalists."
  },
  {
    q: "What was Guernica?",
    a: "A Spanish town bombed by German aircraft in 1937. It became a symbol of civilian suffering."
  },
  {
    q: "What were the consequences of the Spanish Civil War?",
    a: "Franco ruled Spain until 1975, and Germany tested weapons and tactics."
  },
  {
    q: "Who was Paul Preston?",
    a: "A historian who focused on brutality in the Spanish Civil War."
  },
  {
    q: "Who was Hugh Thomas?",
    a: "A historian known for detailed analysis of the Spanish Civil War."
  },
  {
    q: "Who fought in the Chinese Civil War?",
    a: "The CCP led by Mao Zedong fought the KMT led by Chiang Kai-shek."
  },
  {
    q: "What was the Long March?",
    a: "A retreat by the Chinese Communists from 1934 to 1935 that helped Mao become leader."
  },
  {
    q: "Why did the CCP win the Chinese Civil War?",
    a: "Peasant support, guerrilla warfare, KMT corruption, and strong leadership."
  },
  {
    q: "What happened in China in 1949?",
    a: "Mao established the People’s Republic of China, while the KMT fled to Taiwan."
  },
  {
    q: "What were the main causes of WWI?",
    a: "Militarism, Alliances, Imperialism, and Nationalism. Remember MAIN."
  },
  {
    q: "Who was Franz Ferdinand?",
    a: "The Austro-Hungarian Archduke assassinated in 1914, triggering World War I."
  },
  {
    q: "What was trench warfare?",
    a: "Static warfare using trenches, with mud, disease, shell shock, and stalemate."
  },
  {
    q: "What was the Treaty of Versailles?",
    a: "The 1919 treaty blaming Germany for WWI and imposing reparations and military restrictions."
  },
  {
    q: "Who was Fritz Fischer?",
    a: "A historian who argued Germany was mainly responsible for World War I."
  },
  {
    q: "Who was Christopher Clark?",
    a: "A historian who argued multiple powers shared responsibility for World War I."
  },
  {
    q: "What were key features of WWII?",
    a: "Total war, genocide, strategic bombing, propaganda, and civilian targeting."
  },
  {
    q: "What was the Holocaust?",
    a: "The systematic murder of around 6 million Jews by Nazi Germany."
  },
  {
    q: "What happened in Hiroshima and Nagasaki?",
    a: "The US dropped atomic bombs in August 1945, leading to Japan’s surrender."
  },
  {
    q: "What were the consequences of WWII?",
    a: "Creation of the UN, Cold War, weakened Europe, rise of the USA and USSR, and decolonization."
  },
  {
    q: "What similarities existed between Hitler and Mussolini?",
    a: "Both used dictatorship, nationalism, propaganda, and militarism."
  },
  {
    q: "How was Hitler different from Mussolini?",
    a: "Hitler was more ideological and racially driven."
  },
  {
    q: "What similarities existed between German and Japanese expansion?",
    a: "Economic motives, militarism, nationalism, and weak League response."
  },
  {
    q: "What was different about Japanese expansion?",
    a: "Japan focused mainly on Asia and resources."
  },
  {
    q: "What was different about German expansion?",
    a: "Germany focused on Europe and racial expansion."
  }
];
let shuffledCards = [];
let currentIndex = 0;
let correct = 0;
let wrong = 0;
let skipped = 0;
let totalTime = 5 * 60;
let timerInterval;
function shuffle(array) {
  let copy = [...array];
  for (let i = copy.length - 1; i > 0; i--) {
    let randomIndex = Math.floor(Math.random() * (i + 1));
    [copy[i], copy[randomIndex]] = [copy[randomIndex], copy[i]];
  }
  return copy;
}
function startGame() {
  shuffledCards = shuffle(flashcards);
  currentIndex = 0;
  correct = 0;
  wrong = 0;
  skipped = 0;
  totalTime = 5 * 60;
  clearInterval(timerInterval);
  timerInterval = setInterval(updateTimer, 1000);
  showCard();
}
function showCard() {
  if (currentIndex >= shuffledCards.length) {
    endGame();
    return;
  }
  document.getElementById("question").innerText = shuffledCards[currentIndex].q;
  document.getElementById("answer").innerText = shuffledCards[currentIndex].a;
  document.getElementById("answer").style.display = "none";
  document.getElementById("progress").innerText =
    `Card ${currentIndex + 1} of ${shuffledCards.length}`;
  updateScore();
}
function showAnswer() {
  document.getElementById("answer").style.display = "block";
}
function markCorrect() {
  correct++;
  currentIndex++;
  showCard();
}
function markWrong() {
  wrong++;
  currentIndex++;
  showCard();
}
function skipCard() {
  skipped++;
  currentIndex++;
  showCard();
}
function updateScore() {
  document.getElementById("score").innerText =
    `Correct: ${correct} | Wrong: ${wrong} | Skipped: ${skipped}`;
}
function updateTimer() {
  totalTime--;
  let minutes = Math.floor(totalTime / 60);
  let seconds = totalTime % 60;
  document.getElementById("timer").innerText =
    `Time: ${String(minutes).padStart(2, "0")}:${String(seconds).padStart(2, "0")}`;
  if (totalTime <= 0) {
    endGame();
  }
}
function endGame() {
  clearInterval(timerInterval);
  let totalAnswered = correct + wrong;
  let accuracy = totalAnswered > 0
    ? Math.round((correct / totalAnswered) * 100)
    : 0;
  document.getElementById("question").innerText = "Game Over!";
  document.getElementById("answer").style.display = "block";
  document.getElementById("answer").innerHTML =
    `Final Score:<br><br>
    Correct: ${correct}<br>
    Wrong: ${wrong}<br>
    Skipped: ${skipped}<br>
    Accuracy: ${accuracy}%<br><br>
    Restart to try a new random order.`;
  document.getElementById("progress").innerText = "";
}
function restartGame() {
  startGame();
}
startGame();
</script>
</body>
</html>
