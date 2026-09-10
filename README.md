# scenti
Scenti로 향수 취향을 찾아보세요
<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>향BTI — 향기 취향 진단</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Nanum+Myeongjo:wght@400;700;800&family=Gowun+Dodum&display=swap" rel="stylesheet">
<style>
  :root{
    --bg:#E4D8C0;
    --bg-2:#D5C6A8;
    --panel:#F3ECDC;
    --line:#C6B698;
    --ink:#33291F;
    --ink-soft:#7A6C58;
    --brass:#96762F;
    --brass-dim:#7A5F22;
    --rose:#9E5B4B;
  }
  *{box-sizing:border-box;}
  html,body{margin:0;padding:0;}
  body{
    background:var(--bg);
    background-image:radial-gradient(130% 85% at 50% 0%, #F0E7D2 0%, var(--bg) 50%, var(--bg-2) 100%);
    color:var(--ink);
    font-family:'Gowun Dodum',system-ui,sans-serif;
    line-height:1.75;
    min-height:100vh;
    -webkit-font-smoothing:antialiased;
  }
  .wrap{max-width:430px;margin:0 auto;padding:28px 20px 64px;}
  h1,h2,h3,.serif{font-family:'Nanum Myeongjo',serif;}

  /* ── 표지 ── */
  .tag{
    position:relative;background:var(--panel);color:var(--ink);
    border:1px solid var(--line);border-radius:3px;
    padding:34px 26px 30px;text-align:center;
    box-shadow:0 12px 28px rgba(90,70,35,.16);
  }
  .tag::after{
    content:"";position:absolute;inset:7px;border:1px solid var(--line);
    border-radius:2px;pointer-events:none;opacity:.6;
  }
  .tag-rule{width:52px;height:1px;background:var(--brass);margin:14px auto;opacity:.7;}
  h1{font-size:44px;font-weight:800;letter-spacing:.06em;margin:4px 0 2px;}
  .tag .sub{font-size:14px;color:var(--ink-soft);margin:0;}
  .tag .lat{font-family:'Nanum Myeongjo',serif;font-size:11px;letter-spacing:.42em;color:var(--brass);margin:0 0 4px;padding-left:.42em;}
  .intro{font-size:14.5px;color:var(--ink-soft);margin:16px 0 0;text-align:left;}

  /* ── 병 ── */
  .stage{display:flex;justify-content:center;margin:26px 0 8px;}
  .stage svg{width:212px;height:auto;filter:drop-shadow(0 12px 16px rgba(95,72,35,.22));}
  .count{text-align:center;font-size:12.5px;letter-spacing:.24em;color:var(--brass);margin:0 0 22px;padding-left:.24em;}

  /* ── 문항 ── */
  .q{font-family:'Nanum Myeongjo',serif;font-size:21px;line-height:1.6;text-align:center;margin:0 0 22px;}
  .choices{display:flex;flex-direction:column;gap:12px;}
  .choice{
    font:inherit;font-size:15.5px;color:var(--ink);text-align:left;
    background:var(--panel);border:1px solid var(--line);border-radius:3px;
    padding:16px 18px;cursor:pointer;width:100%;
    box-shadow:0 4px 10px rgba(90,70,35,.1);
    transition:transform .12s ease, background .12s ease, border-color .12s ease;
  }
  .choice:hover{background:#FBF6EA;border-color:var(--brass);transform:translateY(-2px);}
  .choice:active{transform:translateY(0);}
  .choice:focus-visible{outline:2px solid var(--brass);outline-offset:3px;}

  /* ── 결과 ── */
  .code{
    font-family:'Nanum Myeongjo',serif;font-size:15px;letter-spacing:.55em;
    color:var(--brass);text-align:center;margin:0 0 4px;padding-left:.55em;
  }
  h2{font-size:31px;font-weight:800;text-align:center;margin:0 0 10px;letter-spacing:.01em;}
  .catch{
    font-family:'Nanum Myeongjo',serif;font-size:18px;line-height:1.65;
    color:var(--brass-dim);text-align:center;margin:0 auto 24px;max-width:22em;
  }
  .card{
    background:var(--panel);color:var(--ink);border:1px solid var(--line);border-radius:3px;
    padding:22px 20px;margin:0 0 14px;box-shadow:0 6px 16px rgba(90,70,35,.12);
  }
  .card p{margin:0;font-size:15px;}
  .card p + p{margin-top:12px;}
  .heading{font-family:'Nanum Myeongjo',serif;font-size:15px;font-weight:700;margin:0 0 10px;color:var(--ink);}
  .warn{border-left:3px solid var(--rose);}
  .list{list-style:none;margin:0;padding:0;}
  .list li{font-size:15px;padding:9px 0;border-bottom:1px solid var(--line);}
  .list li:last-child{border-bottom:0;padding-bottom:0;}
  .list .why{display:block;font-size:12.5px;color:var(--ink-soft);line-height:1.5;}
  .rare{text-align:center;font-size:12.5px;color:var(--ink-soft);margin:0 0 18px;}
  .rare b{font-family:'Nanum Myeongjo',serif;color:var(--brass);letter-spacing:.18em;font-weight:400;}
  .pair{display:flex;gap:10px;}
  .pair button{
    flex:1;font:inherit;font-size:14px;color:var(--ink);background:var(--bg);
    border:1px solid var(--line);border-radius:3px;padding:12px 8px;cursor:pointer;
  }
  .pair button:hover{background:#FBF6EA;border-color:var(--brass);}
  .pair button:focus-visible{outline:2px solid var(--brass);outline-offset:2px;}
  .pair b{display:block;font-family:'Nanum Myeongjo',serif;font-size:11px;letter-spacing:.3em;color:var(--brass);padding-left:.3em;}

  .actions{display:flex;gap:10px;margin-top:22px;}
  .btn{
    flex:1;font:inherit;font-size:15px;padding:14px 10px;border-radius:3px;cursor:pointer;
    background:none;color:var(--ink);border:1px solid var(--line);
  }
  .btn.primary{background:var(--brass);border-color:var(--brass);color:#FBF6EA;}
  .btn:hover{border-color:var(--brass-dim);}
  .btn:focus-visible{outline:2px solid var(--brass);outline-offset:3px;}
  .back{background:none;border:0;color:var(--brass);font:inherit;font-size:13.5px;cursor:pointer;padding:0;margin:0 0 16px;}
  .back:focus-visible{outline:2px solid var(--brass);outline-offset:3px;}
  .note{font-size:12.5px;color:var(--ink-soft);text-align:center;margin:22px 0 0;line-height:1.7;}
  @media (prefers-reduced-motion: reduce){*{transition:none !important;}}
</style>
</head>
<body>
<div class="wrap" id="app"></div>

<script>
/* ─────────── 타입 데이터 ─────────── */
const TYPES = {
  WHSN:{name:"난로 앞 꿀단지",catch:"겨울 이불이 사람이 됐다!<br>포근함이 곧 정체성이에요~",
    desc:"곁에 있으면 이유 없이 마음이 놓이는 향을 고릅니다. 향수를 꾸미는 도구가 아니라 덮는 담요로 쓰는 타입이에요.",
    warn:"한여름엔 본인이 먼저 지칩니다. 6~8월용 서브 향수 하나는 꼭 필요해요.",
    rare:2,liquid:"#C98B3A",match:["WLDN","CHDN"],
    perfumes:[["세르주 루텐 위 부아 바니유","달지만 나무 심지가 박혀 있어 늘어지지 않아요"],["톰포드 토바코 바닐","꿀·담뱃잎·바닐라의 교과서"],["딥티크 벤조앙 보엠","수지의 온기가 은은하게 남습니다"]]},
  WHSU:{name:"야경 디저트",catch:"지나간 자리에 디저트 가게가 생긴다!<br>향으로 자기소개를 끝내는 사람~",
    desc:"기억에 남는 걸 두려워하지 않습니다. 향수는 옷이 아니라 이름표라고 생각하는 타입이에요.",
    warn:"좁고 밀폐된 공간에서는 반 뿌림이 예의. 잔향이 사흘 갑니다.",
    rare:2,liquid:"#9C4A34",match:["CHSU","WHDU"],
    perfumes:[["뮈글러 엔젤","구르망 장르를 만든 장본인"],["YSL 블랙 오피움","커피와 바닐라의 밤 버전"],["랑콤 라 뉘 트레조르","자두 잼 같은 달콤함"]]},
  WHDN:{name:"고찰의 향연기",catch:"전생에 절에서 살았나요?<br>향에서 시간의 냄새를 맡는 사람~",
    desc:"달콤함보다 고요함을 고릅니다. 유행을 거의 신경 쓰지 않고, 10년째 같은 향을 쓰고 있을 확률이 가장 높은 타입이에요.",
    warn:"주변에서 향냄새가 난다고 할 때가 있습니다. 정작 본인은 못 느껴요.",
    rare:4,liquid:"#6E5137",match:["WHSN","CHDN"],
    perfumes:[["딥티크 탐 다오","백단의 기준점 같은 향"],["꼼데가르송 인센스 교토","차갑고 마른 향연기"],["아모아쥬 인터루드","수지와 향신료가 층층이 쌓입니다"]]},
  WHDU:{name:"가죽 재킷",catch:"문 열고 들어오는 순간 공기가 정리된다!<br>향으로 기선을 제압하는 타입~",
    desc:"부드러움보다 윤곽을 좋아합니다. 면접·발표·중요한 자리에 무기처럼 챙기는 향이 반드시 하나 있어요.",
    warn:"첫 만남 데이트엔 조금 셉니다. 상대가 괜히 긴장할 수 있어요.",
    rare:3,liquid:"#5A3A32",match:["CHDU","WHSU"],
    perfumes:[["톰포드 옹브레 레더","가죽에 사프란을 얹은 구조"],["바이레도 블랙 사프란","마르고 어두운 가죽"],["메모 아이리시 레더","풀밭 위의 가죽이라는 반전"]]},
  WLSN:{name:"햇살 아래 살구",catch:"과수원에서 낮잠 자다 온 사람!<br>다정함이 향으로 새어 나와요~",
    desc:"과하지 않은 달콤함의 균형점을 정확히 압니다. 향수 뭐 쓰냐는 질문을 가장 많이 듣는 타입 중 하나예요.",
    warn:"잔향이 짧아 하루 두 번 뿌리게 됩니다. 미니 사이즈를 늘 들고 다니세요.",
    rare:3,liquid:"#E0A96D",match:["CLSN","WLSU"],
    perfumes:[["에르메센스 오스만투스 유난","살구와 차의 경계"],["딥티크 필로시코스","무화과 잎·열매·우유"],["아틀리에 코롱 오랑주 상긴","붉은 오렌지의 단맛"]]},
  WLSU:{name:"포근한 니트",catch:"안아주고 싶은 향의 인간화!<br>스킨센트의 신도~",
    desc:"향수 뿌렸냐는 질문에 아니, 그냥 내 냄샌데라고 답하고 싶은 타입. 향을 피부의 연장으로 씁니다.",
    warn:"본인은 30분이면 아무것도 못 맡고 계속 덧뿌리게 됩니다. 두 번이면 충분해요.",
    rare:2,liquid:"#D9B7A5",match:["CLDU","WLSN"],
    perfumes:[["글로시에 유","사람마다 다르게 퍼지는 머스크"],["나르시소 로드리게즈 뮤스크 뉴드","살결에 붙는 파우더 머스크"],["클린 리저브 스킨","이름 그대로 피부 냄새"]]},
  WLDN:{name:"마른 찻잎",catch:"조용한 사람의 조용한 취향!<br>은근한데 알고 보면 제일 까다로워요~",
    desc:"화려한 향은 애초에 후보에도 없습니다. 대신 잘 마른 나뭇잎, 오래된 종이, 식은 홍차의 미묘한 차이를 정확히 구분해내는 타입이에요.",
    warn:"맞는 향수를 찾는 데 남들보다 세 배 걸립니다. 대신 한번 찾으면 평생 씁니다.",
    rare:4,liquid:"#B49A6A",match:["WHDN","CLDN"],
    perfumes:[["아르마니 프리베 떼 뿌르 엉 에떼","마른 찻잎과 시트러스"],["에르메센스 뿌아브르 사마르캉드","후추와 나무의 건조한 결"],["딥티크 오르페옹","종이와 잉크 같은 마무리"]]},
  WLDU:{name:"따뜻한 콘크리트",catch:"미니멀리스트의 최종 형태!<br>덜어낼수록 멋있다는 걸 아는 사람~",
    desc:"옷장에 검정·회색·베이지만 있을 확률이 높습니다. 향도 똑같이 군더더기를 싫어해요.",
    warn:"유행하는 향을 남들보다 먼저 쓰다가, 유행이 시작되면 흥미를 잃습니다.",
    rare:3,liquid:"#A08C72",match:["CLDU","CHDU"],
    perfumes:[["르라보 상탈 33","건조한 백단과 가죽"],["에센트릭 몰리큘스 몰리큘 01","향이라기보다 인상"],["바이레도 슈퍼 시더","삼나무 한 가지로 끝내는 구성"]]},
  CHSN:{name:"젖은 자두",catch:"향으로 서사를 쓰는 사람!<br>예쁜 것보다 강렬한 걸 골라요~",
    desc:"남들이 너무 세다며 내려놓은 백합·튜베로즈를 집어 드는 타입. 향을 분위기가 아니라 사건으로 씁니다.",
    warn:"사무실에서는 손목 한 번만. 회의실 전체가 꽃밭이 됩니다.",
    rare:4,liquid:"#7B4A5E",match:["CHSU","WHDU"],
    perfumes:[["프레데릭 말 카날 플라워","튜베로즈의 최대치"],["로베르트 피게 프라카","고전적인 백색 꽃다발"],["딥티크 도손","같은 튜베로즈를 비누처럼 다듬은 버전"]]},
  CHSU:{name:"크리스탈 잼",catch:"향수계 인기 아이돌 최애!<br>화려하고 달콤한 데엔 이유가 있어요~",
    desc:"대중성과 화려함을 동시에 잡는 감각이 있습니다. 어떤 향을 골라도 실패 확률이 가장 낮은 타입이에요.",
    warn:"같은 향수를 쓴 사람을 하루에 두 번 마주칠 수 있습니다.",
    rare:1,liquid:"#B85C74",match:["WHSU","CLSU"],
    perfumes:[["MFK 바카라 루쥬 540","사프란과 앰버우드의 유리 같은 단맛"],["톰포드 로스트 체리","체리 리큐어 같은 진함"],["지방시 랑떼르디","어두운 꽃과 파출리"]]},
  CHDN:{name:"비 젖은 숲 바닥",catch:"비 온 뒤 흙냄새를 사랑하는 사람!<br>향수보다 자연을 좋아하는 타입~",
    desc:"달지 않고, 밝지 않고, 축축한 초록을 고릅니다. 향에서 계절과 날씨를 읽는 사람이에요.",
    warn:"남들에겐 약간 어른 냄새로 들릴 수 있어요. 신경 쓸 필요는 없습니다.",
    rare:4,liquid:"#4A5C3E",match:["WHDN","CLDN"],
    perfumes:[["샤넬 시코모어","베티버와 향연기"],["아르마니 프리베 베티버 다르마니","깔끔하게 다듬은 뿌리 냄새"],["겔랑 베티버","고전적인 흙과 담배"]]},
  CHDU:{name:"젖은 아스팔트",catch:"향기 취향 세계 1%!<br>확고하고 독특한 니치 애호가~",
    desc:"알데하이드·메탈릭·잉크 같은 노트를 좋다고 말할 수 있는 사람은 정말 소수입니다. 편안함보다 아름다움을 택하는 타입이에요.",
    warn:"선물로 받은 향수의 90%가 취향에 안 맞습니다. 미리 리스트를 알려주세요.",
    rare:5,liquid:"#5B6B72",match:["WHDU","WLDU"],
    perfumes:[["샤넬 No.19","초록 아이리스와 갈바넘"],["프라다 인퓨전 디리스","차가운 파우더 아이리스"],["딥티크 오 드 미네랄","돌과 물의 냄새"]]},
  CLSN:{name:"아침 과수원",catch:"상큼함의 교과서!<br>누구에게나 사랑받는 무해한 매력~",
    desc:"싫어하는 사람을 찾기 어려운 향을 고릅니다. 첫인상이 중요한 자리에서 가장 안전한 타입이에요.",
    warn:"무난함이 지루함이 되는 순간이 옵니다. 그땐 마른 쪽으로 한 칸 옮겨보세요.",
    rare:2,liquid:"#B9C169",match:["WLSN","CLDN"],
    perfumes:[["조말론 넥타린 블로썸 앤 허니","익은 복숭아와 꿀"],["조말론 블랙베리 앤 베이","풋내가 남은 베리"],["엘리자베스 아덴 화이트티 라일락","봄 한 철을 위한 가벼움"]]},
  CLSU:{name:"수영장 옆 캔디",catch:"여름 방학이 사람이 됐다!<br>청량하고 활기찬 하루가 좋아~",
    desc:"밝고 시원하고 쨍한 걸 고릅니다. 향에서 계절감을 가장 중요하게 여기는 타입이에요.",
    warn:"가을·겨울이면 향수가 갑자기 시시해집니다. 계절용을 따로 두세요.",
    rare:1,liquid:"#6FA8B5",match:["CHSU","CLDU"],
    perfumes:[["돌체앤가바나 라이트 블루","사과와 시더의 여름"],["이세이 미야케 로디쎄이","물 냄새의 대명사"],["조말론 우드 세이지 앤 씨 솔트","바닷바람 쪽으로 한 걸음"]]},
  CLDN:{name:"이른 아침 잔디",catch:"풀밭에서 방금 걸어 나온 사람!<br>자연스러움에 진심인 타입~",
    desc:"좋은 향보다 거슬리지 않는 향을 먼저 따집니다. 인공적인 냄새를 감지하는 코가 특히 예민해서, 남들은 못 느끼는 합성 잔향에서 바로 손을 떼요.",
    warn:"마음에 드는 향은 대체로 지속력이 짧습니다. 취향의 대가로 받아들여야 해요.",
    rare:3,liquid:"#8AA97A",match:["WLDN","CHDN"],
    perfumes:[["불가리 오 퍼퓨메 떼 베르","녹차와 시트러스의 원형"],["딥티크 롬브르 단 로","풀밭에서 딴 듯한 베리"],["엘리자베스 아덴 그린티","가볍고 맑은 초록"]]},
  CLDU:{name:"갓 세탁한 셔츠",catch:"살아있는 인간 비누!<br>뽀득뽀득 씻고 활기차게 시작하는 하루가 좋아~",
    desc:"향으로 화려해지기보다 단정해지고 싶은 타입. 침구 냄새, 갓 다린 셔츠, 빨래 널린 베란다를 사랑합니다.",
    warn:"클린 머스크는 본인 코가 제일 먼저 적응합니다. 없어진 게 아니라 익숙해진 거예요.",
    rare:2,liquid:"#B9C6C9",match:["WLSU","WLDU"],
    perfumes:[["바이레도 블랑쉬","비누와 흰 셔츠"],["메종 마르지엘라 레이지 선데이 모닝","햇볕에 말린 침구"],["MFK 아쿠아 유니버셜","맑고 세련된 물비누"]]}
};

/* ─────────── 문항 ─────────── */
const Q = [
 {a:"temp",w:2,t:"향수를 뿌린 나에게서 떠올랐으면 하는 장면은?",x:["해 질 무렵, 나무 마루가 깔린 방","W"],y:["이른 아침, 물기가 남아 있는 정원","C"]},
 {a:"dens",w:2,t:"엘리베이터에 나와 모르는 사람 한 명이 탔다. 이상적인 상황은?",x:["그 사람이 알아채고 향수 이름을 물어본다","H"],y:["아무도 모르고, 내 손목에 코를 대야 맡아진다","L"]},
 {a:"swee",w:2,t:"향에서 단내가 훅 올라올 때 드는 생각은?",x:["포근하고 기분이 좋아진다","S"],y:["니글거려서 손목을 씻고 싶다","D"]},
 {a:"grain",w:2,t:"좋은 향을 맡았을 때 더 자주 드는 생각은?",x:["이 재료, 실제로 본 적 있다","N"],y:["자연엔 이런 냄새 없는데, 근사하다","U"]},
 {a:"temp",w:1,t:"시향지에 적힌 단어 중 먼저 눈이 가는 쪽은?",x:["앰버 · 통카 · 계피 · 인센스","W"],y:["시트러스 · 민트 · 그린 · 아쿠아","C"]},
 {a:"dens",w:1,t:"한 번 쓸 때 뿌리는 양은?",x:["세 번에서 다섯 번, 목과 손목과 옷까지","H"],y:["한두 번, 혹은 옷에 스치듯","L"]},
 {a:"swee",w:1,t:"바닐라·카라멜 같은 디저트 향을 몸에 뿌리는 건?",x:["좋다, 사람을 끌어당기는 향","S"],y:["먹는 냄새를 왜 몸에… 부담스럽다","D"]},
 {a:"grain",w:1,t:"실험실 냄새 같다, 인공적이다라는 평은?",x:["그 순간 탈락","N"],y:["오히려 세련됐다는 뜻","U"]},
 {a:"temp",w:1,t:"샤워 직후에 뿌리고 싶은 향은?",x:["살갗에 온기가 얹히는 느낌","W"],y:["서늘하게 식혀 주는 느낌","C"]},
 {a:"dens",w:1,t:"지속력에 대한 생각은?",x:["아침에 뿌린 향이 밤까지 남아야 제값","H"],y:["서너 시간이면 충분, 필요하면 다시 뿌린다","L"]},
 {a:"swee",w:1,t:"과일 향이라면 어느 쪽?",x:["잼처럼 푹 익은 과일","S"],y:["덜 익어 풋내와 신맛이 도는 과일","D"]},
 {a:"grain",w:1,t:"이상적인 향의 출처는?",x:["풀밭에서 방금 딴 것 같은 향","N"],y:["현실에 없는, 설계된 구조물 같은 향","U"]},
 {a:"temp",w:1,t:"이 향수 참 따뜻하다는 평을 들었다면?",x:["내가 원하던 바로 그 느낌","W"],y:["나한테는 좀 답답할 것 같다","C"]},
 {a:"dens",w:1,t:"이 향수 좀 헤비하다는 말은 나에게?",x:["칭찬에 가깝다","H"],y:["그 순간 탈락","L"]},
 {a:"swee",w:1,t:"향의 마무리로 원하는 것은?",x:["부드럽고 폭신한 잔향","S"],y:["마르고 쌉쌀한 잔향","D"]},
 {a:"grain",w:1,t:"머스크·알데하이드처럼 자연에 없는 깔끔함은?",x:["어딘가 낯설고 붕 뜬 느낌","N"],y:["딱 내가 원하는 세련된 깔끔함","U"]},
 {a:"suf",w:1,t:"확실히 여성스러운 플로럴을 뿌렸을 때?",x:["나한테 잘 어울린다","E"],y:["코스프레 같고 안 어울린다","T"]},
 {a:"suf",w:1,t:"향으로 남기고 싶은 인상은?",x:["화사하고 부드러운","E"],y:["단정하고 중성적인","T"]},
 {a:"suf",w:1,t:"매장에서 발길이 먼저 가는 코너는?",x:["플로럴 · 프루티","E"],y:["우디 · 머스크 · 유니섹스","T"]}
];

/* ─────────── 상태 ─────────── */
let idx = 0;
let score = {W:0,H:0,S:0,N:0,E:0};
let view = "cover";
let shown = null;   // 결과 화면에서 보고 있는 타입
let mine = null;    // 내 진단 결과

const NEUTRAL = "#CFC4AE";

function currentCode(){
  return (score.W>=3?"W":"C")+(score.H>=3?"H":"L")+(score.S>=3?"S":"D")+(score.N>=3?"N":"U");
}
function suffix(){ return score.E>=2 ? "에겐" : "테토"; }
function progress(){ return idx / Q.length; }

function mix(c1,c2,t){
  const p=h=>[1,3,5].map(i=>parseInt(h.substr(i,2),16));
  const [r1,g1,b1]=p(c1),[r2,g2,b2]=p(c2);
  const v=(a,b)=>Math.round(a+(b-a)*t).toString(16).padStart(2,"0");
  return "#"+v(r1,r2)+v(g1,g2)+v(b1,b2);
}

/* ─────────── 향수병 일러스트 ─────────── */
function bottle(code, ratio){
  const wide = code[1] === "H";                 // 묵직한 유형 = 넓은 플라콘 + 유리 마개
  const liquid = TYPES[code] ? TYPES[code].liquid : NEUTRAL;
  const ink = "#51432F";

  const body = wide
    ? "M60,188 Q60,171 77,166 L120,155 L160,155 L203,166 Q220,171 220,188 L220,329 Q220,344 205,344 L75,344 Q60,344 60,329 Z"
    : "M96,198 Q96,180 113,173 L130,162 L150,162 L167,173 Q184,180 184,198 L184,329 Q184,344 169,344 L111,344 Q96,344 96,329 Z";
  const top = wide ? 155 : 162;
  const box = wide ? "0 0 280 400" : "0 0 300 400";

  // 마개 / 아토마이저
  const cap = wide
    ? `<rect x="122" y="134" width="36" height="24" fill="url(#gl)" stroke="${ink}" stroke-width="1.4"/>
       <rect x="115" y="124" width="50" height="12" rx="2" fill="url(#br)"/>
       <path d="M112,124 Q99,105 118,95 Q140,84 162,95 Q181,105 168,124 Z" fill="url(#gl)" stroke="${ink}" stroke-width="1.4"/>
       <path d="M124,116 Q118,104 130,98" fill="none" stroke="#FFFFFF" stroke-width="2.5" stroke-linecap="round" opacity=".6"/>`
    : `<rect x="130" y="146" width="20" height="18" fill="url(#gl)" stroke="${ink}" stroke-width="1.4"/>
       <rect x="125" y="136" width="30" height="11" rx="2" fill="url(#br)"/>
       <rect x="130" y="125" width="20" height="12" rx="2" fill="url(#br)"/>
       <path d="M150,131 Q200,126 213,158" fill="none" stroke="#96762F" stroke-width="3" stroke-linecap="round"/>
       <ellipse cx="228" cy="192" rx="28" ry="32" fill="url(#br)" opacity=".92" stroke="${ink}" stroke-width="1.2"/>
       <ellipse cx="218" cy="180" rx="7" ry="9" fill="#FFFFFF" opacity=".45"/>
       <circle cx="228" cy="226" r="4" fill="#7A5F22"/>
       <g stroke="#7A5F22" stroke-width="1.6" stroke-linecap="round">
         <path d="M224,229 L220,250"/><path d="M228,230 L228,252"/><path d="M232,229 L236,250"/>
       </g>`;

  const lvl = 344 - (344 - top - 8) * ratio;
  const lx = wide ? 96 : 104, lw = wide ? 88 : 72;
  const ly = wide ? 228 : 236, lh = wide ? 76 : 70;
  const cx = lx + lw / 2;
  const motif = code[3] === "N"                  // 자연 = 식물 잔가지 / 도시 = 아르데코 문양
    ? `<g stroke="#96762F" stroke-width="1.2" fill="none" stroke-linecap="round">
         <path d="M${cx},${ly+36} L${cx},${ly+14}"/>
         <path d="M${cx},${ly+30} q-9,-3 -11,-11 q9,1 11,8"/>
         <path d="M${cx},${ly+24} q9,-3 11,-11 q-9,1 -11,8"/>
       </g>`
    : `<g stroke="#96762F" stroke-width="1.2" fill="none" stroke-linecap="round">
         <path d="M${cx-13},${ly+32} l13,-16 l13,16"/>
         <path d="M${cx-13},${ly+24} l13,-16 l13,16"/>
       </g>`;

  return `
  <svg viewBox="${box}" role="img" aria-label="향수병 일러스트">
    <defs>
      <linearGradient id="gl" x1="0" y1="0" x2="1" y2="0">
        <stop offset="0%" stop-color="#FFFFFF" stop-opacity=".62"/>
        <stop offset="42%" stop-color="#FFFFFF" stop-opacity=".14"/>
        <stop offset="80%" stop-color="#FFFFFF" stop-opacity=".34"/>
        <stop offset="100%" stop-color="#6B573C" stop-opacity=".2"/>
      </linearGradient>
      <linearGradient id="br" x1="0" y1="0" x2="1" y2="0">
        <stop offset="0%" stop-color="#E2C481"/><stop offset="45%" stop-color="#BE9C4E"/><stop offset="100%" stop-color="#7A5F22"/>
      </linearGradient>
      <clipPath id="inside"><path d="${body}"/></clipPath>
    </defs>
    ${cap}
    <path d="${body}" fill="url(#gl)"/>
    <g clip-path="url(#inside)">
      <rect x="40" y="${lvl}" width="260" height="${400-lvl}" fill="${liquid}" opacity=".8"/>
      <rect x="40" y="${lvl}" width="260" height="2.5" fill="#FFFFFF" opacity=".5"/>
    </g>
    <path d="${body}" fill="none" stroke="${ink}" stroke-width="1.4"/>
    <rect x="${wide?76:110}" y="${top+46}" width="9" height="${wide?104:96}" rx="4.5" fill="#FFFFFF" opacity=".4"/>
    <g opacity="${ratio>=1?1:0}">
      <rect x="${lx}" y="${ly}" width="${lw}" height="${lh}" rx="2" fill="#F3ECDC" opacity=".96" stroke="${ink}" stroke-width=".8"/>
      <rect x="${lx+5}" y="${ly+5}" width="${lw-10}" height="${lh-10}" rx="1" fill="none" stroke="#96762F" stroke-width=".8"/>
      ${motif}
      <text x="${cx}" y="${ly+(wide?62:58)}" text-anchor="middle" font-family="Nanum Myeongjo, serif"
            font-size="${wide?15:12}" letter-spacing="${wide?3:1.6}" fill="#33291F">${code}</text>
    </g>
  </svg>`;
}

/* ─────────── 화면 ─────────── */
const app = document.getElementById("app");

function render(){
  if(view === "cover") return renderCover();
  if(view === "quiz")  return renderQuiz();
  return renderResult();
}

function renderCover(){
  app.innerHTML = `
    <div class="tag">
      <p class="lat">SCENT TYPE</p>
      <h1>향BTI</h1>
      <div class="tag-rule"></div>
      <p class="sub">19개의 질문으로 찾는 나의 향기 유형</p>
      <p class="intro">온도 · 밀도 · 당도 · 결, 네 개의 축으로 향 취향을 16가지로 나눕니다. 답할 때마다 병에 향이 차오르고, 다 채우면 당신의 유형이 라벨에 새겨져요.</p>
    </div>
    ${stage("CLDN", 0)}
    <div class="actions"><button class="btn primary" id="go">진단 시작하기</button></div>
    <p class="note">이론상 좋아하는 향이 아니라, 실제로 손이 가는 쪽을 고르세요.</p>`;
  document.getElementById("go").onclick = () => { view="quiz"; render(); };
}

function stage(code, ratio){
  return `<div class="stage">${bottle(code, ratio)}</div>`;
}

function renderQuiz(){
  const q = Q[idx];
  app.innerHTML = `
    ${stage(currentCode(), progress())}
    <p class="count">${String(idx+1).padStart(2,"0")} / ${Q.length}</p>
    <p class="q">${q.t}</p>
    <div class="choices">
      <button class="choice" data-v="${q.x[1]}">${q.x[0]}</button>
      <button class="choice" data-v="${q.y[1]}">${q.y[0]}</button>
    </div>
    ${idx>0 ? '<div class="actions"><button class="btn" id="prev">이전 문항</button></div>' : ""}`;

  app.querySelectorAll(".choice").forEach(b => b.onclick = () => {
    const v = b.dataset.v;
    if(score[v] !== undefined) score[v] += Q[idx].w;
    Q[idx].picked = v;
    idx++;
    if(idx >= Q.length){ mine = currentCode(); shown = mine; view = "result"; }
    render();
    window.scrollTo({top:0});
  });
  const p = document.getElementById("prev");
  if(p) p.onclick = () => {
    idx--;
    const v = Q[idx].picked;
    if(v && score[v] !== undefined) score[v] -= Q[idx].w;
    render();
  };
}

function renderResult(){
  const t = TYPES[shown];
  const isMine = shown === mine;
  const stars = "★".repeat(t.rare) + "☆".repeat(5 - t.rare);
  app.innerHTML = `
    ${isMine ? "" : '<button class="back" id="back">← 내 결과로 돌아가기</button>'}
    ${stage(shown, 1)}
    <p class="code">${shown}${isMine ? "-" + suffix() : ""}</p>
    <h2>${t.name}</h2>
    <p class="catch">${t.catch}</p>
    <p class="rare">희귀도 <b>${stars}</b></p>
    <div class="card"><p>${t.desc}</p></div>
    <div class="card warn">
      <p class="heading">한 가지 주의</p>
      <p>${t.warn}</p>
    </div>
    <div class="card">
      <p class="heading">이 향들부터 시향해 보세요</p>
      <ul class="list">
        ${t.perfumes.map(p => `<li>${p[0]}<span class="why">${p[1]}</span></li>`).join("")}
      </ul>
    </div>
    <div class="card">
      <p class="heading">향 궁합이 좋은 유형</p>
      <div class="pair">
        ${t.match.map(m => `<button data-go="${m}"><b>${m}</b>${TYPES[m].name}</button>`).join("")}
      </div>
    </div>
    <div class="actions">
      <button class="btn" id="copy">결과 복사</button>
      <button class="btn primary" id="again">다시 진단하기</button>
    </div>
    <p class="note">한 축만 다른 유형이 다음 향수를 고를 때 성공률이 가장 높은 후보군입니다.</p>`;

  app.querySelectorAll("[data-go]").forEach(b => b.onclick = () => {
    shown = b.dataset.go; render(); window.scrollTo({top:0});
  });
  const back = document.getElementById("back");
  if(back) back.onclick = () => { shown = mine; render(); window.scrollTo({top:0}); };
  document.getElementById("again").onclick = () => {
    idx = 0; score = {W:0,H:0,S:0,N:0,E:0}; Q.forEach(q => delete q.picked);
    view = "cover"; render(); window.scrollTo({top:0});
  };
  document.getElementById("copy").onclick = async (e) => {
    const m = TYPES[mine];
    const txt = `내 향BTI는 ${mine}-${suffix()} ${m.name}\n${m.catch.replace(/<br>/g," ")}\n추천: ${m.perfumes.map(p=>p[0]).join(", ")}`;
    try{ await navigator.clipboard.writeText(txt); e.target.textContent = "복사했어요"; }
    catch{ e.target.textContent = "복사가 안 돼요"; }
    setTimeout(()=>{ e.target.textContent = "결과 복사"; }, 1800);
  };
}

render();
</script>
</body>
</html>
