---
layout: page
title: About
permalink: /about/
---
<html>
<head>

<style>
    h2 {text-align: center;}
    h1 {text-align: center;}
    p {text-align: center;}
i   mg {
  padding-left: 30px;
  display: inline-block;
}
    .center {
  display: block;
  margin-left: auto;
  margin-right: auto;
}
    #secretMessage {
        display: none;
}
    a.button {
    padding: 1px 6px;
    border: 1px outset buttonborder;
    border-radius: 3px;
    color: buttontext;
    background-color: gold;
    text-decoration: none;
}
</style>
</head>
<body>

<h1>About Me</h1>
<p>My name is Nikhil Ekambaram. I enjoy:</p><br>
<ul>
    <li>Fencing (saber is the best)</li>
    <li>Videogames (Titanfall 2 #1)</li>
    <li>Music (Idol by YOASOBI!!!!!!)</li>
    <li>My dog </li>

</ul>





<p><h1><span style="color:purple;font-weight:bold">Ai Hoshino</span></h1></p> 
<br>
<p>Arguably, Ai Hoshino is the #1 idol. This is because idol=idol and as shown in the animated picture below, has 2 very motivated w/ lightsticks.</p><br>

<p>
<a href="https://evansvetina.github.io/CSSEproj1" class="button">Obligatory partner blog plug</a>

</p>


<img src="https://cdn.myanimelist.net/r/200x268/images/characters/6/496453.jpg?s=f78b6dbaf8d93085f406d5bb9d2aab70" height="230" class="" style="padding-left: 200px;"> <img src="https://media1.tenor.com/m/PdX3X0HrrkYAAAAd/oshi-no-ko-aqua.gif" height="230"> <img src="https://staticg.sportskeeda.com/editor/2023/05/71dd8-16838551463424-1920.jpg" class="center" height="230">

<h2>The #1 playlist i'm listening to right now</h2> <br> <iframe style="border-radius:12px" src="https://open.spotify.com/embed/playlist/13JzhlV0FLIB1ue21TCusB?utm_source=generator" width="100%" height="152" frameBorder="0" allowfullscreen="" allow="autoplay; clipboard-write; encrypted-media; fullscreen; picture-in-picture"></iframe>

<p>Art thou a true anime lover? Prove thyself.(<sub>My beloved...</sub>)</p>
    <div id="secretMessage">
        <p>You have proven yourself. You may now see my anime list</p>
        <ul>
            <li>#1 Your Lie in April -already watched</li>
            <li>#2 Oshi No Ko -already watched</li>
            <li>#3 Jujutsu Kaisen -already watched</li>
            <li>#4 >The Angel Next Door Spoils Me Rotten</li>
        </ul> 
        <h3>Not watched yet</h3>
        <ul>
            <li>Kiznaiver</li>
            <li>Vinland saga</li>
            <li>Death Note</li>
        </ul>
    </div>

<script>
        // Correct the sequence (no spaces in between)
        const weebCode = [
            "a", "i", "h", "o", "s", "h", "i", "n", "o"
        ];

        let inputSequence = [];

        window.addEventListener("keydown", (event) => {
            // Add the key pressed to the input sequence
            inputSequence.push(event.key.toLowerCase());

            // Limit the input sequence length to the length of the weebCode
            if (inputSequence.length > weebCode.length) {
                inputSequence.shift();
            }

            // Check if the input matches the weebCode sequence
            if (JSON.stringify(inputSequence) === JSON.stringify(weebCode)) {
                // Reveal the secret message if the code is correct
                document.getElementById("secretMessage").style.display = "block";
            }
        });
</script>  
 </body>   


</html>