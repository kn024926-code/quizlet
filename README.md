<!DOCTYPE html>
<html lang = "vi">
<head>
<meta charset = "UTF-8">
<meta name = "viewport" content = "width=device-width, initial-scale=1.0">
<title>LangMaster Pro< / title>

<style>
    :root{
        --bg1:#141e30;
        --bg2:#243b55;
        --card - bg: rgba(255,255,255,0.1);
        --text:white;
}

body{
    margin : 0;
    font - family:Segoe UI;
    background:linear - gradient(135deg,var(--bg1),var(--bg2));
    color:var(--text);
    text - align:center;
}

header{
    padding:20px;
    font - size:28px;
    font - weight:bold;
}

.container{
    max - width:900px;
    margin:auto;
    padding:20px;
}

input, select, button{
    padding:10px;
    margin:5px;
    border - radius:8px;
    border:none;
}

button{
    cursor:pointer;
    background:#00c9ff;
    color:black;
    font - weight:bold;
}

.card{
    width:320px;
    height:200px;
    margin:20px auto;
    perspective:1000px;
}

.inner - card{
    width:100 %;
    height:100 %;
    position:relative;
    transition:0.8s;
    transform - style:preserve - 3d;
}

.card.flip.inner - card{
    transform:rotateY(180deg);
}

.face{
    position:absolute;
    width:100 %;
    height:100 %;
    background:var(--card - bg);
    backdrop - filter:blur(10px);
    border - radius:15px;
    display:flex;
    justify - content:center;
    align - items:center;
    font - size:22px;
    backface - visibility:hidden;
}

.back{
    transform:rotateY(180deg);
}

.score{
    font - size:18px;
    margin - top:10px;
}

.dark - toggle{
    position:absolute;
    top:20px;
    right:20px;
}
< / style>
< / head>

<body>

<header>🌎 LangMaster PRO< / header>
<button class = "dark-toggle" onclick = "toggleTheme()">🌙< / button>

<div class = "container">

<h2>➕ Thêm từ< / h2>
<input id = "topic" placeholder = "Chủ đề (VD: IELTS)">
<input id = "word" placeholder = "Từ">
<input id = "meaning" placeholder = "Nghĩa">
<button onclick = "addWord()">Thêm< / button>

<h2>📚 Chọn chủ đề< / h2>
<select id = "topicSelect" onchange = "loadTopic()">< / select>

<div class = "card" onclick = "flipCard()">
<div class = "inner-card">
<div class = "face front" id = "front">Word< / div>
<div class = "face back" id = "back">Meaning< / div>
< / div>
< / div>
<button onclick = "nextCard()">Tiếp< / button>

<h2>📝 Quiz Mode< / h2>
<p id = "question">< / p>
<div id = "answers">< / div>
<div class = "score">Điểm: < span id = "score">0 < / span > < / div>

< / div>

<script>
let data = JSON.parse(localStorage.getItem("langData")) || {};
let currentTopic = "";
let index = 0;
let score = 0;

function save() {
    localStorage.setItem("langData", JSON.stringify(data));
}

function updateTopicList() {
    let select = document.getElementById("topicSelect");
    select.innerHTML = "";
    for (let t in data) {
        let opt = document.createElement("option");
        opt.value = t;
        opt.text = t;
        select.appendChild(opt);
    }
    if (Object.keys(data).length > 0) {
        currentTopic = Object.keys(data)[0];
        loadTopic();
    }
}

function addWord() {
    let t = document.getElementById("topic").value;
    let w = document.getElementById("word").value;
    let m = document.getElementById("meaning").value;

    if (!t || !w || !m) return alert("Nhập đầy đủ!");

    if (!data[t]) data[t] = [];
    data[t].push({ word:w,meaning : m });
    save();
    updateTopicList();
    alert("Đã thêm!");
}

function loadTopic() {
    currentTopic = document.getElementById("topicSelect").value;
    index = 0;
    showCard();
    generateQuiz();
}

function showCard() {
    if (!data[currentTopic] || data[currentTopic].length == 0) return;
    document.getElementById("front").innerText = data[currentTopic][index].word;
    document.getElementById("back").innerText = data[currentTopic][index].meaning;
}

function flipCard() {
    document.querySelector(".card").classList.toggle("flip");
}

function nextCard() {
    document.querySelector(".card").classList.remove("flip");
    index = (index + 1) % data[currentTopic].length;
    showCard();
}

function generateQuiz() {
    if (!data[currentTopic] || data[currentTopic].length < 4) return;
    let arr = data[currentTopic];
    let q = arr[Math.floor(Math.random() * arr.length)];
    document.getElementById("question").innerText = "Nghĩa của: " + q.word + " ?";

    let options = [q.meaning];
    while (options.length < 4) {
        let random = arr[Math.floor(Math.random() * arr.length)].meaning;
        if (!options.includes(random)) options.push(random);
    }

    options.sort(() = > Math.random() - 0.5);
    let ansDiv = document.getElementById("answers");
    ansDiv.innerHTML = "";

    options.forEach(opt = > {
        let btn = document.createElement("button");
        btn.innerText = opt;
        btn.onclick = function() {
            if (opt == q.meaning) {
                score++;
                alert("Đúng!");
            }
            else {
                alert("Sai!");
            }
            document.getElementById("score").innerText = score;
            generateQuiz();
        }
        ansDiv.appendChild(btn);
    });
}

function toggleTheme() {
    if (document.body.style.background.includes("141e30")) {
        document.body.style.background = "linear-gradient(135deg,#ff9966,#ff5e62)";
    }
    else {
        document.body.style.background = "linear-gradient(135deg,#141e30,#243b55)";
    }
}

updateTopicList();
< / script>

< / body>
< / html>
