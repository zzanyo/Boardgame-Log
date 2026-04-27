
# Boardgame-Log<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BoardGame Log - Home Restored</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@300;400;700;900&display=swap');
        body { font-family: 'Noto Sans KR', sans-serif; background-color: #F8F9FD; color: #111827; margin: 0; padding: 0; }
        .purple-point { color: #8B5CF6; }
        .bg-purple-point { background-color: #8B5CF6; }
        .hidden-section { display: none; }
        .card-shadow { box-shadow: 0 10px 25px -5px rgba(139, 92, 246, 0.1), 0 8px 10px -6px rgba(139, 92, 246, 0.1); }
        .nav-active { color: #8B5CF6 !important; font-weight: 900; }
        .content-padding { padding-bottom: 100px; }
        button:disabled { background-color: #E5E7EB !important; color: #9CA3AF !important; cursor: not-allowed; transform: none !important; }
    </style>
</head>
<body>

    <header class="bg-white/80 backdrop-blur-md p-4 sticky top-0 z-[100] flex justify-between items-center border-b border-purple-50">
        <h1 class="text-xl font-black tracking-tighter cursor-pointer" onclick="showSection('home')">
            <i class="fas fa-dice-d20 purple-point mr-1"></i> BG <span class="purple-point">LOG</span>
        </h1>
        <button onclick="showSection('search')" class="bg-purple-point text-white w-10 h-10 rounded-full shadow-lg flex items-center justify-center active:scale-95 transition-transform">
            <i class="fas fa-search"></i>
        </button>
    </header>

    <main id="app" class="p-5 max-w-md mx-auto content-padding">
        
        <section id="home-section">
            <div class="mb-8">
                <h2 class="text-2xl font-bold text-gray-800">안녕하세요! 👋</h2>
                <p class="text-gray-400 text-sm">오늘도 즐거운 게임 라이프 되세요.</p>
            </div>

            <div class="grid grid-cols-2 gap-4 mb-8">
                <div class="bg-white p-6 rounded-[2rem] card-shadow border border-white">
                    <p class="text-[10px] text-gray-400 font-bold uppercase mb-1 tracking-widest">Collection</p>
                    <p class="text-3xl font-black purple-point" id="stat-total">0</p>
                </div>
                <div class="bg-white p-6 rounded-[2rem] card-shadow border border-white">
                    <p class="text-[10px] text-gray-400 font-bold uppercase mb-1 tracking-widest">Plays</p>
                    <p class="text-3xl font-black text-blue-500" id="stat-plays">0</p>
                </div>
            </div>

            <div class="mb-8">
                <h3 class="font-bold text-gray-700 ml-1 mb-3">바로가기</h3>
                <div class="grid grid-cols-3 gap-3">
                    <button onclick="showSection('collection', 'owned')" class="bg-white p-4 rounded-2xl shadow-sm border border-purple-50 flex flex-col items-center">
                        <i class="fas fa-box purple-point mb-2"></i>
                        <span class="text-[10px] font-bold">보유 목록</span>
                    </button>
                    <button onclick="showSection('collection', 'played')" class="bg-white p-4 rounded-2xl shadow-sm border border-purple-50 flex flex-col items-center">
                        <i class="fas fa-check-circle text-green-500 mb-2"></i>
                        <span class="text-[10px] font-bold">해봄 목록</span>
                    </button>
                    <button onclick="showSection('collection', 'wish')" class="bg-white p-4 rounded-2xl shadow-sm border border-purple-50 flex flex-col items-center">
                        <i class="fas fa-heart text-pink-500 mb-2"></i>
                        <span class="text-[10px] font-bold">위시리스트</span>
                    </button>
                </div>
            </div>

            <div class="space-y-4">
                <h3 class="font-bold text-gray-700 ml-1">최근 활동</h3>
                <div id="recent-log-card" class="bg-gray-100 p-10 rounded-[2rem] text-center text-gray-400 text-sm italic">
                    최근 기록이 없습니다. 게임을 즐기고 기록해보세요!
                </div>
            </div>
        </section>

        <section id="search-section" class="hidden-section">
            <div class="flex items-center gap-3 mb-6">
                <button onclick="showSection('home')" class="text-gray-400"><i class="fas fa-arrow-left"></i></button>
                <h2 class="text-xl font-bold">게임을 검색해서 보유 목록에 추가</h2>
            </div>
            <div class="relative mb-6">
                <input type="text" id="search-input" oninput="handleLocalSearch()" placeholder="게임 이름 입력 (예: 스플렌더)" 
                    class="w-full bg-white border-2 border-purple-50 p-4 rounded-2xl pl-12 transition-all shadow-sm focus:border-purple-500 focus:outline-none">
                <i class="fas fa-search absolute left-4 top-5 text-purple-200"></i>
            </div>
            <div id="search-results" class="space-y-3"></div>
        </section>

        <section id="collection-section" class="hidden-section">
            <div class="flex justify-around mb-6 bg-purple-50 p-1 rounded-2xl">
                <button onclick="filterCollection('owned')" id="tab-owned" class="py-3 flex-1 rounded-xl text-sm font-bold">보유</button>
                <button onclick="filterCollection('played')" id="tab-played" class="py-3 flex-1 rounded-xl text-sm font-bold">해봄</button>
                <button onclick="filterCollection('wish')" id="tab-wish" class="py-3 flex-1 rounded-xl text-sm font-bold">위시</button>
            </div>
            <div id="collection-list" class="space-y-3"></div>
        </section>

        <section id="timeline-section" class="hidden-section">
            <h2 class="text-xl font-bold mb-6">플레이 타임라인 📜</h2>
            <div id="timeline-list" class="relative border-l-2 border-purple-100 ml-4 space-y-8"></div>
        </section>
    </main>

    <nav id="bottom-nav" class="fixed bottom-0 left-0 right-0 bg-white/90 backdrop-blur-md border-t border-gray-100 flex justify-around p-4 z-[500] shadow-[0_-5px_20px_rgba(0,0,0,0.05)]">
        <button onclick="showSection('home')" class="nav-btn nav-active flex flex-col items-center flex-1">
            <i class="fas fa-home text-xl"></i><span class="text-[10px] mt-1 font-bold">HOME</span>
        </button>
        <button onclick="showSection('collection')" class="nav-btn text-gray-400 flex flex-col items-center flex-1">
            <i class="fas fa-layer-group text-xl"></i><span class="text-[10px] mt-1 font-bold">GAMES</span>
        </button>
        <button onclick="showSection('timeline')" class="nav-btn text-gray-400 flex flex-col items-center flex-1">
            <i class="fas fa-history text-xl"></i><span class="text-[10px] mt-1 font-bold">HISTORY</span>
        </button>
    </nav>

    <div id="detail-modal" class="fixed inset-0 bg-black/50 z-[1000] hidden flex items-center justify-center p-6 backdrop-blur-sm">
        <div class="bg-white rounded-[2.5rem] w-full max-w-sm p-8 relative shadow-2xl overflow-y-auto max-h-[85vh]">
            <button onclick="closeModal()" class="absolute top-6 right-6 text-gray-300 text-2xl hover:text-gray-500">&times;</button>
            <div id="modal-content"></div>
        </div>
    </div>

    <div id="log-modal" class="fixed inset-0 bg-black/50 z-[1000] hidden flex items-center justify-center p-6 backdrop-blur-sm">
        <div class="bg-white rounded-[2.5rem] w-full max-w-sm p-8 relative shadow-2xl">
            <button onclick="closeModal()" class="absolute top-6 right-6 text-gray-300 text-2xl hover:text-gray-500">&times;</button>
            
            <h3 class="text-lg font-black mb-1 text-center">기록하기 ✍️</h3>
            <p id="validation-msg" class="text-[11px] text-center text-red-400 mb-4 font-bold">필수 항목을 모두 채워주세요.</p>
            
            <form id="log-form" class="space-y-4">
                <input type="hidden" id="log-game-id">
                
                <div class="grid grid-cols-2 gap-2">
                    <div>
                        <label class="text-[10px] font-bold text-gray-400 ml-1">날짜 *</label>
                        <input type="date" id="log-date" class="w-full bg-gray-50 border-none p-4 rounded-2xl font-bold text-sm" required>
                    </div>
                    <div>
                        <label class="text-[10px] font-bold text-gray-400 ml-1">별점 *</label>
                        <select id="log-rating" class="w-full bg-gray-50 p-4 rounded-2xl font-bold text-yellow-500 text-sm" required>
                            <option value="">선택</option>
                            <option value="5">⭐⭐⭐⭐⭐</option>
                            <option value="4">⭐⭐⭐⭐</option>
                            <option value="3">⭐⭐⭐</option>
                            <option value="2">⭐⭐</option>
                            <option value="1">⭐</option>
                        </select>
                    </div>
                </div>
                
                <div class="flex gap-2">
                    <div class="w-1/2">
                        <label class="text-[10px] font-bold text-gray-400 ml-1">인원 *</label>
                        <input type="number" id="log-players" placeholder="명" class="w-full bg-gray-50 p-4 rounded-2xl text-sm" required>
                    </div>
                    <div class="w-1/2">
                        <label class="text-[10px] font-bold text-gray-400 ml-1">시간 *</label>
                        <input type="number" id="log-time" placeholder="분" class="w-full bg-gray-50 p-4 rounded-2xl text-sm" required>
                    </div>
                </div>

                <div>
                    <label class="text-[10px] font-bold text-gray-400 ml-1">승패 *</label>
                    <div class="flex items-center justify-around py-3 bg-gray-50 rounded-2xl font-bold text-sm mt-1">
                        <label class="cursor-pointer"><input type="radio" name="result" value="win" checked class="accent-purple-500"> 승리</label>
                        <label class="cursor-pointer"><input type="radio" name="result" value="loss" class="accent-purple-500"> 패배</label>
                    </div>
                </div>

                <div>
                    <label class="text-[10px] font-bold text-gray-400 ml-1">메모 (선택)</label>
                    <textarea id="log-memo" placeholder="오늘 게임은 어땠나요?" class="w-full bg-gray-50 p-4 rounded-2xl h-20 text-sm mt-1 resize-none"></textarea>
                </div>

                <button type="submit" id="save-log-btn" disabled class="w-full bg-purple-point text-white py-4 rounded-2xl font-black shadow-lg shadow-purple-100 active:scale-95 transition-transform">저장하기</button>
            </form>
        </div>
    </div>

    <script>
        // DB - 이전과 동일하게 유지
        const GAME_DB = [
            { id: "s-01", name: "스플렌더", en: "Splendor", weight: 1.79, year: 2014 },
            { id: "s-02", name: "카탄", en: "Catan", weight: 2.30, year: 1995 },
            { id: "s-03", name: "할리갈리", en: "Halli Galli", weight: 1.01, year: 1990 },
            { id: "s-04", name: "테라포밍 마스", en: "Terraforming Mars", weight: 3.25, year: 2016 },
            { id: "s-05", name: "루미큐브", en: "Rummikub", weight: 1.72, year: 1977 },
            { id: "s-06", name: "윙스팬", en: "Wingspan", weight: 2.45, year: 2019 },
            { id: "s-07", name: "아즈울", en: "Azul", weight: 1.76, year: 2017 },
            { id: "s-08", name: "코드네임", en: "Codenames", weight: 1.27, year: 2015 },
            { id: "s-09", name: "다빈치 코드", en: "Da Vinci Code", weight: 1.15, year: 2004 },
            { id: "s-10", name: "뱅!", en: "Bang!", weight: 1.63, year: 2002 }
        ];

        let collection = JSON.parse(localStorage.getItem('bg-coll-v6')) || [];
        let logs = JSON.parse(localStorage.getItem('bg-logs-v6')) || [];

        window.onload = () => {
            renderHome();
            document.getElementById('log-date').valueAsDate = new Date();
            initValidation();
        };

        function showSection(id, filter = 'owned') {
            document.querySelectorAll('main > section').forEach(s => s.classList.add('hidden-section'));
            const target = document.getElementById(`${id}-section`);
            if(target) target.classList.remove('hidden-section');
            
            document.querySelectorAll('.nav-btn').forEach(b => {
                b.classList.remove('nav-active');
                b.classList.add('text-gray-400');
            });
            const navMap = { 'home': 0, 'collection': 1, 'timeline': 2 };
            if(navMap[id] !== undefined) {
                const activeBtn = document.querySelectorAll('.nav-btn')[navMap[id]];
                activeBtn.classList.add('nav-active');
                activeBtn.classList.remove('text-gray-400');
            }

            if(id === 'home') renderHome();
            if(id === 'collection') filterCollection(filter);
            if(id === 'timeline') renderTimeline();
            window.scrollTo(0,0);
        }

        function handleLocalSearch() {
            const input = document.getElementById('search-input').value.toLowerCase().trim();
            const resDiv = document.getElementById('search-results');
            if(!input) { resDiv.innerHTML = ""; return; }
            const filtered = GAME_DB.filter(g => g.name.includes(input) || g.en.toLowerCase().includes(input));
            resDiv.innerHTML = filtered.length ? "" : `<div class="bg-white p-6 rounded-2xl text-center"><button onclick="addCustomGame('${input}')" class="text-purple-600 font-bold underline">'${input}' 직접 추가</button></div>`;
            
            filtered.forEach(g => {
                resDiv.innerHTML += `
                <div class="bg-white p-4 rounded-2xl shadow-sm flex items-center gap-4 cursor-pointer border border-transparent hover:border-purple-100" onclick="showDetail('${g.id}')">
                    <div class="w-12 h-12 bg-purple-50 rounded-xl flex items-center justify-center text-purple-500 font-black">${g.name[0]}</div>
                    <div class="flex-1">
                        <h4 class="font-bold text-gray-800">${g.name}</h4>
                        <p class="text-[10px] text-gray-400 uppercase">${g.en}</p>
                    </div>
                    <button onclick="event.stopPropagation(); addToColl('${g.id}', '${g.name}', 'owned')" 
                        class="bg-purple-600 text-white px-4 py-2 rounded-xl text-xs font-black shadow-md active:scale-90 transition-transform">
                        추가
                    </button>
                </div>`;
            });
        }

        // [수정] 추가 시 항상 '보유(owned)' 목록에 저장
        function addToColl(id, name, status = 'owned') {
            const existing = collection.find(g => g.id === id);
            if(existing) {
                alert('이미 콜렉션에 있는 게임입니다.');
                return;
            }
            collection.push({ id, name, status, weight: 2.0 });
            localStorage.setItem('bg-coll-v6', JSON.stringify(collection));
            alert(`'${name}' 게임을 보유 목록에 추가했습니다!`);
            renderHome();
        }

        function addCustomGame(name) {
            const id = "c-" + Date.now();
            collection.push({ id, name, status: 'owned', weight: 2.0 });
            localStorage.setItem('bg-coll-v6', JSON.stringify(collection));
            alert('보유 목록에 직접 추가되었습니다.');
            showSection('collection', 'owned');
        }

        function renderHome() {
            document.getElementById('stat-total').innerText = collection.length;
            document.getElementById('stat-plays').innerText = logs.length;
            const recent = document.getElementById('recent-log-card');
            if(logs.length > 0) {
                recent.className = "bg-white p-6 rounded-[2rem] card-shadow text-left flex items-center gap-4 border border-white";
                recent.innerHTML = `
                    <div class="bg-purple-600 w-14 h-14 rounded-2xl flex items-center justify-center text-white shadow-lg shadow-purple-100"><i class="fas fa-dice text-xl"></i></div>
                    <div>
                        <p class="text-[10px] text-gray-400 font-bold uppercase mb-0.5">${logs[0].date}</p>
                        <h4 class="font-bold text-gray-800 leading-tight">${logs[0].gameName}</h4>
                        <p class="text-xs text-purple-600 font-black tracking-tight">${logs[0].result === 'win' ? '승리 🎉' : '플레이 완료'} • ⭐${logs[0].rating}</p>
                    </div>`;
            } else {
                recent.innerHTML = "최근 기록이 없습니다. 게임을 즐기고 기록해보세요!";
                recent.className = "bg-gray-100 p-10 rounded-[2rem] text-center text-gray-400 text-sm italic";
            }
        }

        function filterCollection(status) {
            const tabs = ['owned', 'played', 'wish'];
            tabs.forEach(t => {
                const el = document.getElementById(`tab-${t}`);
                el.className = "py-3 flex-1 rounded-xl text-sm font-bold " + (t === status ? "bg-purple-600 text-white shadow-lg" : "text-gray-400");
            });
            const list = document.getElementById('collection-list');
            const items = collection.filter(g => g.status === status);
            list.innerHTML = items.length ? "" : `<div class="py-20 text-center text-gray-300 italic">비어 있습니다.</div>`;
            
            items.forEach(g => {
                list.innerHTML += `
                <div class="bg-white p-4 rounded-3xl flex items-center gap-4 card-shadow border border-white">
                    <div class="w-12 h-12 bg-gray-50 rounded-xl flex items-center justify-center text-gray-300 font-black text-xl">${g.name[0]}</div>
                    <div class="flex-1 font-bold text-gray-700 truncate">${g.name}</div>
                    <button onclick="openLogModal('${g.id}')" class="bg-purple-50 text-purple-600 px-4 py-2 rounded-xl text-xs font-black active:scale-95 transition-transform">기록</button>
                </div>`;
            });
        }

        function showDetail(id) {
            const g = GAME_DB.find(d => d.id === id);
            if(!g) return;
            const content = document.getElementById('modal-content');
            document.getElementById('detail-modal').classList.remove('hidden');
            content.innerHTML = `
                <div class="flex flex-col items-center">
                    <div class="w-24 h-24 bg-purple-100 rounded-3xl flex items-center justify-center text-4xl mb-4">🎲</div>
                    <h2 class="text-2xl font-black text-gray-800 mb-1 text-center">${g.name}</h2>
                    <p class="text-xs font-bold text-gray-400 uppercase tracking-widest mb-6">${g.en}</p>
                    <div class="w-full flex justify-between items-center bg-gray-50 p-5 rounded-3xl mb-8">
                        <div><p class="text-[10px] font-black text-gray-400 uppercase mb-1">Difficulty</p><p class="text-2xl font-black purple-point">${g.weight}</p></div>
                        <div class="w-14 h-14"><canvas id="wChart"></canvas></div>
                    </div>
                    <div class="grid grid-cols-3 gap-2 w-full">
                        <button onclick="addToColl('${g.id}','${g.name}','owned')" class="bg-gray-800 text-white py-3.5 rounded-2xl text-[10px] font-black">보유</button>
                        <button onclick="addToColl('${g.id}','${g.name}','played')" class="bg-purple-600 text-white py-3.5 rounded-2xl text-[10px] font-black">해봄</button>
                        <button onclick="addToColl('${g.id}','${g.name}','wish')" class="bg-pink-500 text-white py-3.5 rounded-2xl text-[10px] font-black">위시</button>
                    </div>
                </div>`;
            const ctx = document.getElementById('wChart').getContext('2d');
            new Chart(ctx, { type: 'doughnut', data: { datasets: [{ data: [g.weight, 5-g.weight], backgroundColor: ['#8B5CF6', '#E5E7EB'], borderWidth: 0 }] }, options: { cutout: '75%', plugins: { tooltip: { enabled: false } } } });
        }

        function openLogModal(id) {
            document.getElementById('log-game-id').value = id;
            document.getElementById('log-modal').classList.remove('hidden');
            validateForm();
        }

        function closeModal() {
            document.getElementById('detail-modal').classList.add('hidden');
            document.getElementById('log-modal').classList.add('hidden');
        }

        function initValidation() {
            ['log-date', 'log-players', 'log-time', 'log-rating'].forEach(id => {
                document.getElementById(id).addEventListener('input', validateForm);
            });
        }

        function validateForm() {
            const date = document.getElementById('log-date').value;
            const players = document.getElementById('log-players').value;
            const time = document.getElementById('log-time').value;
            const rating = document.getElementById('log-rating').value;
            const btn = document.getElementById('save-log-btn');
            const msg = document.getElementById('validation-msg');

            if (date && players && time && rating) {
                btn.disabled = false;
                msg.innerText = "모든 항목이 채워졌습니다! 저장 가능합니다. ✨";
                msg.className = "text-[11px] text-center text-green-500 mb-4 font-bold";
            } else {
                btn.disabled = true;
                msg.innerText = "빈칸인 항목을 채워주세요 (날짜, 인원, 시간, 별점 필수)";
                msg.className = "text-[11px] text-center text-red-400 mb-4 font-bold";
            }
        }

        document.getElementById('log-form').onsubmit = function(e) {
            e.preventDefault();
            const gameId = document.getElementById('log-game-id').value;
            const gIdx = collection.findIndex(c => c.id === gameId);
            const g = collection[gIdx];

            const log = { 
                id: Date.now(), 
                gameName: g.name, 
                date: document.getElementById('log-date').value, 
                players: document.getElementById('log-players').value, 
                time: document.getElementById('log-time').value, 
                result: document.querySelector('input[name="result"]:checked').value, 
                rating: document.getElementById('log-rating').value,
                memo: document.getElementById('log-memo').value 
            };

            logs.unshift(log);
            collection[gIdx].status = 'played'; // 자동으로 '해봄'으로 이동
            
            localStorage.setItem('bg-logs-v6', JSON.stringify(logs));
            localStorage.setItem('bg-coll-v6', JSON.stringify(collection));

            closeModal();
            this.reset();
            renderHome();
            alert('기록 완료 및 "해봄" 목록으로 이동되었습니다.');
        };

        function renderTimeline() {
            const list = document.getElementById('timeline-list');
            list.innerHTML = logs.length ? "" : `<div class="py-20 text-center text-gray-300 italic">기록이 없습니다.</div>`;
            logs.forEach(l => {
                list.innerHTML += `
                <div class="mb-8 ml-8 bg-white p-6 rounded-[2.5rem] card-shadow border border-white relative">
                    <div class="absolute -left-[43px] top-6 bg-purple-600 w-5 h-5 rounded-full border-4 border-white shadow-md"></div>
                    <p class="text-[10px] font-black purple-point mb-1 uppercase tracking-widest">${l.date}</p>
                    <h4 class="font-extrabold text-gray-800 text-lg leading-tight mb-2">${l.gameName}</h4>
                    <div class="flex gap-4 text-[11px] font-bold text-gray-400">
                        <span><i class="fas fa-user-friends mr-1"></i>${l.players}명</span>
                        <span><i class="fas fa-clock mr-1"></i>${l.time}분</span>
                        <span class="${l.result === 'win' ? 'text-blue-500' : 'text-red-400'} font-black">#${l.result.toUpperCase()}</span>
                        <span class="text-yellow-500 font-bold ml-auto">⭐ ${l.rating}</span>
                    </div>
                    ${l.memo ? `<div class="mt-4 p-4 bg-gray-50 rounded-2xl text-[11px] text-gray-500 italic leading-relaxed">"${l.memo}"</div>` : ''}
                </div>`;
            });
        }
    </script>
</body>
</html>
