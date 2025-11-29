<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>짱구의 야식 성격 테스트</title>
    
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- 폰트: 짱구 느낌의 둥글둥글한 'Jua' 폰트 -->
    <link href="https://fonts.googleapis.com/css2?family=Jua&display=swap" rel="stylesheet">

    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Jua', 'sans-serif'],
                    },
                    colors: {
                        'shin-red': '#FF4E4E',      // 짱구 티셔츠 빨강
                        'shin-yellow': '#FFD700',   // 짱구 바지 노랑
                        'shin-bg': '#FFF8DC',       // 크림색 배경
                        'shin-blue': '#40E0D0',     // 파자마 민트
                    },
                    animation: {
                        'bounce-slow': 'bounce 2s infinite',
                        'wiggle': 'wiggle 1s ease-in-out infinite',
                        'pop': 'pop 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275) forwards',
                    },
                    keyframes: {
                        wiggle: {
                            '0%, 100%': { transform: 'rotate(-3deg)' },
                            '50%': { transform: 'rotate(3deg)' },
                        },
                        pop: {
                            '0%': { transform: 'scale(0.5)', opacity: '0' },
                            '100%': { transform: 'scale(1)', opacity: '1' },
                        }
                    }
                }
            }
        }
    </script>

    <style>
        /* 짱구 파자마 패턴 배경 */
        .shin-pattern {
            background-color: #98FB98;
            background-image: 
                radial-gradient(circle, #FF6B6B 15%, transparent 16%),
                radial-gradient(var(--tw-gradient-stops)); 
            background-size: 40px 40px;
            /* 네모 세모는 CSS로 그리기 복잡하여 심플한 도트/패턴으로 느낌 냄 */
        }
        
        .shape-square {
            background-image: linear-gradient(45deg, #FFD700 25%, transparent 25%, transparent 75%, #FFD700 75%, #FFD700),
            linear-gradient(45deg, #FFD700 25%, transparent 25%, transparent 75%, #FFD700 75%, #FFD700);
            background-position: 0 0, 20px 20px;
            background-size: 40px 40px;
            opacity: 0.3;
        }

        .card-shadow {
            box-shadow: 8px 8px 0px 0px rgba(0,0,0,1);
            border: 4px solid black;
        }

        /* 화면 전환 효과 */
        .section {
            transition: opacity 0.3s ease, transform 0.3s ease;
            position: absolute;
            width: 100%;
            height: 100%;
            top: 0;
            left: 0;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            opacity: 0;
            pointer-events: none;
            transform: scale(0.95);
        }

        .section.active {
            opacity: 1;
            pointer-events: auto;
            transform: scale(1);
            position: relative; /* 활성화된 섹션은 흐름에 참여 */
        }

        /* 이미지 플레이스홀더 스타일 */
        .character-placeholder {
            width: 200px;
            height: 200px;
            background-color: #eee;
            border: 4px solid black;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            overflow: hidden;
            background-image: url('https://via.placeholder.com/200x200?text=NO+IMAGE'); /* 기본 이미지 */
            background-size: cover;
            background-position: center;
        }
    </style>
</head>
<body class="bg-shin-blue min-h-screen flex items-center justify-center p-4 overflow-hidden relative">

    <!-- 배경 장식 (파자마 패턴 느낌) -->
    <div class="absolute inset-0 shape-square pointer-events-none"></div>
    <div class="absolute top-10 left-10 text-6xl animate-bounce-slow opacity-50">🔺</div>
    <div class="absolute bottom-10 right-10 text-6xl animate-wiggle opacity-50">🟩</div>
    <div class="absolute top-1/2 right-10 text-6xl animate-pulse opacity-50">🟡</div>

    <!-- 메인 컨테이너 -->
    <main class="w-full max-w-lg bg-white card-shadow rounded-3xl p-6 relative min-h-[650px] flex flex-col items-center justify-center overflow-hidden">

        <!-- 1. 시작 화면 -->
        <div id="intro-screen" class="section active text-center">
            <h1 class="text-4xl text-shin-red mb-2 drop-shadow-sm leading-tight">
                떡잎방범대<br>야식 성격 테스트
            </h1>
            <p class="text-gray-500 mb-8 text-lg">나랑 딱 맞는 짱구 캐릭터는 누구?</p>

            <div class="relative w-48 h-48 mx-auto mb-8 animate-wiggle">
                <!-- 메인 이미지 (이모지로 대체) -->
                <div class="absolute inset-0 bg-yellow-200 rounded-full blur-xl opacity-70"></div>
                <div class="relative text-[10rem] leading-none select-none">🍱</div>
                <div class="absolute bottom-0 right-0 text-6xl animate-bounce">👦🏻</div>
            </div>

            <button onclick="goToGame()" class="w-full bg-shin-red text-white text-2xl py-4 rounded-2xl border-4 border-black shadow-[4px_4px_0px_0px_rgba(0,0,0,1)] hover:translate-y-1 hover:shadow-none transition-all">
                테스트 시작하기 🚀
            </button>
        </div>

        <!-- 2. 질문 화면 -->
        <div id="game-screen" class="section w-full">
            <!-- 상단 진행바 -->
            <div class="w-full flex justify-between items-end mb-6 px-2">
                <span class="text-2xl font-bold text-shin-red">Q.<span id="q-num">1</span></span>
                <div class="flex-1 ml-4 h-4 bg-gray-200 rounded-full border-2 border-black overflow-hidden">
                    <div id="progress-bar" class="h-full bg-shin-yellow transition-all duration-300" style="width: 8%"></div>
                </div>
            </div>

            <!-- 질문 텍스트 -->
            <div class="flex-1 flex items-center justify-center mb-8">
                <h2 id="q-text" class="text-2xl text-center text-gray-800 leading-snug break-keep">
                    질문 내용이 여기에 들어갑니다.
                </h2>
            </div>

            <!-- 답변 버튼 -->
            <div class="flex flex-col gap-4 w-full">
                <button onclick="answer('A')" class="group relative w-full bg-blue-50 hover:bg-blue-100 border-4 border-blue-400 text-gray-800 p-5 rounded-xl text-left transition-all hover:scale-[1.02] active:scale-95">
                    <span class="absolute top-1/2 left-4 -translate-y-1/2 text-3xl opacity-50 group-hover:opacity-100 transition-opacity">🅰️</span>
                    <span id="a-text" class="block pl-12 text-lg font-medium">답변 A</span>
                </button>

                <div class="text-center text-gray-400 font-bold text-sm my-1">- VS -</div>

                <button onclick="answer('B')" class="group relative w-full bg-red-50 hover:bg-red-100 border-4 border-red-400 text-gray-800 p-5 rounded-xl text-left transition-all hover:scale-[1.02] active:scale-95">
                    <span class="absolute top-1/2 left-4 -translate-y-1/2 text-3xl opacity-50 group-hover:opacity-100 transition-opacity">🅱️</span>
                    <span id="b-text" class="block pl-12 text-lg font-medium">답변 B</span>
                </button>
            </div>
        </div>

        <!-- 3. 로딩 화면 -->
        <div id="loading-screen" class="section text-center">
            <div class="text-6xl animate-spin-slow mb-4">🌀</div>
            <h3 class="text-2xl text-gray-700">성격 분석 중...</h3>
            <p class="text-gray-500 mt-2">초코비 먹으면서 기다려!</p>
        </div>

        <!-- 4. 결과 화면 -->
        <div id="result-screen" class="section w-full text-center">
            <p class="text-lg text-gray-500 mb-1">나의 야식 스타일은?</p>
            <h2 id="result-mbti" class="text-4xl font-black text-shin-red mb-4 drop-shadow-md tracking-wider">ENTP</h2>
            
            <!-- 캐릭터 이미지 영역 -->
            <div class="relative mx-auto mb-4">
                <div id="char-img-container" class="character-placeholder mx-auto">
                    <!-- 실제 이미지가 있다면 여기에 <img src="..."> 가 들어갑니다 -->
                    <span class="text-gray-400 text-sm p-4">캐릭터 이미지를<br>넣어주세요!</span>
                </div>
                <!-- 장식용 스티커 -->
                <div class="absolute -bottom-2 -right-2 text-5xl animate-bounce">✨</div>
            </div>

            <h3 id="result-char-name" class="text-2xl bg-yellow-100 px-4 py-2 rounded-lg border-2 border-black inline-block mb-4">
                캐릭터 이름
            </h3>

            <div class="bg-gray-50 border-2 border-dashed border-gray-400 rounded-xl p-4 mb-6 text-left text-gray-700 leading-relaxed text-sm h-32 overflow-y-auto">
                <span id="result-desc">결과 설명이 나옵니다.</span>
            </div>

            <div class="grid grid-cols-2 gap-2 w-full mb-4">
                 <div class="bg-blue-100 p-2 rounded-lg border border-blue-300">
                    <div class="text-xs text-blue-500 font-bold">환상의 짝꿍 🙆‍♂️</div>
                    <div id="partner-good" class="text-sm font-bold mt-1">철수</div>
                </div>
                <div class="bg-red-100 p-2 rounded-lg border border-red-300">
                    <div class="text-xs text-red-500 font-bold">환장의 짝꿍 🙅‍♀️</div>
                    <div id="partner-bad" class="text-sm font-bold mt-1">훈이</div>
                </div>
            </div>

            <button onclick="location.reload()" class="w-full bg-gray-800 text-white text-xl py-3 rounded-xl border-4 border-gray-600 shadow-[2px_2px_0px_0px_#999] hover:bg-black transition-colors">
                다시 하기 🔄
            </button>
        </div>

    </main>

    <script>
        // --------------------------------------------------------
        // 1. MBTI 12문항 (E/I, S/N, T/F, J/P 각 3문제씩)
        // --------------------------------------------------------
        const questions = [
            // E vs I (에너지 방향)
            { type: 'EI', q: "친구들이 '오늘 밤 야식 콜?' 이라고 톡이 왔다.", a: "당연하지! 바로 나간다. (파티)", b: "오늘은 좀 피곤한데... 읽씹하거나 거절. (혼자)" },
            { type: 'EI', q: "단골 식당 사장님이 말을 걸어온다면?", a: "오 사장님~ 하면서 수다를 떤다.", b: "네.. 하고 어색하게 웃으며 밥만 먹는다." },
            { type: 'EI', q: "맛있는 야식 사진을 찍었다! 나의 행동은?", a: "바로 인스타 스토리에 공유! 자랑한다.", b: "나만 간직하거나 친한 친구 한명에게만 보낸다." },
            
            // S vs N (인식 방식)
            { type: 'SN', q: "새로 생긴 치킨집 메뉴판을 볼 때 나는?", a: "가격, 양, 리뷰수 등 팩트를 체크한다.", b: "'지옥에서 온 불닭' 같은 이름과 컨셉에 끌린다." },
            { type: 'SN', q: "떡볶이 맛을 설명할 때 나는?", a: "맵고 달고 떡이 쫄깃해.", b: "마치 입안에서 용이 춤추는 듯한 맛이야!" },
            { type: 'SN', q: "야식을 먹으며 하는 생각은?", a: "맛있다. 배부르다. 내일 붓겠지? (현실)", b: "이 닭은 어디서 왔을까... 닭의 일생은... (상상)" },

            // T vs F (판단 기준)
            { type: 'TF', q: "친구가 '나 우울해서 케이크 먹었어'라고 한다.", a: "어느 카페? 맛있었어? (사실 정보)", b: "무슨 일 있어? 괜찮아? (감정 공감)" },
            { type: 'TF', q: "야식 메뉴를 고를 때 더 중요한 건?", a: "가성비, 할인쿠폰, 영양성분 (논리)", b: "지금 나의 기분, 가게 분위기 (감성)" },
            { type: 'TF', q: "배달 음식이 1시간 늦게 도착했다.", a: "가게에 전화해서 따지거나 배달앱 고객센터 문의.", b: "배달 기사님도 힘들겠지... 그냥 먹는다." },

            // J vs P (생활 양식)
            { type: 'JP', q: "야식 먹을 준비를 할 때 나는?", a: "미리 세팅하고, 먹고 나서 치울 계획까지 세움.", b: "일단 뚜껑 열고 먹어! 치우는 건 나중에." },
            { type: 'JP', q: "주말에 맛집 탐방을 가기로 했다.", a: "오픈시간, 브레이크타임, 이동경로 검색 완료.", b: "일단 그 동네 가서 끌리는 곳 들어간다." },
            { type: 'JP', q: "내일 먹을 야식을 냉장고에 넣어둘 때", a: "밀폐용기에 깔끔하게 소분해서 라벨링.", b: "비닐봉지째로 쑤셔 넣는다." }
        ];

        // --------------------------------------------------------
        // 2. 16가지 결과 데이터 (MBTI 매칭)
        // --------------------------------------------------------
        // 이미지 URL은 저작권 문제로 비워두거나 플레이스홀더를 사용합니다.
        // 사용자가 직접 'imgUrl' 부분에 인터넷 이미지 주소를 넣으면 작동합니다.
        const results = {
            "ISTJ": { name: "철수", desc: "철저한 계획파! 야식도 영양성분과 칼로리를 따져가며 먹는 당신. 흘리는 꼴은 못 봐요.", partner: "ENFP", bad: "ENFJ", imgColor: "#E0F7FA" },
            "ISFJ": { name: "훈이", desc: "소심하지만 세심한 당신. 친구들이 먹고 싶다는 메뉴에 '난 다 좋아...' 하며 맞춰주는 배려왕.", partner: "ESFP", bad: "ENTP", imgColor: "#E8F5E9" },
            "INFJ": { name: "흑곰 (악당 보스)", desc: "조용해 보이지만 속에는 거대한 계획이 있는 당신. 야식 메뉴 하나에도 깊은 철학이 담겨 있습니다.", partner: "ENFP", bad: "ESTP", imgColor: "#EDE7F6" },
            "INTJ": { name: "원장 선생님", desc: "겉모습은 무서워 보일 수 있지만, 사실은 누구보다 따뜻하고 체계적인 리더! 효율적인 식사를 추구해요.", partner: "ENFP", bad: "ESFP", imgColor: "#FFF3E0" },
            
            "ISTP": { name: "신형만 (아빠)", desc: "만사 귀찮지만 할 땐 하는 스타일! 야식은 간단하게 맥주 한 캔에 안주 하나면 충분해.", partner: "ESTJ", bad: "INFP", imgColor: "#ECEFF1" },
            "ISFP": { name: "봉미선 (엄마)", desc: "누워있는 게 제일 좋아~ 하지만 맛있는 건 포기 못해! 세일하는 마트 전단지를 보면 눈이 번쩍!", partner: "ESTJ", bad: "ENTJ", imgColor: "#FCE4EC" },
            "INFP": { name: "흰둥이", desc: "말수는 적지만 속이 깊고 충성심 강한 당신. 혼자 조용히 맛미를 음미하는 시간이 제일 행복해요.", partner: "ENFJ", bad: "ESTJ", imgColor: "#FAFAFA" },
            "INTP": { name: "맹구", desc: "독특한 자신만의 세계가 있는 당신. '이 돌멩이 모양 초콜릿은 뭐지?' 호기심 가득한 야식 탐험가.", partner: "ENTJ", bad: "ESFJ", imgColor: "#FFF8E1" },
            
            "ESTP": { name: "액션가면", desc: "자신감 뿜뿜! '음하하하! 이 구역의 맛집은 내가 접수한다!' 활동적이고 스릴을 즐기는 미식가.", partner: "ISFJ", bad: "INFJ", imgColor: "#E3F2FD" },
            "ESFP": { name: "짱아", desc: "반짝이는 게 좋아! 화려한 비주얼의 디저트나 신상 야식은 무조건 사진 찍고 먹어야 직성이 풀려요.", partner: "ISFJ", bad: "INTJ", imgColor: "#FFF3E0" },
            "ENFP": { name: "짱구", desc: "자유로운 영혼! '호호이~ 예쁜 누나, 피망은 싫어요!' 하고 싶은 대로 하는 분위기 메이커.", partner: "INFJ", bad: "ISTJ", imgColor: "#FFEBEE" },
            "ENTP": { name: "부리부리 대마왕", desc: "이익을 위해서라면 배신도 한다? 농담이고, 언변이 뛰어나고 기발한 아이디어로 야식 협상을 주도해요.", partner: "INFJ", bad: "ISFJ", imgColor: "#F3E5F5" },
            
            "ESTJ": { name: "붉은장미 삼총사 리더", desc: "카리스마 넘치는 리더! '오늘 야식은 족발로 통일한다. 불만 없지?' 효율적인 주문을 주도합니다.", partner: "ISFP", bad: "INFP", imgColor: "#FBE9E7" },
            "ESFJ": { name: "나미리 선생님", desc: "남들의 시선을 즐기는 당신! 비싼 브랜드 야식이나 유행하는 음식은 꼭 먹어봐야 해요.", partner: "ISFP", bad: "INTP", imgColor: "#E0F2F1" },
            "ENFJ": { name: "이슬이 누나", desc: "천사 같은 마음씨! 모두가 만족할만한 야식 메뉴를 고르느라 행복한 고민을 하는 평화주의자.", partner: "INFP", bad: "ISTP", imgColor: "#F1F8E9" },
            "ENTJ": { name: "채성아 선생님", desc: "열정적인 지도자! 맛집 탐방 동호회를 만들어서라도 체계적으로 맛있는 걸 찾아다니는 스타일.", partner: "INTP", bad: "ISFP", imgColor: "#FFFDE7" }
        };

        // --------------------------------------------------------
        // 3. 로직 처리
        // --------------------------------------------------------
        let currentStep = 0;
        let scores = { E: 0, I: 0, S: 0, N: 0, T: 0, F: 0, J: 0, P: 0 };

        function goToGame() {
            document.getElementById('intro-screen').classList.remove('active');
            document.getElementById('game-screen').classList.add('active');
            currentStep = 0;
            renderQuestion();
        }

        function renderQuestion() {
            const q = questions[currentStep];
            
            // 프로그레스바
            const progress = ((currentStep + 1) / questions.length) * 100;
            document.getElementById('progress-bar').style.width = `${progress}%`;
            document.getElementById('q-num').innerText = currentStep + 1;
            
            // 텍스트 애니메이션 리셋
            const qText = document.getElementById('q-text');
            qText.innerText = q.q;
            document.getElementById('a-text').innerText = q.a;
            document.getElementById('b-text').innerText = q.b;
        }

        function answer(choice) {
            const q = questions[currentStep];
            const type = q.type; // 'EI', 'SN', 'TF', 'JP'
            
            // 점수 로직 (A선택 시 앞글자 점수+, B선택 시 뒷글자 점수+)
            // 예: EI 타입에서 A선택 -> E+1, B선택 -> I+1
            const first = type[0];
            const second = type[1];

            if (choice === 'A') scores[first]++;
            else scores[second]++;

            currentStep++;

            if (currentStep < questions.length) {
                renderQuestion();
            } else {
                showLoading();
            }
        }

        function showLoading() {
            document.getElementById('game-screen').classList.remove('active');
            document.getElementById('loading-screen').classList.add('active');
            
            // 1.5초 후 결과 표시
            setTimeout(() => {
                document.getElementById('loading-screen').classList.remove('active');
                showResult();
            }, 1500);
        }

        function showResult() {
            document.getElementById('result-screen').classList.add('active');

            // MBTI 조합 계산
            let mbti = "";
            mbti += scores.E >= scores.I ? "E" : "I";
            mbti += scores.S >= scores.N ? "S" : "N";
            mbti += scores.T >= scores.F ? "T" : "F";
            mbti += scores.J >= scores.P ? "J" : "P";

            const data = results[mbti];

            // 데이터 바인딩
            document.getElementById('result-mbti').innerText = mbti;
            document.getElementById('result-char-name').innerText = data.name;
            document.getElementById('result-desc').innerText = data.desc;
            document.getElementById('partner-good').innerText = results[data.partner].name;
            document.getElementById('partner-bad').innerText = results[data.bad].name;

            // 캐릭터 이미지 배경색 설정 (분위기 맞춤)
            const imgContainer = document.getElementById('char-img-container');
            imgContainer.style.backgroundColor = data.imgColor;

            // ★ 중요: 여기에 실제 이미지 URL을 매핑하면 이미지가 뜹니다.
            // 예시: if(mbti === 'ENFP') imgTag.src = '짱구이미지주소.jpg';
            // 현재는 저작권 이슈로 텍스트로 대체합니다.
            imgContainer.innerHTML = `<span class="text-3xl font-bold text-gray-400 select-none">${data.name}</span>`;
        }
    </script>
</body>
</html>
