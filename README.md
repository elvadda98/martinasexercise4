<!DOCTYPE html>
<html>
<head>
    <title>Vocabulary Adventure</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            background-color: #f5f9fc;
            line-height: 1.6;
        }
        h1 {
            color: #2c3e50;
            text-align: center;
            margin-bottom: 10px;
        }
        .game-selector {
            display: flex;
            justify-content: center;
            margin-bottom: 20px;
            gap: 10px;
        }
        .game-btn {
            background-color: #3498db;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 6px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s;
        }
        .game-btn:hover {
            background-color: #2980b9;
            transform: translateY(-2px);
        }
        .game-btn.active {
            background-color: #2c3e50;
        }
        .game-section {
            display: none;
            background-color: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.08);
        }
        .active-game {
            display: block;
        }
        .word-bank {
            background-color: #e8f4f8;
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 20px;
            text-align: center;
        }
        .word-bank span {
            display: inline-block;
            background-color: #3498db;
            color: white;
            padding: 8px 15px;
            margin: 5px;
            border-radius: 20px;
            font-weight: bold;
            font-size: 14px;
            cursor: grab;
            user-select: none;
            transition: all 0.2s;
        }
        .word-bank span:active {
            cursor: grabbing;
            transform: scale(1.05);
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
        }
        .story {
            margin-bottom: 20px;
            padding: 15px;
            background-color: #f8f9fa;
            border-radius: 6px;
            line-height: 1.8;
        }
        .story-blank {
            border-bottom: 2px dashed #3498db;
            padding: 0 5px;
            display: inline-block;
            min-width: 120px;
            height: 28px;
            text-align: center;
            color: #e74c3c;
            font-weight: bold;
            position: relative;
            margin: 0 3px;
        }
        .story-blank.correct {
            color: #27ae60;
            border-bottom-color: #27ae60;
        }
        .story-blank.highlight {
            background-color: #fffacd;
        }
        .dropped-word {
            background-color: #e8f4f8;
            padding: 3px 8px;
            border-radius: 4px;
            cursor: pointer;
        }
        .option {
            background-color: #e8f4f8;
            padding: 8px 12px;
            margin: 5px;
            border-radius: 4px;
            cursor: pointer;
            transition: all 0.3s;
        }
        .option:hover {
            background-color: #d4e6f1;
        }
        .correct {
            background-color: #d5f5e3 !important;
            color: #27ae60;
            font-weight: bold;
        }
        .incorrect {
            background-color: #fadbd8 !important;
            color: #e74c3c;
        }
        .feedback {
            margin-top: 8px;
            padding: 8px;
            border-radius: 4px;
            display: none;
        }
        .match-container {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin-bottom: 20px;
        }
        .match-item {
            width: calc(50% - 20px);
            padding: 10px;
            background-color: #f8f9fa;
            border-radius: 6px;
            cursor: pointer;
            transition: all 0.3s;
        }
        .match-item:hover {
            background-color: #e8f4f8;
        }
        .match-item.matched {
            background-color: #d5f5e3;
            cursor: default;
        }
        #score {
            text-align: center;
            font-size: 18px;
            margin: 20px 0;
            font-weight: bold;
        }
        #restart {
            display: block;
            margin: 20px auto;
            padding: 10px 20px;
            background-color: #3498db;
            color: white;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-size: 16px;
        }
        #restart:hover {
            background-color: #2980b9;
        }
        .check-story-btn {
            background-color: #2ecc71;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 6px;
            cursor: pointer;
            font-weight: bold;
            margin-top: 15px;
        }
        #word-clone {
            position: absolute;
            pointer-events: none;
            z-index: 100;
            opacity: 0.8;
        }
    </style>
</head>
<body>
    <h1>📚 Vocabulary Adventure</h1>
    
    <div class="word-bank" id="word-bank">
        <h3>Word Bank:</h3>
        <!-- Words will be added by JavaScript -->
    </div>
    
    <div class="game-selector">
        <button class="game-btn active" onclick="showGame('story')">Story with Gaps</button>
        <button class="game-btn" onclick="showGame('multiple-choice')">Multiple Choice</button>
        <button class="game-btn" onclick="showGame('matching')">Word Matching</button>
    </div>
    
    <div id="score">Score: 0/20</div>
    
    <!-- Story with Gaps Game -->
    <div id="story" class="game-section active-game">
        <h2>📖 The Big Presentation</h2>
        <div class="story">
            <p>Today was the day of our big presentation at the company <span class="story-blank" id="story-blank-1" data-answer="headquarters">______</span>. We needed to <span class="story-blank" id="story-blank-2" data-answer="meet up">______</span> at 8:30 AM to prepare. The <span class="story-blank" id="story-blank-3" data-answer="main">______</span> challenge was technical difficulties, but we knew we could <span class="story-blank" id="story-blank-4" data-answer="overcome">______</span> them.</p>
            
            <p>We decided to <span class="story-blank" id="story-blank-5" data-answer="write down">______</span> all key points to remember them better. The meeting would <span class="story-blank" id="story-blank-6" data-answer="kick off">______</span> with our CEO's introduction. Our success was <span class="story-blank" id="story-blank-7" data-answer="depending on">______</span> how well we presented.</p>
            
            <p>The most <span class="story-blank" id="story-blank-8" data-answer="main">______</span> part was demonstrating the new product. We needed to <span class="story-blank" id="story-blank-9" data-answer="come up with">______</span> creative solutions during Q&A. Luckily, the meeting room was <span class="story-blank" id="story-blank-10" data-answer="convenient">______</span> for all team members.</p>
        </div>
        <button class="check-story-btn" onclick="checkStory()">Check Answers</button>
        <div id="story-feedback" class="feedback"></div>
    </div>
    
    <!-- Multiple Choice Game -->
    <div id="multiple-choice" class="game-section">
        <h2>🔘 Multiple Choice Challenge</h2>
        <p>Select the correct word for each sentence:</p>
        <div id="choice-container">
            <div class="sentence">
                <p>1. The team will ______ at the coffee shop before the conference.</p>
                <div class="options">
                    <div class="option" onclick="checkChoiceAnswer(0, 'meet up', 'meet up')">meet up</div>
                    <div class="option" onclick="checkChoiceAnswer(0, 'kick off', 'meet up')">kick off</div>
                    <div class="option" onclick="checkChoiceAnswer(0, 'write down', 'meet up')">write down</div>
                    <div class="option" onclick="checkChoiceAnswer(0, 'overcome', 'meet up')">overcome</div>
                </div>
                <div class="feedback" id="choice-feedback-0"></div>
            </div>
            <div class="sentence">
                <p>2. Please ______ your contact information on this form.</p>
                <div class="options">
                    <div class="option" onclick="checkChoiceAnswer(1, 'come up with', 'write down')">come up with</div>
                    <div class="option" onclick="checkChoiceAnswer(1, 'write down', 'write down')">write down</div>
                    <div class="option" onclick="checkChoiceAnswer(1, 'kick off', 'write down')">kick off</div>
                    <div class="option" onclick="checkChoiceAnswer(1, 'depending on', 'write down')">depending on</div>
                </div>
                <div class="feedback" id="choice-feedback-1"></div>
            </div>
            <div class="sentence">
                <p>3. The ______ of our discussion today is workplace safety.</p>
                <div class="options">
                    <div class="option" onclick="checkChoiceAnswer(2, 'headquarters', 'topic')">headquarters</div>
                    <div class="option" onclick="checkChoiceAnswer(2, 'main', 'topic')">main</div>
                    <div class="option" onclick="checkChoiceAnswer(2, 'topic', 'topic')">topic</div>
                    <div class="option" onclick="checkChoiceAnswer(2, 'convenient', 'topic')">convenient</div>
                </div>
                <div class="feedback" id="choice-feedback-2"></div>
            </div>
            <div class="sentence">
                <p>4. We need to ______ a new marketing strategy by Friday.</p>
                <div class="options">
                    <div class="option" onclick="checkChoiceAnswer(3, 'come up with', 'come up with')">come up with</div>
                    <div class="option" onclick="checkChoiceAnswer(3, 'overcome', 'come up with')">overcome</div>
                    <div class="option" onclick="checkChoiceAnswer(3, 'meet up', 'come up with')">meet up</div>
                    <div class="option" onclick="checkChoiceAnswer(3, 'kick off', 'come up with')">kick off</div>
                </div>
                <div class="feedback" id="choice-feedback-3"></div>
            </div>
            <div class="sentence">
                <p>5. The training session will ______ with an introduction video.</p>
                <div class="options">
                    <div class="option" onclick="checkChoiceAnswer(4, 'depending on', 'kick off')">depending on</div>
                    <div class="option" onclick="checkChoiceAnswer(4, 'write down', 'kick off')">write down</div>
                    <div class="option" onclick="checkChoiceAnswer(4, 'kick off', 'kick off')">kick off</div>
                    <div class="option" onclick="checkChoiceAnswer(4, 'main', 'kick off')">main</div>
                </div>
                <div class="feedback" id="choice-feedback-4"></div>
            </div>
        </div>
    </div>
    
    <!-- Matching Game -->
    <div id="matching" class="game-section">
        <h2>🔤 Word Matching</h2>
        <p>Match each word to its definition:</p>
        <div class="match-container" id="match-container"></div>
        <div id="match-feedback" class="feedback"></div>
    </div>
    
    <button id="restart">Restart All Games</button>
    <div id="word-clone"></div>

    <script>
        // Vocabulary data
        const words = [
            { word: "depending on", definition: "Determined by something else" },
            { word: "headquarters", definition: "Main office of a company" },
            { word: "overcome", definition: "Successfully deal with problems" },
            { word: "main", definition: "Most important" },
            { word: "convenient", definition: "Suitable or easy to reach" },
            { word: "write down", definition: "Put thoughts on paper" },
            { word: "meet up", definition: "Get together with someone" },
            { word: "topic", definition: "Subject being discussed" },
            { word: "come up with", definition: "Think of and suggest" },
            { word: "kick off", definition: "Start something officially" }
        ];
        
        // Story answers
        const storyAnswers = [
            { id: "story-blank-1", answer: "headquarters" },
            { id: "story-blank-2", answer: "meet up" },
            { id: "story-blank-3", answer: "main" },
            { id: "story-blank-4", answer: "overcome" },
            { id: "story-blank-5", answer: "write down" },
            { id: "story-blank-6", answer: "kick off" },
            { id: "story-blank-7", answer: "depending on" },
            { id: "story-blank-8", answer: "main" },
            { id: "story-blank-9", answer: "come up with" },
            { id: "story-blank-10", answer: "convenient" }
        ];
        
        let score = 0;
        let matchedPairs = 0;
        let firstSelection = null;
        let draggedWord = null;
        let wordClone = null;
        
        // Initialize all games
        function initAllGames() {
            initWordBank();
            initStory();
            initMatching();
            score = 0;
            updateScore();
        }
        
        // Initialize word bank
        function initWordBank() {
            const bank = document.getElementById("word-bank");
            bank.innerHTML = "<h3>Word Bank:</h3>";
            
            // Shuffle words and add to bank
            shuffle([...words]).forEach(wordObj => {
                const wordSpan = document.createElement("span");
                wordSpan.textContent = wordObj.word;
                wordSpan.draggable = true;
                wordSpan.dataset.word = wordObj.word;
                
                // Drag events
                wordSpan.addEventListener('dragstart', dragStart);
                wordSpan.addEventListener('dragend', dragEnd);
                
                bank.appendChild(wordSpan);
            });
        }
        
        // Story with Gaps
        function initStory() {
            storyAnswers.forEach(item => {
                const blank = document.getElementById(item.id);
                blank.innerHTML = "______";
                blank.dataset.answer = item.answer;
                blank.classList.remove("correct");
                blank.style.backgroundColor = "";
                
                // Drop events
                blank.addEventListener('dragover', dragOver);
                blank.addEventListener('dragenter', dragEnter);
                blank.addEventListener('dragleave', dragLeave);
                blank.addEventListener('drop', drop);
            });
            document.getElementById("story-feedback").style.display = "none";
        }
        
        // Matching Game
        function initMatching() {
            matchedPairs = 0;
            firstSelection = null;
            const container = document.getElementById("match-container");
            container.innerHTML = "";
            
            // Shuffle and create word and definition cards
            const shuffledWords = shuffle([...words]);
            const shuffledDefs = shuffle(shuffledWords.map(w => w.definition));
            
            shuffledWords.forEach((word, index) => {
                const wordDiv = document.createElement("div");
                wordDiv.className = "match-item";
                wordDiv.textContent = word.word;
                wordDiv.dataset.word = word.word;
                wordDiv.onclick = function() { selectMatchItem(this); };
                container.appendChild(wordDiv);
            });
            
            shuffledDefs.forEach(def => {
                const defDiv = document.createElement("div");
                defDiv.className = "match-item";
                defDiv.textContent = def;
                defDiv.dataset.definition = def;
                defDiv.onclick = function() { selectMatchItem(this); };
                container.appendChild(defDiv);
            });
            
            document.getElementById("match-feedback").style.display = "none";
        }
        
        // DRAG AND DROP FUNCTIONS
        function dragStart(e) {
            draggedWord = this;
            e.dataTransfer.setData('text/plain', this.dataset.word);
            this.style.opacity = '0.4';
            
            // Create floating clone
            wordClone = document.getElementById('word-clone');
            wordClone.textContent = this.textContent;
            wordClone.style.backgroundColor = this.style.backgroundColor;
            wordClone.style.display = 'block';
            wordClone.style.left = `${e.pageX - 50}px`;
            wordClone.style.top = `${e.pageY - 20}px`;
        }
        
        function dragEnd() {
            this.style.opacity = '1';
            wordClone.style.display = 'none';
        }
        
        function dragOver(e) {
            e.preventDefault();
        }
        
        function dragEnter(e) {
            e.preventDefault();
            this.classList.add('highlight');
        }
        
        function dragLeave() {
            this.classList.remove('highlight');
        }
        
        function drop(e) {
            e.preventDefault();
            this.classList.remove('highlight');
            
            const word = e.dataTransfer.getData('text/plain');
            const blank = this;
            
            // Check if blank is empty
            if (blank.textContent === "______") {
                blank.innerHTML = `<span class="dropped-word" data-word="${word}">${word}</span>`;
                
                // Add remove functionality
                const droppedWord = blank.querySelector('.dropped-word');
                droppedWord.addEventListener('click', function() {
                    blank.innerHTML = "______";
                    blank.style.backgroundColor = "";
                });
                
                // Color feedback for correctness
                if (word === blank.dataset.answer) {
                    blank.style.backgroundColor = "#e8f5e9";
                } else {
                    blank.style.backgroundColor = "#ffebee";
                }
            }
        }
        
        // Update word clone position during drag
        document.addEventListener('dragover', function(e) {
            if (wordClone) {
                wordClone.style.left = `${e.pageX - 50}px`;
                wordClone.style.top = `${e.pageY - 20}px`;
            }
        });
        
        // Game selection
        function showGame(gameId) {
            document.querySelectorAll('.game-section').forEach(section => {
                section.classList.remove('active-game');
            });
            document.querySelectorAll('.game-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            document.getElementById(gameId).classList.add('active-game');
            document.querySelector(`.game-btn[onclick="showGame('${gameId}')"]`).classList.add('active');
        }
        
        // Check story answers
        function checkStory() {
            let correctCount = 0;
            const feedback = document.getElementById("story-feedback");
            feedback.innerHTML = "";
            
            storyAnswers.forEach(item => {
                const blank = document.getElementById(item.id);
                const droppedWord = blank.querySelector('.dropped-word');
                
                if (droppedWord && droppedWord.dataset.word === item.answer) {
                    blank.classList.add("correct");
                    correctCount++;
                } else {
                    blank.classList.remove("correct");
                    if (droppedWord) {
                        feedback.innerHTML += `<p>The blank for "${item.answer}" was incorrect.</p>`;
                    } else {
                        feedback.innerHTML += `<p>You missed the blank for "${item.answer}".</p>`;
                    }
                }
            });
            
            if (correctCount === storyAnswers.length) {
                feedback.innerHTML = "<p style='color:#27ae60;'>🎉 Perfect! All answers correct!</p>";
                score += 5; // Bonus for completing story
                updateScore();
            } else {
                feedback.innerHTML += `<p>You got ${correctCount} out of ${storyAnswers.length} correct.</p>`;
                score += correctCount;
                updateScore();
            }
            feedback.style.display = "block";
        }
        
        // Check multiple choice answers
        function checkChoiceAnswer(index, selected, correct) {
            const feedback = document.getElementById(`choice-feedback-${index}`);
            const options = document.querySelectorAll(`#choice-container .sentence:nth-child(${index+1}) .option`);
            
            options.forEach(opt => {
                opt.style.pointerEvents = "none"; // Disable further clicks
                if (opt.textContent === correct) {
                    opt.classList.add("correct");
                } else if (opt.textContent === selected && selected !== correct) {
                    opt.classList.add("incorrect");
                }
            });
            
            if (selected === correct) {
                feedback.textContent = "✓ Correct!";
                feedback.className = "feedback correct";
                feedback.style.display = "block";
                if (!options[0].parentElement.dataset.scored) {
                    score++;
                    options[0].parentElement.dataset.scored = "true";
                    updateScore();
                }
            } else {
                feedback.textContent = `✗ Incorrect. The correct answer is: ${correct}`;
                feedback.className = "feedback incorrect";
                feedback.style.display = "block";
            }
        }
        
        // Matching game selection
        function selectMatchItem(item) {
            if (item.classList.contains("matched")) return;
            
            if (!firstSelection) {
                firstSelection = item;
                item.style.backgroundColor = "#d4e6f1";
            } else {
                // Check for match
                const word = firstSelection.dataset.word || words.find(w => w.definition === firstSelection.textContent)?.word;
                const definition = item.dataset.definition || words.find(w => w.word === firstSelection.textContent)?.definition;
                
                if ((firstSelection.dataset.word && item.textContent === definition) || 
                    (item.dataset.word && firstSelection.textContent === definition)) {
                    
                    firstSelection.classList.add("matched");
                    item.classList.add("matched");
                    firstSelection.style.backgroundColor = "";
                    matchedPairs++;
                    
                    if (matchedPairs === words.length) {
                        document.getElementById("match-feedback").textContent = "🎉 Perfect! All matches correct!";
                        document.getElementById("match-feedback").className = "feedback correct";
                        document.getElementById("match-feedback").style.display = "block";
                        score += 5; // Bonus for completing matching
                        updateScore();
                    }
                } else {
                    setTimeout(() => {
                        firstSelection.style.backgroundColor = "";
                        item.style.backgroundColor = "";
                    }, 500);
                }
                firstSelection = null;
            }
        }
        
        // Update score display
        function updateScore() {
            document.getElementById("score").textContent = `Score: ${score}/20`;
        }
        
        // Shuffle array
        function shuffle(array) {
            return array.sort(() => Math.random() - 0.5);
        }
        
        // Restart all games
        document.getElementById("restart").onclick = initAllGames;
        
        // Initialize on load
        initAllGames();
    </script>
</body>
</html>
