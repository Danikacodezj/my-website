<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>ADVICEBOX</title>

<style>
body {
  margin: 0;
  font-family: Arial;
  background: #0b0f14;
  color: white;
}

#container {
  display: flex;
  height: 100vh;
}

#left {
  width: 300px;
  background: rgba(255,255,255,0.05);
  padding: 15px;
}

#feed {
  flex: 1;
  padding: 20px;
  overflow-y: auto;
}

input, textarea {
  width: 100%;
  margin-top: 8px;
  padding: 8px;
  border-radius: 8px;
  border: none;
  outline: none;
}

button {
  width: 100%;
  margin-top: 10px;
  padding: 10px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-weight: bold;
}

.post {
  background: rgba(255,255,255,0.06);
  padding: 12px;
  border-radius: 12px;
  margin-bottom: 10px;
}

.tag {
  display: inline-block;
  font-size: 12px;
  padding: 3px 8px;
  border-radius: 10px;
  margin-top: 5px;
  background: #00c2ff;
}
</style>
</head>

<body>

<div id="container">

  <div id="left">
    <h2>ADVICEBOX</h2>

    <input id="title" placeholder="What do you need / offer?">
    <textarea id="desc" placeholder="Describe it..."></textarea>

    <select id="type">
      <option value="need">I need advice</option>
      <option value="offer">I can give advice</option>
    </select>

    <button onclick="post()">Post</button>

    <div style="margin-top:15px;font-size:12px;opacity:0.7">
      Always Anonymous. Tell us your situation. Give Advice.
    </div>
  </div>

  <div id="feed"></div>

</div>

<script>

let posts = JSON.parse(localStorage.getItem("helpboard") || "[]");

function render(){
  const feed = document.getElementById("feed");
  feed.innerHTML = "";

  posts.reverse().forEach(p => {
    const div = document.createElement("div");
    div.className = "post";

    div.innerHTML = `
      <b>${p.title}</b><br>
      <div style="opacity:0.8;margin-top:5px">${p.desc}</div>
      <div class="tag">${p.type === "need" ? "SITUATION" : "ADVICE"}</div>
    `;

    feed.appendChild(div);
  });
}

function post(){
  const title = document.getElementById("title").value;
  const desc = document.getElementById("desc").value;
  const type = document.getElementById("type").value;

  if(!title || !desc) return;

  posts.push({title, desc, type});
  localStorage.setItem("helpboard", JSON.stringify(posts));

  document.getElementById("title").value = "";
  document.getElementById("desc").value = "";

  render();
}

render();

</script>

</body>
</html>
