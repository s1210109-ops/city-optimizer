<!DOCTYPE html>
<html lang="zh-Hant">
<head>
    <meta charset="UTF-8">
    <title>女主角滑動切換DEMO</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        body {
            background: linear-gradient(135deg, #10192B 0%, #284B63 100%);
            margin: 0;
            height: 100vh;
            touch-action: pan-y;
            overflow: hidden;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
        }
        #goddess-stage {
            display: flex;
            width: 100vw;
            height: 60vh;
            align-items: center;
            justify-content: center;
            overflow: visible;
        }
        .goddess-img {
            border-radius: 25px;
            box-shadow: 0 9px 40px #00fff977, 0 2px 18px #2229;
            transition: transform 0.6s cubic-bezier(.55,1.5,.32,1), box-shadow 0.4s;
            width: 270px;
            height: 360px;
            object-fit: cover;
            user-select: none;
            background: #132255;
            border: 4px solid #fff;
            position: relative;
            z-index: 1;
            animation: float 2.2s infinite alternate;
        }
        @keyframes float { 0% { transform: translateY(0);} 100% { transform: translateY(-22px);} }

        .goddess-name {
            font-size: 2.1rem;
            color: #18fdfa;
            font-weight: bold;
            text-align: center;
            margin: 24px 0 0 0;
            letter-spacing: 4px;
            text-shadow: 0 2px 20px #00ffffcc;
            transition: color 0.25s;
        }
        #slider-bar {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 20px;
            margin-top: 25px;
        }
        .avatar-btn {
            width: 55px; height: 55px;
            border-radius: 50%;
            border: 2.2px solid #44f1ef;
            overflow: hidden;
            background: rgba(0,255,255,0.11);
            box-shadow: 0 0 7px #00fff955;
            transition: border-color 0.2s, box-shadow 0.2s;
            cursor: pointer;
            margin: 0 3px;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 2px;
            outline: none;
        }
        .avatar-btn.selected {
            border: 2.8px solid #ffe273;
            box-shadow: 0 0 16px #ffe27399;
        }
        .avatar-btn img { width: 100%; height: 100%; object-fit: cover; }
        .slider-arrow {
            font-size: 2.7rem;
            color: #ffaa40;
            background: transparent;
            border: none;
            cursor: pointer;
            margin: 0 15px;
            user-select: none;
            transition: color 0.2s;
        }
        .slider-arrow:active { color: #caf8ff; }
        /* 手勢引導（可隱藏） */
        #gesture-tip {
            color: #44fcf3;
            font-size: 1.12rem;
            margin-top: 13px;
            letter-spacing: 2px;
            opacity: 0.62;
        }
    </style>
</head>
<body>
    <div id="goddess-stage"></div>
    <div class="goddess-name" id="goddess-name"></div>
    <div id="slider-bar"></div>
    <div id="gesture-tip">⬅️ 以滑動或點按切換女主 ➡️</div>
<script>
const goddessAvatars = [
    {name: "司小南", img: "1.jpg"},
    {name: "蕾切爾", img: "2.jpg"},
    {name: "娃娃", img: "3.jpg"},
];
let idx = 0;

// 主要顯示
function renderGoddess() {
    const stage = document.getElementById('goddess-stage');
    stage.innerHTML = `<img src="${goddessAvatars[idx].img}" class="goddess-img" draggable="false">`;
    document.getElementById('goddess-name').innerText = goddessAvatars[idx].name;
    // avatars bar
    const bar = document.getElementById('slider-bar');
    bar.innerHTML = `<button class="slider-arrow" onclick="prevGoddess()">&#8592;</button>`;
    goddessAvatars.forEach((g, i)=>{
        bar.innerHTML += `<button class="avatar-btn${i===idx?" selected":""}" onclick="selectGoddess(${i})">
            <img src="${g.img}">
        </button>`;
    });
    bar.innerHTML += `<button class="slider-arrow" onclick="nextGoddess()">&#8594;</button>`;
}
function selectGoddess(i) {
    idx = i;
    renderGoddess();
}
function prevGoddess() {
    idx = (idx-1+goddessAvatars.length)%goddessAvatars.length;
    renderGoddess();
}
function nextGoddess() {
    idx = (idx+1)%goddessAvatars.length;
    renderGoddess();
}
// 滑動手勢
let touchX=0, dx=0;
document.body.addEventListener('touchstart',e=>{ touchX = e.touches[0].clientX; },{passive:true});
document.body.addEventListener('touchmove',e=>{ dx = e.touches[0].clientX-touchX; },{passive:true});
document.body.addEventListener('touchend',()=>{
    if(dx>50) prevGoddess();
    else if(dx<-50) nextGoddess();
    dx=0;
}, {passive:true});

// 滑鼠拖拽也支援
let dragX=0, ddx=0;
document.body.addEventListener('mousedown',e=>{ dragX = e.clientX; });
document.body.addEventListener('mousemove',e=>{ if(dragX) ddx = e.clientX-dragX; });
document.body.addEventListener('mouseup',()=>{
    if(ddx>50) prevGoddess();
    else if(ddx<-50) nextGoddess();
    dragX=0; ddx=0;
});

renderGoddess();
</script>
</body>
</html>
