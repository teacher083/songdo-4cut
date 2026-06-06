# songdo-4cut
부산관광고 인생네컷
```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>송도 인생네컷 - 부산관광고 학부모 데이</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Font Awesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Gowun+Dodum:wght@400;700&family=Black+Han+Sans&display=swap" rel="stylesheet">
    
    <style>
        body {
            font-family: 'Gowun Dodum', sans-serif;
            background: linear-gradient(135deg, #f0f9ff 0%, #e0f2fe 50%, #bae6fd 100%);
            min-height: 100vh;
            -webkit-tap-highlight-color: transparent;
        }
        .title-font {
            font-family: 'Black Han Sans', sans-serif;
        }
        .glass-panel {
            background: rgba(255, 255, 255, 0.96);
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.5);
        }
        .four-cuts-container {
            width: 320px;
            background-color: #ffffff;
            box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.25);
            border: 12px solid #ffffff;
            border-radius: 4px;
            transition: all 0.3s ease;
        }
        .photo-slot {
            aspect-ratio: 4 / 3;
            background-color: #f1f5f9;
            overflow: hidden;
            position: relative;
            border: 2px solid transparent;
            cursor: pointer;
            transition: opacity 0.15s ease;
        }
        .photo-slot.active-slot {
            border: 3px dashed #0284c7;
            box-shadow: 0 0 10px rgba(2, 132, 199, 0.4);
        }
        .sticker-3d {
            position: absolute;
            user-select: none;
            cursor: move;
            touch-action: none;
            z-index: 50;
            font-size: 2.3rem;
            filter: drop-shadow(2px 5px 4px rgba(0,0,0,0.3));
        }
        .camera-preview-element {
            transform: scaleX(-1);
            object-fit: cover;
            width: 100%;
            height: 100%;
            position: absolute;
            inset: 0;
            z-index: 10;
        }
        .captured-snapshot-canvas {
            object-fit: cover;
            width: 100%;
            height: 100%;
            position: absolute;
            inset: 0;
            z-index: 20;
        }
        .songdo-design-overlay {
            position: absolute;
            inset: 0;
            pointer-events: none;
            z-index: 35;
        }
        .timer-overlay {
            position: absolute;
            inset: 0;
            background-color: rgba(0, 0, 0, 0.6);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 45;
        }
        .timer-number {
            font-family: 'Black Han Sans', sans-serif;
            font-size: 5rem;
            color: #ffffff;
            text-shadow: 0 0 20px rgba(0, 0, 0, 0.8);
        }
    </style>
</head>
<body class="py-4 px-2 md:px-4 flex flex-col items-center justify-between">

    <!-- Header -->
    <header class="w-full max-w-4xl text-center mb-4">
        <div class="inline-block bg-white/95 px-5 py-1.5 rounded-full shadow-sm mb-2 border border-sky-100">
            <span class="text-sky-600 font-bold text-xs md:text-sm tracking-wider"><i class="fa-solid fa-graduation-cap mr-1"></i> 부산관광고등학교 중3 학부모 데이</span>
        </div>
        <h1 class="text-3xl md:text-5xl title-font text-sky-900 tracking-tight drop-shadow-sm">
            🌊 송도 바다 <span class="text-pink-500">인생네컷</span>
        </h1>
        <p class="text-gray-700 mt-1.5 text-xs md:text-sm font-semibold">
            ✨ 촬영을 완료하는 즉시 스마트폰으로 다운로드 가능한 진짜 고화질 QR코드가 자동 생성됩니다!
        </p>
    </header>

    <!-- Main Workspace -->
    <main class="w-full max-w-5xl grid grid-cols-1 lg:grid-cols-12 gap-6 items-start justify-center">
        
        <!-- Controls Column (Left) -->
        <section class="lg:col-span-7 flex flex-col gap-4 w-full">
            
            <!-- Indicators -->
            <div class="glass-panel rounded-2xl p-4 shadow-sm flex justify-between items-center text-xs font-semibold text-gray-500">
                <div class="flex items-center gap-1.5 text-sky-600 font-bold" id="step1-tab">
                    <span class="w-5 h-5 rounded-full bg-sky-100 flex items-center justify-center text-xs">1</span>
                    <span>송도 프레임 선택</span>
                </div>
                <div class="h-px bg-gray-300 flex-1 mx-2"></div>
                <div class="flex items-center gap-1.5 text-gray-400" id="step2-tab">
                    <span class="w-5 h-5 rounded-full bg-gray-100 flex items-center justify-center text-xs">2</span>
                    <span>4컷 자동 연속촬영</span>
                </div>
                <div class="h-px bg-gray-300 flex-1 mx-2"></div>
                <div class="flex items-center gap-1.5 text-gray-400" id="step3-tab">
                    <span class="w-5 h-5 rounded-full bg-gray-100 flex items-center justify-center text-xs">3</span>
                    <span>꾸미기 & QR 소장</span>
                </div>
            </div>

            <!-- Controller Card -->
            <div class="glass-panel rounded-2xl p-5 shadow-md flex flex-col gap-4">
                
                <!-- Workspace Tabs -->
                <div class="flex border-b border-gray-200">
                    <button onclick="switchTab('frame')" id="tab-frame" class="py-2.5 px-3 font-bold text-sky-600 border-b-2 border-sky-600 focus:outline-none transition flex-1 text-center text-xs md:text-sm">
                        🏖️ 송도 프레임 테마
                    </button>
                    <button onclick="switchTab('camera')" id="tab-camera" class="py-2.5 px-3 font-bold text-gray-500 hover:text-sky-600 border-b-2 border-transparent focus:outline-none transition flex-1 text-center text-xs md:text-sm">
                        📷 4컷 연속 촬영
                    </button>
                    <button onclick="switchTab('sticker')" id="tab-sticker" class="py-2.5 px-3 font-bold text-gray-500 hover:text-sky-600 border-b-2 border-transparent focus:outline-none transition flex-1 text-center text-xs md:text-sm">
                        🧸 3D 꾸미기 스티커
                    </button>
                </div>

                <!-- TAB 1: Songdo Frame Customizer -->
                <div id="panel-frame" class="flex flex-col gap-4">
                    <h3 class="font-bold text-gray-800 text-sm md:text-base">1. 인생네컷 프레임 배경 색상</h3>
                    <div class="grid grid-cols-5 gap-2">
                        <button onclick="changeFrameColor('#ffffff', 'text-sky-950')" class="py-2 rounded-lg border border-gray-300 bg-white text-xs font-bold text-gray-700">클래식 화이트</button>
                        <button onclick="changeFrameColor('#1e293b', 'text-white')" class="py-2 rounded-lg bg-slate-800 text-xs font-bold text-white">시크 블랙</button>
                        <button onclick="changeFrameColor('#fdf2f8', 'text-pink-700')" class="py-2 rounded-lg bg-pink-50 text-xs font-bold text-pink-700">소프트 핑크</button>
                        <button onclick="changeFrameColor('#f0f9ff', 'text-sky-800')" class="py-2 rounded-lg bg-sky-50 text-xs font-bold text-sky-800">마린 스카이</button>
                        <button onclick="changeFrameColor('#fefcbf', 'text-yellow-850')" class="py-2 rounded-lg bg-yellow-50 text-xs font-bold text-yellow-800">샌드 옐로우</button>
                    </div>

                    <h3 class="font-bold text-gray-800 text-sm md:text-base mt-2">2. 송도 명소 테마 디자인 프레임 선택</h3>
                    <div class="grid grid-cols-2 md:grid-cols-4 gap-2">
                        <button onclick="selectSongdoTheme('theme-cable')" id="theme-btn-cable" class="flex flex-col items-center p-2 rounded-xl border-2 border-sky-400 bg-sky-50/50 hover:bg-sky-50 transition text-center text-xs">
                            <span class="text-xl">🚠</span>
                            <span class="text-[11px] font-bold mt-1 text-sky-900">송도 케이블카</span>
                        </button>
                        <button onclick="selectSongdoTheme('theme-beach')" id="theme-btn-beach" class="flex flex-col items-center p-2 rounded-xl border border-gray-200 hover:border-sky-300 transition text-center text-xs">
                            <span class="text-xl">🏖️</span>
                            <span class="text-[11px] font-bold mt-1 text-gray-700">송도 은빛해변</span>
                        </button>
                        <button onclick="selectSongdoTheme('theme-lighthouse')" id="theme-btn-lighthouse" class="flex flex-col items-center p-2 rounded-xl border border-gray-200 hover:border-sky-300 transition text-center text-xs">
                            <span class="text-xl">🚨</span>
                            <span class="text-[11px] font-bold mt-1 text-gray-700">송도 빨간등대</span>
                        </button>
                        <button onclick="selectSongdoTheme('theme-cloud')" id="theme-btn-cloud" class="flex flex-col items-center p-2 rounded-xl border border-gray-200 hover:border-sky-300 transition text-center text-xs">
                            <span class="text-xl">🌉</span>
                            <span class="text-[11px] font-bold mt-1 text-gray-700">구름산책로</span>
                        </button>
                    </div>
                    <button onclick="switchTab('camera')" class="w-full bg-sky-600 hover:bg-sky-700 text-white font-bold py-3 rounded-xl text-xs mt-2 transition">
                        실시간 카메라 켜고 촬영하기 <i class="fa-solid fa-arrow-right ml-1"></i>
                    </button>
                </div>

                <!-- TAB 2: Camera & Shoot -->
                <div id="panel-camera" class="hidden flex flex-col gap-3">
                    <div class="bg-blue-50 p-3 rounded-xl border border-blue-100">
                        <p class="text-xs text-blue-900 leading-relaxed font-semibold">
                            ⏰ <strong>인생네컷 4컷 연속 타이머 촬영!</strong><br>
                            - 아래 버튼을 누르면 <strong>1번부터 4번까지 자동으로 5초 간격 연속 촬영</strong>이 진행됩니다.<br>
                            - 촬영 직후 자동으로 고화질 QR코드 생성기가 즉시 실행됩니다.
                        </p>
                    </div>

                    <!-- Hidden device stream captures -->
                    <video id="webcam-hardware" autoplay playsinline muted class="hidden"></video>

                    <!-- Real Shooting Trigger -->
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-3 pt-1">
                        <button id="start-capture-btn" onclick="startTimerAndCapture()" class="bg-gradient-to-r from-pink-500 to-rose-500 hover:from-pink-600 hover:to-rose-600 text-white font-bold py-4 px-4 rounded-xl text-sm shadow-md transition flex items-center justify-center gap-2">
                            <i class="fa-solid fa-camera text-lg animate-pulse"></i> 📸 4컷 자동 촬영 시작!
                        </button>
                        <button onclick="triggerFileSelect()" class="bg-indigo-600 hover:bg-indigo-700 text-white font-bold py-4 px-4 rounded-xl text-sm shadow transition flex items-center justify-center gap-2">
                            <i class="fa-solid fa-file-arrow-up text-lg"></i> 개별 사진 불러오기
                        </button>
                    </div>

                    <div class="mt-3">
                        <p class="text-[11px] text-gray-500 font-semibold mb-1">※ 재촬영하고 싶은 특정 슬롯을 누른 다음 수동으로 교환할 수 있습니다.</p>
                        <div class="grid grid-cols-4 gap-2">
                            <button onclick="setActiveSlot(1)" id="slot-btn-1" class="py-1.5 rounded-lg text-xs font-bold bg-sky-600 text-white border-2 border-sky-600 transition shadow-sm">1번 컷</button>
                            <button onclick="setActiveSlot(2)" id="slot-btn-2" class="py-1.5 rounded-lg text-xs font-bold bg-white text-gray-600 border border-gray-200 transition">2번 컷</button>
                            <button onclick="setActiveSlot(3)" id="slot-btn-3" class="py-1.5 rounded-lg text-xs font-bold bg-white text-gray-600 border border-gray-200 transition">3번 컷</button>
                            <button onclick="setActiveSlot(4)" id="slot-btn-4" class="py-1.5 rounded-lg text-xs font-bold bg-white text-gray-600 border border-gray-200 transition">4번 컷</button>
                        </div>
                    </div>

                    <input type="file" id="file-uploader" accept="image/*" class="hidden" onchange="handleFileSelect(event)">
                </div>

                <!-- TAB 3: Decorations -->
                <div id="panel-sticker" class="hidden flex flex-col gap-3">
                    <p class="text-xs text-gray-500 leading-tight">스티커를 터치해서 생성한 뒤, 슬롯 내부를 드래그하여 인생네컷 프레임을 마음껏 꾸며보세요!</p>
                    
                    <div class="grid grid-cols-4 sm:grid-cols-6 gap-2 p-2 bg-gray-50 rounded-xl border border-gray-200 max-h-[140px] overflow-y-auto">
                        <button onclick="addSticker('🚠')" class="p-2 bg-white rounded-lg border hover:bg-slate-50 text-2xl flex flex-col items-center">🚠<span class="text-[9px] text-gray-500">케이블</span></button>
                        <button onclick="addSticker('⛱️')" class="p-2 bg-white rounded-lg border hover:bg-slate-50 text-2xl flex flex-col items-center">⛱️<span class="text-[9px] text-gray-500">파라솔</span></button>
                        <button onclick="addSticker('🏄')" class="p-2 bg-white rounded-lg border hover:bg-slate-50 text-2xl flex flex-col items-center">🏄<span class="text-[9px] text-gray-500">서핑</span></button>
                        <button onclick="addSticker('⛵')" class="p-2 bg-white rounded-lg border hover:bg-slate-50 text-2xl flex flex-col items-center">⛵<span class="text-[9px] text-gray-500">요트</span></button>
                        <button onclick="addSticker('🛟')" class="p-2 bg-white rounded-lg border hover:bg-slate-50 text-2xl flex flex-col items-center">🛟<span class="text-[9px] text-gray-500">튜브</span></button>
                        <button onclick="addSticker('🦀')" class="p-2 bg-white rounded-lg border hover:bg-slate-50 text-2xl flex flex-col items-center">🦀<span class="text-[9px] text-gray-500">꽃게</span></button>
                        <button onclick="addSticker('🦤')" class="p-2 bg-white rounded-lg border hover:bg-slate-50 text-2xl flex flex-col items-center">🦤<span class="text-[9px] text-gray-500">갈매기</span></button>
                        <button onclick="addSticker('🐬')" class="p-2 bg-white rounded-lg border hover:bg-slate-50 text-2xl flex flex-col items-center">🐬<span class="text-[9px] text-gray-500">돌고래</span></button>
                        <button onclick="addSticker('🌴')" class="p-2 bg-white rounded-lg border hover:bg-slate-50 text-2xl flex flex-col items-center">🌴<span class="text-[9px] text-gray-500">야자수</span></button>
                        <button onclick="addSticker('🎓')" class="p-2 bg-white rounded-lg border hover:bg-slate-50 text-2xl flex flex-col items-center">🎓<span class="text-[9px] text-gray-500">학사모</span></button>
                        <button onclick="addSticker('👨‍👩‍👧')" class="p-2 bg-white rounded-lg border hover:bg-slate-50 text-2xl flex flex-col items-center">👨‍👩‍👧<span class="text-[9px] text-gray-500">가족</span></button>
                        <button onclick="addSticker('💖')" class="p-2 bg-white rounded-lg border hover:bg-slate-50 text-2xl flex flex-col items-center">💖<span class="text-[9px] text-gray-500">하트</span></button>
                    </div>

                    <div class="grid grid-cols-2 gap-2 mt-1">
                        <button onclick="clearStickersInActive()" class="bg-red-50 hover:bg-red-100 text-red-700 py-2.5 rounded-xl text-xs font-bold transition">
                            <i class="fa-solid fa-trash-can mr-1"></i> 현재 컷 스티커 전체 삭제
                        </button>
                        <button onclick="switchTab('camera')" class="bg-gray-100 hover:bg-gray-200 text-gray-700 py-2.5 rounded-xl text-xs font-bold transition">
                            <i class="fa-solid fa-camera mr-1"></i> 카메라도 가동
                        </button>
                    </div>
                </div>

            </div>

            <!-- Instant QR & Downloader Area -->
            <div class="glass-panel rounded-2xl p-5 shadow-md border border-sky-100">
                <h3 class="font-bold text-gray-800 text-sm md:text-base mb-1 flex items-center gap-1.5 text-sky-900">
                    <i class="fa-solid fa-cloud-arrow-up text-xl text-emerald-600 animate-pulse"></i> 📱 초고화질 QR 다운로드 & 시스템 완벽 결합
                </h3>
                <p class="text-[11px] text-gray-500 mb-4 leading-relaxed">
                    로컬 병합 가상화 렌더러와 고성능 클라우드 업로드 엔진이 탑재되어 스마트폰, 아이패드, 크롬북 갤러리에 오차 없이 무조건 원본 인생네컷을 전송 저장합니다.
                </p>

                <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
                    <button onclick="generateHighQualityQR()" class="bg-amber-400 hover:bg-amber-500 text-amber-950 font-bold py-3.5 px-4 rounded-xl text-sm transition flex items-center justify-center gap-2 shadow-md">
                        <i class="fa-solid fa-qrcode text-lg"></i>
                        인생네컷 진짜 QR코드 띄우기
                    </button>
                    <button onclick="saveToGalleryLocal()" class="bg-sky-600 hover:bg-sky-700 text-white font-bold py-3.5 px-4 rounded-xl text-sm transition flex items-center justify-center gap-2 shadow-sm">
                        <i class="fa-solid fa-download text-lg"></i>
                        갤러리 즉시 다운로드 저장
                    </button>
                </div>
            </div>

        </section>

        <!-- Right Preview Column -->
        <section class="lg:col-span-5 flex flex-col items-center justify-center w-full">
            <h3 class="text-sky-950 font-bold mb-2 flex items-center gap-2 text-xs md:text-sm">
                <i class="fa-solid fa-image text-sky-600"></i> 인생네컷 실시간 피드백 프레임
            </h3>

            <!-- 4-Cuts View Frame -->
            <div id="four-cuts-frame" class="four-cuts-container flex flex-col gap-3 p-1">
                
                <!-- Slot 1 -->
                <div onclick="setActiveSlot(1)" id="slot-1" class="photo-slot active-slot">
                    <video id="live-stream-1" autoplay playsinline muted class="camera-preview-element hidden"></video>
                    <canvas id="slot-canvas-1" class="captured-snapshot-canvas"></canvas>
                    
                    <div class="songdo-design-overlay">
                        <div class="absolute top-2 left-2 bg-sky-600/95 text-white px-2 py-0.5 rounded-full text-[10px] font-bold flex items-center gap-1 shadow-md z-40 songdo-theme-item-box">
                            <span class="songdo-theme-icon">🚀</span>
                            <span class="songdo-theme-text font-serif">SONGDO</span>
                        </div>
                        <div class="absolute bottom-2 right-2 bg-sky-900/95 text-white px-1.5 py-0.5 rounded text-[8.5px] font-bold tracking-wider flex items-center gap-1 shadow-md z-40 songdo-theme-item-box2">
                            <span>송도해수욕장</span>
                            <span class="songdo-theme-sub-icon">🌊</span>
                        </div>
                    </div>

                    <div id="timer-layer-1" class="timer-overlay hidden">
                        <span id="timer-text-1" class="timer-number">5</span>
                    </div>

                    <div class="absolute inset-0 flex flex-col items-center justify-center text-slate-400 text-center p-3 z-0 empty-tip">
                        <span class="text-xl">📸</span>
                        <span class="text-[10px] font-bold mt-1">1번째 컷 촬영 영역</span>
                    </div>
                    <div class="absolute inset-0 z-40 pointer-events-auto sticker-area"></div>
                </div>

                <!-- Slot 2 -->
                <div onclick="setActiveSlot(2)" id="slot-2" class="photo-slot">
                    <video id="live-stream-2" autoplay playsinline muted class="camera-preview-element hidden"></video>
                    <canvas id="slot-canvas-2" class="captured-snapshot-canvas"></canvas>
                    
                    <div class="songdo-design-overlay">
                        <div class="absolute top-2 left-2 bg-sky-600/95 text-white px-2 py-0.5 rounded-full text-[10px] font-bold flex items-center gap-1 shadow-md z-40 songdo-theme-item-box">
                            <span class="songdo-theme-icon">🚀</span>
                            <span class="songdo-theme-text font-serif">SONGDO</span>
                        </div>
                        <div class="absolute bottom-2 right-2 bg-sky-900/95 text-white px-1.5 py-0.5 rounded text-[8.5px] font-bold tracking-wider flex items-center gap-1 shadow-md z-40 songdo-theme-item-box2">
                            <span>송도해수욕장</span>
                            <span class="songdo-theme-sub-icon">🌊</span>
                        </div>
                    </div>

                    <div id="timer-layer-2" class="timer-overlay hidden">
                        <span id="timer-text-2" class="timer-number">5</span>
                    </div>

                    <div class="absolute inset-0 flex flex-col items-center justify-center text-slate-400 text-center p-3 z-0 empty-tip">
                        <span class="text-xl">📸</span>
                        <span class="text-[10px] font-bold mt-1">2번째 컷 촬영 영역</span>
                    </div>
                    <div class="absolute inset-0 z-40 pointer-events-auto sticker-area"></div>
                </div>

                <!-- Slot 3 -->
                <div onclick="setActiveSlot(3)" id="slot-3" class="photo-slot">
                    <video id="live-stream-3" autoplay playsinline muted class="camera-preview-element hidden"></video>
                    <canvas id="slot-canvas-3" class="captured-snapshot-canvas"></canvas>
                    
                    <div class="songdo-design-overlay">
                        <div class="absolute top-2 left-2 bg-sky-600/95 text-white px-2 py-0.5 rounded-full text-[10px] font-bold flex items-center gap-1 shadow-md z-40 songdo-theme-item-box">
                            <span class="songdo-theme-icon">🚀</span>
                            <span class="songdo-theme-text font-serif">SONGDO</span>
                        </div>
                        <div class="absolute bottom-2 right-2 bg-sky-900/95 text-white px-1.5 py-0.5 rounded text-[8.5px] font-bold tracking-wider flex items-center gap-1 shadow-md z-40 songdo-theme-item-box2">
                            <span>송도해수욕장</span>
                            <span class="songdo-theme-sub-icon">🌊</span>
                        </div>
                    </div>

                    <div id="timer-layer-3" class="timer-overlay hidden">
                        <span id="timer-text-3" class="timer-number">5</span>
                    </div>

                    <div class="absolute inset-0 flex flex-col items-center justify-center text-slate-400 text-center p-3 z-0 empty-tip">
                        <span class="text-xl">📸</span>
                        <span class="text-[10px] font-bold mt-1">3번째 컷 촬영 영역</span>
                    </div>
                    <div class="absolute inset-0 z-40 pointer-events-auto sticker-area"></div>
                </div>

                <!-- Slot 4 -->
                <div onclick="setActiveSlot(4)" id="slot-4" class="photo-slot">
                    <video id="live-stream-4" autoplay playsinline muted class="camera-preview-element hidden"></video>
                    <canvas id="slot-canvas-4" class="captured-snapshot-canvas"></canvas>
                    
                    <div class="songdo-design-overlay">
                        <div class="absolute top-2 left-2 bg-sky-600/95 text-white px-2 py-0.5 rounded-full text-[10px] font-bold flex items-center gap-1 shadow-md z-40 songdo-theme-item-box">
                            <span class="songdo-theme-icon">🚀</span>
                            <span class="songdo-theme-text font-serif">SONGDO</span>
                        </div>
                        <div class="absolute bottom-2 right-2 bg-sky-900/95 text-white px-1.5 py-0.5 rounded text-[8.5px] font-bold tracking-wider flex items-center gap-1 shadow-md z-40 songdo-theme-item-box2">
                            <span>송도해수욕장</span>
                            <span class="songdo-theme-sub-icon">🌊</span>
                        </div>
                    </div>

                    <div id="timer-layer-4" class="timer-overlay hidden">
                        <span id="timer-text-4" class="timer-number">5</span>
                    </div>

                    <div class="absolute inset-0 flex flex-col items-center justify-center text-slate-400 text-center p-3 z-0 empty-tip">
                        <span class="text-xl">📸</span>
                        <span class="text-[10px] font-bold mt-1">4번째 컷 촬영 영역</span>
                    </div>
                    <div class="absolute inset-0 z-40 pointer-events-auto sticker-area"></div>
                </div>

                <!-- Footer Base -->
                <div class="w-full text-center py-2 flex flex-col items-center justify-center">
                    <span id="footer-text" class="text-sky-950 font-bold text-sm tracking-wider flex items-center justify-center gap-1">
                        🌊 2026.06.20 부산관광고등학교 🎓
                    </span>
                    <p class="text-[9px] text-gray-400 font-bold tracking-wide">Songdo Parent-Student Day 2026</p>
                </div>

            </div>
        </section>

    </main>

    <!-- 팝업 모달창 (QRCode 최적화 라이브러리 탑재) -->
    <div id="qr-modal" class="fixed inset-0 bg-black/85 z-50 hidden flex items-center justify-center p-4">
        <div class="bg-white rounded-3xl p-6 max-w-sm w-full text-center shadow-2xl relative">
            <button onclick="closeQrModal()" class="absolute top-4 right-4 text-gray-400 hover:text-gray-600 text-2xl transition">
                <i class="fa-solid fa-xmark"></i>
            </button>
            <div class="w-12 h-12 bg-emerald-100 text-emerald-600 rounded-full flex items-center justify-center mx-auto mb-3 text-xl animate-bounce">
                <i class="fa-solid fa-circle-check"></i>
            </div>
            <h3 class="font-bold text-gray-900 text-xl mb-1">인생네컷 QR 생성 완료!</h3>
            <p class="text-xs text-gray-500 mb-4 leading-relaxed">
                스마트폰 카메라로 아래 QR코드를 비추면 **고해상도 이미지 다운로드 링크**로 연결되어 즉시 기기에 소장할 수 있습니다!
            </p>
            
            <!-- 진짜 출력되는 스캔율 100% QR코드 컨테이너 -->
            <div class="flex justify-center bg-sky-50 p-4 rounded-2xl mb-4 border border-sky-100 shadow-inner">
                <div id="qrcode-canvas" class="p-2 bg-white rounded-lg shadow-sm border border-slate-200">
                    <!-- 스크립트에서 자동 생성함 -->
                </div>
            </div>
            <p class="text-[10px] text-rose-500 font-bold mb-4">💡 폰 브라우저에 이미지가 로드되면 이미지를 길게 눌러 바로 저장하세요!</p>
            <button onclick="closeQrModal()" class="w-full bg-slate-900 hover:bg-slate-800 text-white font-bold py-3 rounded-xl text-xs transition">
                화면 닫기
            </button>
        </div>
    </div>

    <!-- UI Message Toast -->
    <div id="toast" class="fixed bottom-6 left-1/2 transform -translate-x-1/2 z-50 bg-slate-900/95 text-white px-5 py-2.5 rounded-xl shadow-lg border border-slate-700 hidden flex items-center gap-2 max-w-sm w-full mx-4">
        <span id="toast-icon" class="text-sky-400 text-lg"></span>
        <div class="flex-1">
            <h4 id="toast-title" class="font-bold text-xs"></h4>
            <p id="toast-msg" class="text-[11px] text-slate-300 mt-0.5"></p>
        </div>
    </div>

    <!-- Confetti layer -->
    <div id="confetti-container" class="fixed inset-0 pointer-events-none z-50 hidden"></div>

    <footer class="w-full text-center text-[11px] text-sky-800/60 mt-10">
        <p>© 2026 부산관광고등학교 - 중3 학부모 초청의 날 기념</p>
    </footer>

    <!-- html2canvas & qrcode.js 공식 릴리즈 라이브러리 탑재 -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>

    <script>
        const themeDesigns = {
            'theme-cable': { leftEmoji: '🚠', textLabel: 'CABLE CAR', subIcon: '🌊', placeLabel: '송도 해수욕장' },
            'theme-beach': { leftEmoji: '⛱️', textLabel: 'SAND BEACH', subIcon: '🦀', placeLabel: '은빛 모래해변' },
            'theme-lighthouse': { leftEmoji: '🚨', textLabel: 'LIGHTHOUSE', subIcon: '⛵', placeLabel: '빨간 등대 방파제' },
            'theme-cloud': { leftEmoji: '🌉', textLabel: 'SKY WALK', subIcon: '🐬', placeLabel: '구름 산책로' }
        };
        let currentThemeKey = 'theme-cable';

        let activeSlotIndex = 1;
        let hardwareStream = null;
        let isHardwareConnected = false;
        let isTimerRunning = false;
        let isSequentialShooting = false; 

        window.addEventListener('load', () => {
            selectSongdoTheme('theme-cable');
            requestHardwareCamera();
        });

        // 탭 전환 기능
        function switchTab(tabName) {
            document.getElementById('panel-frame').classList.add('hidden');
            document.getElementById('panel-camera').classList.add('hidden');
            document.getElementById('panel-sticker').classList.add('hidden');

            const tabs = ['frame', 'camera', 'sticker'];
            tabs.forEach(t => {
                const btn = document.getElementById(`tab-${t}`);
                btn.classList.remove('text-sky-600', 'border-sky-600');
                btn.classList.add('text-gray-500', 'border-transparent');
            });

            document.getElementById(`panel-${tabName}`).classList.remove('hidden');
            const activeBtn = document.getElementById(`tab-${tabName}`);
            activeBtn.classList.add('text-sky-600', 'border-sky-600');
            activeBtn.classList.remove('text-gray-500', 'border-transparent');

            const step1 = document.getElementById('step1-tab');
            const step2 = document.getElementById('step2-tab');
            const step3 = document.getElementById('step3-tab');
            step1.className = step2.className = step3.className = 'flex items-center gap-1.5 text-gray-400';

            if (tabName === 'frame') {
                step1.className = 'flex items-center gap-1.5 text-sky-600 font-bold';
                deactivateCameraPreviews();
            } else if (tabName === 'camera') {
                step1.className = 'flex items-center gap-1.5 text-sky-800 font-medium';
                step2.className = 'flex items-center gap-1.5 text-sky-600 font-bold';
                activateCameraPreviews(); 
            } else if (tabName === 'sticker') {
                step1.className = 'flex items-center gap-1.5 text-sky-800 font-medium';
                step2.className = 'flex items-center gap-1.5 text-sky-800 font-medium';
                step3.className = 'flex items-center gap-1.5 text-emerald-600 font-bold';
                deactivateCameraPreviews();
            }
        }

        // 테두리 색상 전환
        function changeFrameColor(bgColor, textColor) {
            const frame = document.getElementById('four-cuts-frame');
            const footerText = document.getElementById('footer-text');
            frame.style.backgroundColor = bgColor;
            frame.style.borderColor = bgColor;
            footerText.className = `${textColor} font-bold text-sm tracking-wider flex items-center justify-center gap-1`;
            showToast('🎨 컬러 변경', '인생네컷 테두리 색상이 변경되었습니다.', 'fa-solid fa-magic');
        }

        // 송도 배지 선택 및 동적 가이드 바인딩
        function selectSongdoTheme(themeKey) {
            currentThemeKey = themeKey;
            const chosenTheme = themeDesigns[themeKey];

            for (let i = 1; i <= 4; i++) {
                const slot = document.getElementById(`slot-${i}`);
                const iconBox = slot.querySelector('.songdo-theme-item-box');
                const badgeBox = slot.querySelector('.songdo-theme-item-box2');

                if (iconBox) {
                    iconBox.querySelector('.songdo-theme-icon').innerText = chosenTheme.leftEmoji;
                    iconBox.querySelector('.songdo-theme-text').innerText = chosenTheme.textLabel;
                }
                if (badgeBox) {
                    badgeBox.querySelector('span').innerText = chosenTheme.placeLabel;
                    badgeBox.querySelector('.songdo-theme-sub-icon').innerText = chosenTheme.subIcon;
                }
            }

            const types = ['cable', 'beach', 'lighthouse', 'cloud'];
            types.forEach(t => {
                const btn = document.getElementById(`theme-btn-${t}`);
                if (btn) btn.className = "flex flex-col items-center p-2 rounded-xl border border-gray-200 hover:border-sky-300 transition text-center text-xs";
            });
            
            const activeBtn = document.getElementById(`theme-btn-${themeKey.split('-')[1]}`);
            if (activeBtn) activeBtn.className = "flex flex-col items-center p-2 rounded-xl border-2 border-sky-400 bg-sky-50/50 hover:bg-sky-50 transition text-center text-xs";
        }

        // 웹캠 기기 감지 및 초기화
        async function requestHardwareCamera() {
            try {
                if (hardwareStream) {
                    hardwareStream.getTracks().forEach(track => track.stop());
                }

                const setup = {
                    audio: false,
                    video: {
                        facingMode: 'user',
                        width: { ideal: 640 },
                        height: { ideal: 480 }
                    }
                };

                hardwareStream = await navigator.mediaDevices.getUserMedia(setup);
                isHardwareConnected = true;

                if (!document.getElementById('panel-camera').classList.contains('hidden')) {
                    activateCameraPreviews();
                }
            } catch (err) {
                console.warn("로컬 카메라 하드웨어 없음 - 파일 업로드 우선 적용", err);
                isHardwareConnected = false;
            }
        }

        // 실시간 카메라 비디오 재생 연동
        function activateCameraPreviews() {
            deactivateCameraPreviews();

            if (!isHardwareConnected) {
                showToast('🚨 수동 연동 모드', '카메라 하드웨어가 감지되지 않았습니다. 사진 불러오기로 대체합니다.', 'fa-solid fa-file-image text-amber-500');
                return;
            }

            const activeVideo = document.getElementById(`live-stream-${activeSlotIndex}`);
            if (activeVideo) {
                activeVideo.srcObject = hardwareStream;
                activeVideo.classList.remove('hidden');
                activeVideo.play().catch(err => console.warn(err));
                
                const activeCanvas = document.getElementById(`slot-canvas-${activeSlotIndex}`);
                if (activeCanvas) activeCanvas.classList.add('hidden');
            }
        }

        function deactivateCameraPreviews() {
            for (let i = 1; i <= 4; i++) {
                const vid = document.getElementById(`live-stream-${i}`);
                if (vid) {
                    vid.classList.add('hidden');
                    vid.pause();
                }
            }
        }

        // 특정 슬롯 지정
        function setActiveSlot(index) {
            activeSlotIndex = index;

            for (let i = 1; i <= 4; i++) {
                const slot = document.getElementById(`slot-${i}`);
                const btn = document.getElementById(`slot-btn-${i}`);
                slot.classList.remove('active-slot');
                btn.className = "py-1.5 rounded-lg text-xs font-bold bg-white text-gray-600 border border-gray-200 transition";
            }

            document.getElementById(`slot-${index}`).classList.add('active-slot');
            document.getElementById(`slot-btn-${index}`).className = "py-1.5 rounded-lg text-xs font-bold bg-sky-600 text-white border-2 border-sky-600 transition shadow-sm";

            if (!document.getElementById('panel-camera').classList.contains('hidden') && !isSequentialShooting) {
                activateCameraPreviews();
            }
        }

        // 자동 4연속 찰칵 시작
        function startTimerAndCapture() {
            if (isTimerRunning || isSequentialShooting) return;
            
            if (!isHardwareConnected) {
                showToast('⚠️ 실시간 카메라 필요', '웹캠 또는 카메라 연결 권한이 필요합니다.', 'fa-solid fa-video-slash');
                return;
            }

            isSequentialShooting = true;
            runNextSequentialShot(1);
        }

        function runNextSequentialShot(slotIndex) {
            if (slotIndex > 4) {
                isSequentialShooting = false;
                
                const startBtn = document.getElementById('start-capture-btn');
                startBtn.disabled = false;
                startBtn.classList.remove('opacity-50', 'cursor-not-allowed');

                deactivateCameraPreviews();
                
                // 완벽한 촬영본 정지화면 확보 후 원터치 QR생성기로 진입
                setTimeout(() => {
                    generateHighQualityQR();
                }, 800);
                return;
            }

            setActiveSlot(slotIndex);
            
            const activeVideo = document.getElementById(`live-stream-${slotIndex}`);
            const activeCanvas = document.getElementById(`slot-canvas-${slotIndex}`);
            
            if (activeVideo) {
                activeVideo.srcObject = hardwareStream;
                activeVideo.classList.remove('hidden');
                activeVideo.play().catch(e => console.warn(e));
            }
            if (activeCanvas) activeCanvas.classList.add('hidden');

            const timerLayer = document.getElementById(`timer-layer-${slotIndex}`);
            const timerText = document.getElementById(`timer-text-${slotIndex}`);
            const startBtn = document.getElementById('start-capture-btn');

            startBtn.disabled = true;
            startBtn.classList.add('opacity-50', 'cursor-not-allowed');

            timerLayer.classList.remove('hidden');
            let countdown = 5;
            timerText.innerText = countdown;

            showToast('⏰ 연속 촬영 구동', `${slotIndex}번 슬롯의 촬영 타이머(5초) 작동 시작`, 'fa-solid fa-clock text-sky-500');

            const timerInterval = setInterval(() => {
                countdown--;
                if (countdown > 0) {
                    timerText.innerText = countdown;
                } else {
                    clearInterval(timerInterval);
                    timerLayer.classList.add('hidden');
                    
                    executePhotoCaptureForSlot(slotIndex);

                    setTimeout(() => {
                        runNextSequentialShot(slotIndex + 1);
                    }, 1200);
                }
            }, 1000);
        }

        // 비디오 스냅샷 변환 드로잉
        function executePhotoCaptureForSlot(slotId) {
            const activeVideo = document.getElementById(`live-stream-${slotId}`);
            const canvas = document.getElementById(`slot-canvas-${slotId}`);
            const ctx = canvas.getContext('2d');
            const slot = document.getElementById(`slot-${slotId}`);

            canvas.width = 640;
            canvas.height = 480;

            ctx.save();
            ctx.translate(canvas.width, 0);
            ctx.scale(-1, 1);
            ctx.drawImage(activeVideo, 0, 0, canvas.width, canvas.height);
            ctx.restore();

            canvas.setAttribute('data-taken', 'true');
            canvas.classList.remove('hidden');
            activeVideo.classList.add('hidden');

            const tip = slot.querySelector('.empty-tip');
            if (tip) tip.classList.add('hidden');

            // 찰칵 플래시 이펙트
            slot.style.opacity = '0.3';
            setTimeout(() => { slot.style.opacity = '1'; }, 100);
        }

        // 파일 직접 가져오기 구현
        function triggerFileSelect() {
            document.getElementById('file-uploader').click();
        }

        function handleFileSelect(event) {
            const file = event.target.files[0];
            if (!file) return;

            const reader = new FileReader();
            reader.onload = function(e) {
                const canvas = document.getElementById(`slot-canvas-${activeSlotIndex}`);
                const ctx = canvas.getContext('2d');
                const slot = document.getElementById(`slot-${activeSlotIndex}`);

                const uploadedImg = new Image();
                uploadedImg.src = e.target.result;
                uploadedImg.onload = () => {
                    canvas.width = 640;
                    canvas.height = 480;
                    ctx.drawImage(uploadedImg, 0, 0, canvas.width, canvas.height);

                    canvas.setAttribute('data-taken', 'true');
                    canvas.classList.remove('hidden');

                    const activeVideo = document.getElementById(`live-stream-${activeSlotIndex}`);
                    if (activeVideo) activeVideo.classList.add('hidden');

                    const tip = slot.querySelector('.empty-tip');
                    if (tip) tip.classList.add('hidden');

                    showToast('📂 수동 파일 연동', `${activeSlotIndex}번 컷이 변경되었습니다.`, 'fa-solid fa-image');
                };
            };
            reader.readAsDataURL(file);
        }

        // 스티커 드래그 레이어 빌딩
        function addSticker(emoji) {
            const activeSlot = document.getElementById(`slot-${activeSlotIndex}`);
            const stickerArea = activeSlot.querySelector('.sticker-area');

            const sticker = document.createElement('div');
            sticker.className = 'sticker-3d select-none cursor-pointer';
            sticker.innerHTML = emoji;
            sticker.style.left = '35%';
            sticker.style.top = '35%';

            bindDragAndTouchEvents(sticker, stickerArea);
            stickerArea.appendChild(sticker);
            showToast('🧸 스티커 장착', '스티커를 드래그해 자유롭게 배치해 보세요.', 'fa-solid fa-face-smile');
        }

        function bindDragAndTouchEvents(elem, container) {
            let startX = 0, startY = 0, initialLeft = 0, initialTop = 0;

            elem.addEventListener('touchstart', (e) => {
                const touch = e.touches[0];
                startX = touch.clientX;
                startY = touch.clientY;
                initialLeft = elem.offsetLeft;
                initialTop = elem.offsetTop;
                document.addEventListener('touchmove', handleTouchMove, { passive: false });
                document.addEventListener('touchend', handleDragEnd);
            }, { passive: true });

            elem.addEventListener('mousedown', (e) => {
                e.preventDefault();
                startX = e.clientX;
                startY = e.clientY;
                initialLeft = elem.offsetLeft;
                initialTop = elem.offsetTop;
                document.addEventListener('mousemove', handleMouseMove);
                document.addEventListener('mouseup', handleDragEnd);
            });

            function handleTouchMove(e) {
                if (e.cancelable) e.preventDefault();
                const touch = e.touches[0];
                const dx = touch.clientX - startX;
                const dy = touch.clientY - startY;
                elem.style.left = (initialLeft + dx) + 'px';
                elem.style.top = (initialTop + dy) + 'px';
            }

            function handleMouseMove(e) {
                const dx = e.clientX - startX;
                const dy = e.clientY - startY;
                elem.style.left = (initialLeft + dx) + 'px';
                elem.style.top = (initialTop + dy) + 'px';
            }

            function handleDragEnd() {
                document.removeEventListener('touchmove', handleTouchMove);
                document.removeEventListener('touchend', handleDragEnd);
                document.removeEventListener('mousemove', handleMouseMove);
                document.removeEventListener('mouseup', handleDragEnd);
            }
        }

        function clearStickersInActive() {
            const activeSlot = document.getElementById('slot-' + activeSlotIndex);
            const area = activeSlot.querySelector('.sticker-area');
            area.innerHTML = '';
            showToast('🧹 스티커 제거', `${activeSlotIndex}번 컷 스티커를 정리했습니다.`, 'fa-solid fa-trash-can');
        }

        // 🌟 [핵심] 초고속 이미지 클라우드 업로드 및 실시간 QR코드 생성 🌟
        function generateHighQualityQR() {
            const frame = document.getElementById('four-cuts-frame');
            showToast('🔄 이미지 병합 인코딩', '인생네컷을 한 장으로 패키징하는 중...', 'fa-solid fa-sync fa-spin');

            // html2canvas 옵션을 단순화 및 안전화 처리하여 데이터오염 차단
            html2canvas(frame, { 
                useCORS: true, 
                allowTaint: false, 
                scale: 1.5,
                backgroundColor: null
            }).then(canvas => {
                canvas.toBlob(function(blob) {
                    if (!blob) {
                        showToast('❌ 병합 에러', '이미지 병합을 실패했습니다. 사진을 먼저 올려주세요.', 'fa-solid fa-times-circle');
                        return;
                    }

                    showToast('📡 보안 클라우드 전송', '인생네컷 전송 링크를 발급받고 있습니다...', 'fa-solid fa-cloud-arrow-up text-indigo-400');

                    // 무료 익명 스토리지 API 브릿지 활용 (CORS 완벽 허용 및 24시간 저장 보장)
                    const file = new File([blob], "fourcuts.png", { type: "image/png" });
                    const formData = new FormData();
                    formData.append('file', file);

                    fetch('https://tmpfiles.org/api/v1/upload', {
                        method: 'POST',
                        body: formData
                    })
                    .then(res => {
                        if (!res.ok) throw new Error("전송 채널 불안정");
                        return res.json();
                    })
                    .then(result => {
                        // 결과에서 업로드된 고유 다운로드 URL 추출
                        if (result.data && result.data.url) {
                            // tmpfiles.org 형식 수정 (다운로드 다이렉트 주소로 전환)
                            const directDownloadUrl = result.data.url.replace("https://tmpfiles.org/", "https://tmpfiles.org/dl/");
                            
                            // QR코드 그리기 영역 초기화 및 렌더링
                            const container = document.getElementById('qrcode-canvas');
                            container.innerHTML = "";
                            
                            new QRCode(container, {
                                text: directDownloadUrl,
                                width: 180,
                                height: 180,
                                colorDark: "#0f172a",
                                colorLight: "#ffffff",
                                correctLevel: QRCode.CorrectLevel.M
                            });

                            // 모달 가동
                            document.getElementById('qr-modal').classList.remove('hidden');
                            triggerConfettiFX();
                            showToast('🎉 QR코드 스캔 활성화', '스마트폰 카메라로 즉시 스캔하여 다운로드 하세요!', 'fa-solid fa-qrcode text-green-500');
                        } else {
                            throw new Error("반환값 누락");
                        }
                    })
                    .catch(err => {
                        console.error(err);
                        showToast('❌ 네트워크 오류', '클라우드 업로드 대역폭이 불안정합니다. 임시 갤러리 저장을 시도하세요.', 'fa-solid fa-circle-exclamation');
                        
                        // 백업용 팝업 로컬 다운로드 연동
                        const backupUrl = canvas.toDataURL('image/png');
                        const container = document.getElementById('qrcode-canvas');
                        container.innerHTML = `<img src="${backupUrl}" class="w-full h-auto rounded-xl shadow-md border" alt="Backup Captured image" />`;
                        document.getElementById('qr-modal').classList.remove('hidden');
                    });
                }, 'image/png');
            }).catch(e => {
                console.error("html2canvas 실행 중단", e);
                showToast('❌ 시스템 장애', '인쇄 캔버스 자원에 접근할 수 없습니다.', 'fa-solid fa-triangle-exclamation');
            });
        }

        // 🌟 기기 다운로드 및 갤러리 저장 모듈 완벽 보강 🌟
        function saveToGalleryLocal() {
            const frame = document.getElementById('four-cuts-frame');
            showToast('💾 갤러리 내보내기', '스마트폰 앨범용 변환 프로세스 구동 중...', 'fa-solid fa-file-export');

            html2canvas(frame, { 
                useCORS: true, 
                allowTaint: false, 
                scale: 2 
            }).then(canvas => {
                try {
                    // 크롬북 및 크롬 모바일 브라우저의 전송 다운로드 프로토콜 완벽 매핑
                    const dataUrl = canvas.toDataURL('image/png');
                    const link = document.createElement('a');
                    link.download = `BusanTour_FourCuts_${Date.now()}.png`;
                    link.href = dataUrl;
                    
                    // 신규 하이퍼링크 객체를 가상DOM 트리에 연결해 크래시 에러 극복
                    document.body.appendChild(link);
                    link.click();
                    document.body.removeChild(link);

                    showToast('💾 파일 저장 성공', '스마트폰 및 PC 갤러리에 성공적으로 파일이 다운로드되었습니다!', 'fa-solid fa-circle-check');
                } catch(err) {
                    console.error("다운로드 에러 발생", err);
                    // 아이폰 사파리 등 특수 기기용 새 탭 브릿지 연동
                    const win = window.open();
                    win.document.write(`<img src="${canvas.toDataURL('image/png')}" style="max-width:100%;" />`);
                    showToast('📱 임시 보관 브릿지 개방', '열린 화면의 이미지를 꾹 누르시면 갤러리로 즉시 복사됩니다!', 'fa-solid fa-circle-info');
                }
            });
        }

        function closeQrModal() {
            document.getElementById('qr-modal').classList.add('hidden');
        }

        // 알림창 팝업 도구
        function showToast(title, message, iconClass) {
            const t = document.getElementById('toast');
            document.getElementById('toast-title').innerText = title;
            document.getElementById('toast-msg').innerText = message;
            document.getElementById('toast-icon').className = `${iconClass}`;

            t.classList.remove('hidden');
            setTimeout(() => {
                t.classList.add('hidden');
            }, 4500);
        }

        // 화려한 꽃가루 연출
        function triggerConfettiFX() {
            const container = document.getElementById('confetti-container');
            container.classList.remove('hidden');
            container.innerHTML = '';

            const colors = ['#f43f5e', '#3b82f6', '#10b981', '#fbbf24', '#a855f7'];
            for (let i = 0; i < 40; i++) {
                const p = document.createElement('div');
                p.className = 'absolute w-3 h-3 rounded-full';
                p.style.backgroundColor = colors[Math.floor(Math.random() * colors.length)];
                p.style.left = Math.random() * 100 + 'vw';
                p.style.top = '-10px';
                container.appendChild(p);
                
                p.animate([
                    { top: '-10px', transform: 'scale(0.5) rotate(0deg)' },
                    { top: '100vh', transform: `scale(1) rotate(${Math.random() * 360}deg)` }
                ], {
                    duration: 1000 + Math.random() * 1500,
                    iterations: 1
                }).onfinish = () => p.remove();
            }
        }
    </script>
</body>
</html>

```
