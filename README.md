<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>The Fortune Teller</title>
<link href="https://fonts.googleapis.com/css2?family=Cinzel+Decorative:wght@700&family=Playfair+Display:wght@400;700&display=swap" rel="stylesheet">
<style>
    * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
    }

    body {
        font-family: 'Playfair Display', serif;
        background: linear-gradient(rgba(0, 0, 0, 0.8), rgba(0, 0, 0, 0.9)), 
                    url('https://images.unsplash.com/photo-1519681393784-d120267933ba?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1740&q=80');
        background-size: cover;
        background-position: center;
        background-attachment: fixed;
        text-align: center;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        min-height: 100vh;
        color: #fff;
        padding: 20px;
        position: relative;
        overflow: hidden;
    }

    body::before {
        content: "";
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: radial-gradient(circle at center, transparent 60%, rgba(0, 0, 0, 0.9) 100%);
        pointer-events: none;
        z-index: -1;
    }

    .container {
        max-width: 800px;
        width: 100%;
        padding: 30px;
        background: rgba(0, 0, 0, 0.7);
        border-radius: 20px;
        box-shadow: 0 0 30px rgba(139, 0, 139, 0.5);
        border: 1px solid rgba(255, 215, 0, 0.3);
        position: relative;
        z-index: 2;
        backdrop-filter: blur(5px);
        margin-top: 20px;
    }

    h1 {
        font-family: 'Cinzel Decorative', cursive;
        font-size: 3.5rem;
        margin-bottom: 20px;
        color: #ffd700;
        text-shadow: 0 0 10px rgba(255, 215, 0, 0.7);
        letter-spacing: 2px;
        animation: glow 2s infinite alternate;
    }

    .crystal-ball {
        width: 200px;
        height: 200px;
        background: radial-gradient(circle at 30% 30%, #a0d8ef, #1e90ff, #00008b);
        border-radius: 50%;
        margin: 20px auto;
        position: relative;
        box-shadow: 0 0 30px rgba(30, 144, 255, 0.7);
        overflow: hidden;
        border: 5px solid #ffd700;
        animation: float 3s ease-in-out infinite;
    }

    .crystal-ball::before {
        content: "";
        position: absolute;
        top: 20%;
        left: 20%;
        width: 30%;
        height: 30%;
        background: rgba(255, 255, 255, 0.3);
        border-radius: 50%;
        filter: blur(5px);
    }

    .crystal-ball::after {
        content: "";
        position: absolute;
        top: 10%;
        left: 10%;
        width: 15%;
        height: 15%;
        background: rgba(255, 255, 255, 0.5);
        border-radius: 50%;
        filter: blur(3px);
    }

    #btn-ask {
        background: linear-gradient(45deg, #8b008b, #4b0082, #8b008b);
        color: #fff;
        padding: 20px 50px;
        font-size: 1.8rem;
        font-family: 'Playfair Display', serif;
        border: none;
        border-radius: 50px;
        cursor: pointer;
        margin: 30px 0;
        box-shadow: 0 0 20px rgba(139, 0, 139, 0.7);
        transition: all 0.3s ease;
        position: relative;
        overflow: hidden;
        letter-spacing: 1px;
        font-weight: bold;
    }

    #btn-ask:hover {
        transform: translateY(-5px);
        box-shadow: 0 10px 25px rgba(139, 0, 139, 0.9);
        background: linear-gradient(45deg, #9b209b, #5b1092, #9b209b);
    }

    #btn-ask:active {
        transform: translateY(0);
        box-shadow: 0 0 15px rgba(139, 0, 139, 0.7);
    }

    #btn-ask::after {
        content: "";
        position: absolute;
        top: -50%;
        left: -60%;
        width: 20px;
        height: 200%;
        background: rgba(255, 255, 255, 0.3);
        transform: rotate(30deg);
        transition: all 0.6s;
    }

    #btn-ask:hover::after {
        left: 120%;
    }

    .fortune-display {
        background: rgba(0, 0, 0, 0.6);
        border: 1px solid rgba(255, 215, 0, 0.3);
        border-radius: 15px;
        padding: 25px;
        margin: 25px 0;
        min-height: 180px;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        box-shadow: 0 0 20px rgba(0, 0, 0, 0.5);
        transition: all 0.5s ease;
    }

    .fortune-text {
        font-size: 2.2rem;
        font-weight: bold;
        color: #ffd700;
        margin-bottom: 15px;
        text-shadow: 0 0 10px rgba(255, 215, 0, 0.7);
        opacity: 0;
        transform: translateY(20px);
        transition: all 0.8s ease;
    }

    .fortune-text.show {
        opacity: 1;
        transform: translateY(0);
    }

    .fortune-meaning {
        font-size: 1.2rem;
        color: #a0d8ef;
        font-style: italic;
        max-width: 600px;
        margin-top: 10px;
        opacity: 0;
        transform: translateY(20px);
        transition: all 0.8s ease 0.3s;
    }

    .fortune-meaning.show {
        opacity: 1;
        transform: translateY(0);
    }

    .stars {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        pointer-events: none;
        z-index: 1;
    }

    .star {
        position: absolute;
        background-color: #fff;
        border-radius: 50%;
        animation: twinkle var(--duration) infinite ease-in-out;
    }

    footer {
        font-size: 0.9rem;
        color: #ccc;
        text-align: center;
        background: rgba(0, 0, 0, 0.6);
        padding: 1.2em;
        border-top: 1px solid rgba(255, 215, 0, 0.3);
        width: 100%;
        position: relative;
        z-index: 2;
        margin-top: 30px;
    }

    footer a {
        color: #ffd700;
        text-decoration: none;
        transition: all 0.3s ease;
    }

    footer a:hover {
        color: #a0d8ef;
        text-shadow: 0 0 5px rgba(160, 216, 239, 0.7);
    }

    @keyframes glow {
        0% {
            text-shadow: 0 0 10px rgba(255, 215, 0, 0.7);
        }
        100% {
            text-shadow: 0 0 20px rgba(255, 215, 0, 0.9), 0 0 30px rgba(255, 215, 0, 0.6);
        }
    }

    @keyframes float {
        0% {
            transform: translateY(0);
        }
        50% {
            transform: translateY(-15px);
        }
        100% {
            transform: translateY(0);
        }
    }

    @keyframes twinkle {
        0%, 100% {
            opacity: 0.2;
        }
        50% {
            opacity: 1;
        }
    }

    @media (max-width: 768px) {
        h1 {
            font-size: 2.5rem;
        }
        
        .crystal-ball {
            width: 150px;
            height: 150px;
        }
        
        #btn-ask {
            padding: 15px 30px;
            font-size: 1.5rem;
        }
        
        .fortune-text {
            font-size: 1.8rem;
        }
    }

    @media (max-width: 480px) {
        h1 {
            font-size: 2rem;
        }
        
        .container {
            padding: 20px 15px;
        }
        
        .crystal-ball {
            width: 120px;
            height: 120px;
        }
        
        #btn-ask {
            padding: 12px 25px;
            font-size: 1.3rem;
        }
        
        .fortune-text {
            font-size: 1.5rem;
        }
    }
</style>
</head>
<body>
    <div class="stars" id="stars"></div>
    
    <div class="container">
        <h1>The Fortune Teller</h1>
        
        <div class="crystal-ball"></div>
        
        <button id="btn-ask">Ask a Question</button>
        
        <div class="fortune-display">
            <div class="fortune-text" id="fortune-text">Your destiny awaits...</div>
            <div class="fortune-meaning" id="fortune-meaning">Focus your thoughts and ask your question</div>
        </div>
    </div>
    
    <footer>
        <p>Created with mystical energies | The answers come from the cosmos</p>
        <p>Images by <a href="https://unsplash.com" target="_blank">Unsplash</a></p>
    </footer>

<script>
    // Create stars
    const starsContainer = document.getElementById('stars');
    for (let i = 0; i < 150; i++) {
        const star = document.createElement('div');
        star.classList.add('star');
        star.style.left = `${Math.random() * 100}%`;
        star.style.top = `${Math.random() * 100}%`;
        star.style.width = `${Math.random() * 3}px`;
        star.style.height = star.style.width;
        star.style.setProperty('--duration', `${Math.random() * 3 + 2}s`);
        starsContainer.appendChild(star);
    }

    const answers = [
        { text: "It is certain", meaning: "Confident and definite." },
        { text: "It is decidedly so", meaning: "Strong confirmation." },
        { text: "Without a doubt", meaning: "Assurance of certainty." },
        { text: "Yes, definitely", meaning: "Resounding yes." },
        { text: "You may rely on it", meaning: "Trustworthy." },
        { text: "As I see it, yes", meaning: "Optimistic." },
        { text: "Most likely", meaning: "Favorable odds." },
        { text: "Outlook good", meaning: "Positive outlook." },
        { text: "Signs point to yes", meaning: "Cosmic confirmation." },
        { text: "Reply hazy, try again", meaning: "Uncertain, try again later." },
        { text: "Ask again later", meaning: "Requires time to align." },
        { text: "Better not tell you now", meaning: "The mystery must unfold." },
        { text: "Cannot predict now", meaning: "Elusive prediction." },
        { text: "Concentrate and ask again", meaning: "Focus your energy." },
        { text: "Don't count on it", meaning: "Cautionary." },
        { text: "My reply is no", meaning: "Firm negative." },
        { text: "My sources say no", meaning: "Definitive negative." },
        { text: "Outlook not so good", meaning: "Discouraging signs." },
        { text: "Very doubtful", meaning: "Highly doubtful." }
    ];

    const btnAsk = document.getElementById('btn-ask');
    const fortuneText = document.getElementById('fortune-text');
    const fortuneMeaning = document.getElementById('fortune-meaning');
    const speechSynthesis = window.speechSynthesis;

    btnAsk.addEventListener('click', function() {
        // Reset animations
        fortuneText.classList.remove('show');
        fortuneMeaning.classList.remove('show');
        
        // Add thinking effect
        fortuneText.textContent = "Consulting the spirits...";
        fortuneMeaning.textContent = "The cosmos are aligning...";
        fortuneText.classList.add('show');
        fortuneMeaning.classList.add('show');
        
        // Get random answer after delay
        setTimeout(() => {
            const randomIndex = Math.floor(Math.random() * answers.length);
            const answer = answers[randomIndex];
            
            // Apply new answer with animation
            fortuneText.textContent = answer.text;
            fortuneMeaning.textContent = answer.meaning;
            
            // Trigger animations
            fortuneText.classList.remove('show');
            fortuneMeaning.classList.remove('show');
            void fortuneText.offsetWidth; // Trigger reflow
            fortuneText.classList.add('show');
            fortuneMeaning.classList.add('show');
            
            // Speak the answer
            speak(answer.text);
        }, 2000);
    });

    function speak(text) {
        // Cancel any ongoing speech
        speechSynthesis.cancel();
        
        const utterance = new SpeechSynthesisUtterance(text);
        utterance.rate = 0.9;
        utterance.pitch = 0.8;
        utterance.volume = 1;
        
        // Set voice to a mystical sounding one if available
        const voices = speechSynthesis.getVoices();
        const mysticalVoices = voices.filter(voice => 
            voice.name.includes('Samantha') || 
            voice.name.includes('Karen') || 
            voice.name.includes('Moira') ||
            voice.lang.includes('en')
        );
        
        if (mysticalVoices.length > 0) {
            utterance.voice = mysticalVoices[0];
        }
        
        speechSynthesis.speak(utterance);
    }

    // Initialize speech synthesis voices
    if (speechSynthesis.onvoiceschanged !== undefined) {
        speechSynthesis.onvoiceschanged = () => {};
    }
</script>
</body>
</html>
