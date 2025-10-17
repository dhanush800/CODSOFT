<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Recommendation System - CodSoft AI Intern</title>
<style>
  body {
    font-family: Arial, sans-serif;
    background: #121212;
    color: #fff;
    padding: 30px;
  }

  h2 {
    color: #00bfa5;
    text-align: center;
  }

  .cards-container, .recommendations-container {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    gap: 15px;
    margin-top: 20px;
  }

  .card {
    background: linear-gradient(135deg, #1e1e1e, #2a2a2a);
    border-radius: 15px;
    padding: 15px;
    text-align: center;
    cursor: pointer;
    transition: 0.3s;
    box-shadow: 0 5px 15px rgba(0,0,0,0.5);
  }

  .card:hover {
    transform: scale(1.05);
    background: linear-gradient(135deg, #004d40, #00695c);
  }

  .card.selected {
    border: 3px solid #00bfa5;
  }

  .tag {
    display: inline-block;
    margin: 5px 3px;
    padding: 5px 10px;
    background: #00796b;
    border-radius: 12px;
    font-size: 12px;
  }

  #getRec {
    display: block;
    margin: 25px auto;
    padding: 12px 20px;
    font-size: 18px;
    border: none;
    border-radius: 12px;
    background: #00bfa5;
    cursor: pointer;
    transition: 0.3s;
  }

  #getRec:hover {
    background: #00796b;
  }

  .recommendations-container .card {
    border: 2px solid #00bfa5;
  }

</style>
</head>
<body>

<h2>Advanced Recommendation System</h2>
<p style="text-align:center;">Select items you like and click "Get Recommendations"</p>

<div class="cards-container" id="cards"></div>
<button id="getRec">Get Recommendations</button>

<h2 style="margin-top:40px;">Recommended Items</h2>
<div class="recommendations-container" id="recommendations"></div>

<script>
const dataset = [
  {name:"The Alchemist", tags:["fiction","adventure","philosophy"]},
  {name:"Atomic Habits", tags:["self-help","productivity"]},
  {name:"1984", tags:["fiction","dystopia","classic"]},
  {name:"The Power of Habit", tags:["self-help","productivity"]},
  {name:"Sapiens", tags:["history","non-fiction"]},
  {name:"The Hobbit", tags:["fiction","fantasy","adventure"]},
  {name:"Clean Code", tags:["programming","self-help"]},
  {name:"Meditations", tags:["philosophy","classic"]},
  {name:"Harry Potter", tags:["fiction","fantasy","adventure"]},
  {name:"The Lean Startup", tags:["business","productivity"]}
];

const cardsContainer = document.getElementById('cards');
const recContainer = document.getElementById('recommendations');

// Create cards
dataset.forEach((item, index) => {
  const card = document.createElement('div');
  card.className = 'card';
  card.dataset.index = index;
  card.innerHTML = `<h3>${item.name}</h3>` + item.tags.map(tag=>`<span class="tag">${tag}</span>`).join('');
  card.addEventListener('click', ()=>card.classList.toggle('selected'));
  cardsContainer.appendChild(card);
});

function getRecommendations() {
  const selectedCards = Array.from(document.querySelectorAll('.card.selected'));
  if(selectedCards.length===0){
    recContainer.innerHTML="<span style='color:red'>Select at least 1 item!</span>";
    return;
  }

  const tagCount = {};
  selectedCards.forEach(card=>{
    const item = dataset[card.dataset.index];
    item.tags.forEach(tag=>{
      tagCount[tag] = (tagCount[tag] || 0) + 1;
    });
  });

  const scores = dataset.map(item=>{
    let score=0;
    item.tags.forEach(tag=>{if(tagCount[tag]) score+=tagCount[tag];});
    return {item, score};
  });

  const selectedNames = selectedCards.map(c=>dataset[c.dataset.index].name);
  const recommendations = scores.filter(i=>!selectedNames.includes(i.item.name))
                              .sort((a,b)=>b.score-a.score)
                              .slice(0,5); // top 5

  recContainer.innerHTML='';
  recommendations.forEach(r=>{
    const card = document.createElement('div');
    card.className='card';
    card.innerHTML = `<h3>${r.item.name}</h3>` + r.item.tags.map(tag=>`<span class="tag">${tag}</span>`).join('');
    recContainer.appendChild(card);
  });
}

document.getElementById('getRec').addEventListener('click', getRecommendations);
</script>

</body>
</html>
