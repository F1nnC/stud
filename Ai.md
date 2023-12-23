---
author: Finn Carpenter
description: Testing out AI
permalink: /ChatBot
layout: posts
---

<button id="btn">Click Me</button>
<input type="text" id="input">
<p id="result"></p>

<script type="importmap">
    {
    "imports": {
        "@google/generative-ai": "https://esm.run/@google/generative-ai"
    }
    }
</script>

<script type="module">
    import { GoogleGenerativeAI } from "@google/generative-ai";
    const button = document.getElementById('btn'); 
    // Fetch your API_KEY
    const API_KEY = "AIzaSyAhz12wUf7k5twpq4sDuY44Ef4UVvUG3XE";

    // Access your API key (see "Set up your API key" above)
    const genAI = new GoogleGenerativeAI(API_KEY);

    async function runChat() {
        // For text-only input, use the gemini-pro model
        const model = genAI.getGenerativeModel({ model: "gemini-pro"});
        var input = document.getElementById('input').value;
        const resultDIV = document.getElementById('result');
        const prompt = input;

        const result = await model.generateContent(prompt);
        const response = await result.response;
        const text = response.text();
        resultDIV.innerHTML = text;
    }

    button.addEventListener('click', runChat);
</script>